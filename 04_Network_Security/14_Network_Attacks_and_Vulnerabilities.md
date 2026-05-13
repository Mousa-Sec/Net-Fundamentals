# Day 14: Network Attacks & Vulnerabilities (Objective 4.2)

## 1. Denial of Service (DoS) Fundamentals
A Denial of Service is an action that causes a service to fail by exhausting its resources (CPU, RAM, or Bandwidth).
* **Vulnerability-Based DoS:** Pinpointing a specific weakness in an OS or application to crash it with a single, small packet (this is why patching is critical).
* **Volume-Based DoS:** Overwhelming the system with so much traffic that it cannot process legitimate requests.
* **The "Distraction" Tactic:** Attackers often use a DoS as a "smoke screen." While the security team is busy trying to bring the server back online, the attacker is quietly exfiltrating data from a different part of the network.
* **Friendly DoS (Accidental):**
  * *Switch Loops:* Connecting switches without **STP (Spanning Tree Protocol)** enabled creates a broadcast storm.
  * *Bandwidth Exhaustion:* A single user downloading a massive file (like a Linux ISO) on a limited connection.
* **Physical DoS:** Cutting the main power to a building or a water leak in the data center.

## 2. Distributed Denial of Service (DDoS)
A DDoS occurs when multiple devices act in unison to overwhelm a target.
* **The Botnet:** A collection of compromised devices (Zombies/Bots) managed by a central **Command and Control (C2)** server.
* **Asymmetric Threat:** The attacker has very few resources (one laptop) but can control millions of devices to bring down a massive enterprise with significantly more resources.


## 3. Reflection and Amplification Attacks
This is a high-efficiency attack where a small request results in a massive response, which is then redirected (reflected) to the victim.
* **The Process:**
  1. The attacker (using a botnet) sends a small query to a public service (DNS, NTP, ICMP).
  2. The attacker **spoofs the Source IP** to be the IP address of the **Victim**.
  3. The service sends the massive response to the Victim instead of the attacker.
* **DNS Amplification Example:**
  * A standard DNS query is small (~28 bytes).
  * An attacker requests **DNSSEC keys** using the `dig any` command.
  * The response can be up to **1,300+ bytes** (a 46x amplification).
* **Commonly Abused Protocols:**
  * **NTP (Network Time Protocol):** Used to sync clocks.
  * **DNS (Domain Name System):** Used to resolve names.
  * **ICMP (Ping):** Used for connectivity checks.

## 4. Purple Team Defense Perspective
To defend against these attacks, we use:
* **Anti-DDoS Services:** (e.g., Cloudflare, Akamai) that act as a massive "filter" to absorb the traffic before it hits the company's edge.
* **IPS/Firewall Rate Limiting:** Automatically dropping traffic if a single IP or protocol exceeds a certain threshold.
* **Sinkholing:** Redirecting malicious traffic to a "dead end" (null interface) to protect the rest of the network.

## 5. VLAN Hopping
VLANs are designed to keep network traffic separated at Layer 2. VLAN Hopping is a technique used by attackers to bypass this isolation and send traffic to a different VLAN without passing through a router.

### Method A: Switch Spoofing
This attack exploits the **Dynamic Trunking Protocol (DTP)** which is often enabled by default on switch ports to allow for "Plug-and-Play" networking.
* **The Attack:** The attacker’s machine sends DTP packets to the switch, pretending to be another switch. The switch is "tricked" into negotiating a **Trunk Link** with the attacker.
* **The Result:** Once the trunk is established, the attacker has access to **all VLANs** allowed on that trunk.
* **Defense:**
  * Disable DTP (autonegotiation) on all end-user ports.
  * Manually configure ports as `access` ports and explicitly define trunk ports.


### Method B: Double Tagging
This is a more sophisticated Layer 2 attack that exploits how switches handle the **Native VLAN** and 802.1Q tags.
* **The Requirements:**
  1. The attacker must be connected to a port on the **Native VLAN** (usually VLAN 1 by default).
  2. There must be a trunk link between the attacker's switch and the target's switch.
* **The Attack:**
  1. The attacker crafts a frame with **two 802.1Q tags**. 
  2. The **Outer Tag** matches the Native VLAN of the trunk.
  3. The **Inner Tag** matches the Victim's VLAN.
* **The Flow:**
  1. The first switch receives the frame, sees the Outer Tag matches the Native VLAN, and **strips the tag** before sending it across the trunk.
  2. The second switch receives the frame, sees only the **Inner Tag** remains, and assumes the frame belongs to the Victim's VLAN.
  3. The frame is delivered to the victim.
* **The Catch:** This is **one-way communication only**. The attacker can send data (like a DoS or a malicious command), but the response will not come back to them.
* **Defense:**
  * **Change the Native VLAN** from the default (VLAN 1) to an unused ID.
  * Force **tagging of the Native VLAN** across all trunks so the first switch doesn't strip the outer tag.
  * Move all end-user ports off of the Native VLAN.


## 6. MAC Flooding
This attack targets the **CAM Table (Content Addressable Memory)**, also known as the MAC Address Table, of a network switch.

### The Mechanics of a Switch
* **The Learning Process:** A switch examines the **Source MAC address** of every inbound frame. If the address is new, it records the MAC and the physical port it arrived on in the CAM table.
* **Directed Forwarding:** When a switch knows the destination MAC, it sends the frame *only* to the specific port associated with that MAC.
* **Unicast Flooding (Normal Behavior):** If the switch does **not** know the destination MAC address (it's not in the table), it must "flood" the frame out of every port except the one it arrived on to ensure it reaches its destination.

### The Attack: MAC Flooding
* **The Goal:** To fill up the limited space in the switch's CAM table with thousands of fake, random MAC addresses.
* **The Execution:** An attacker uses a tool (like `macof`) to blast the switch with frames containing randomized source MAC addresses.
* **The "Fail-Open" Result:** Once the CAM table is full, the switch can no longer learn new addresses. It begins to treat **every** incoming frame as an "unknown unicast." 
* **Impact:** The switch effectively turns into a **Hub**. All traffic is broadcast to every port on the switch.
* **Attacker's Benefit:** The attacker can now sit on their own port and use a packet sniffer (like Wireshark) to capture sensitive traffic from *other* conversations that should have been private.


### Defense: Port Security
Modern managed switches have a feature called **Port Security** to prevent this exact attack.
* **MAC Limiting:** You can configure a switch port to only learn a specific number of MAC addresses (e.g., only 1 or 2 per port). If an attacker tries to send 5,000, the port shuts down.
* **Sticky MACs:** The switch "remembers" the first MAC address it sees on a port and ignores or blocks any others.
* **Violation Actions:** You can choose what happens when the limit is reached:
  * **Protect:** Drops traffic from the extra MACs but keeps the port up.
  * **Restrict:** Drops traffic and sends a log message/SNMP trap.
  * **Shutdown:** Disables the entire physical port (requires an admin to manually turn it back on).
