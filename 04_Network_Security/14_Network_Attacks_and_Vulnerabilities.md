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

## 7. Spoofing and Poisoning Attacks
Spoofing occurs when a person or device pretends to be another to gain unauthorized access or intercept data. This is a primary driver for **On-path attacks** (formerly Man-in-the-Middle).

### A. ARP Poisoning (Layer 2)
ARP (Address Resolution Protocol) maps an IP address to a physical MAC address. It has **no built-in authentication**, which attackers exploit.
* **The Normal Process:** A device broadcasts "Who has 192.168.1.1?" and the legitimate router responds with its MAC address. The device then stores this in its **ARP Cache**.
* **The Attack:** The attacker sends **unsolicited ARP responses** to a victim machine, claiming to be the gateway (e.g., "I am 192.168.1.1, and here is my MAC").
* **The Result:** The victim updates its ARP cache with the **attacker's MAC address**. 
* **Impact:** All traffic intended for the router is sent to the attacker first. The attacker then forwards the traffic to the real router so the victim never notices the delay.
* **Defense:** **DAI (Dynamic ARP Inspection)** on switches, which validates ARP packets against a trusted database (DHCP Snooping binding table).

### B. DNS Poisoning (Layer 3/7)
DNS Poisoning (or Spoofing) redirects users to a malicious website by providing a fake IP address in response to a domain name query.
* **Methods of Execution:**
  1. **Modifying the Hosts File:** Attackers gain access to a local machine and edit the `hosts` file. Since this file is checked **before** DNS, the local machine will always go to the attacker's IP.
  2. **On-path DNS Redirection:** The attacker sits in the middle and intercepts a DNS request, sending back a fake response before the real DNS server can.
  3. **DNS Server Compromise:** The attacker hacks the DNS server directly and changes the actual records (e.g., changing the IP for `bank.com` to the attacker's server).
* **The Impact:** Users believe they are on a legitimate site (like their bank), but they are actually entering credentials into a fake site owned by the attacker.
* **Defense:** **DNSSEC (Domain Name System Security Extensions)**, which uses digital signatures to verify that the DNS response is legitimate and hasn't been altered.


## 8. On-Path Attacks (Formerly Man-in-the-Middle)
While ARP and DNS poisoning are the *methods*, the **On-path attack** is the *result*. 
* **Intercept & Modify:** The attacker doesn't just watch the traffic; they can actively change it (e.g., changing the recipient's bank account number in a wire transfer request).
* **Invisible Presence:** If done correctly, the attacker is completely transparent. Both endpoints believe they are talking directly to each other.

## 9. Rogue Services
Rogue services are unauthorized servers or devices added to a network that can cause disruption, data theft, or provide backdoor access.

### A. Rogue DHCP Server
DHCP has no built-in security; any device can respond to a DHCP discover broadcast.
* **The Attack:** An unauthorized device begins handing out IP addresses.
* **The Impact:**
  * **Network Disruption:** Providing invalid gateway addresses or duplicate IPs, causing a DoS.
  * **On-Path Initiation:** Setting the "Default Gateway" to the attacker's IP so all traffic flows through them.
* **Defense:**
  * **DHCP Snooping:** An L2 switch feature that identifies "Trusted" ports (where the real DHCP server is) and "Untrusted" ports (users). The switch drops any DHCP *responses* coming from untrusted ports.
  * **AD Authorization:** In Windows environments, only authorized DHCP servers in Active Directory are allowed to start.
* **Remediation:** Physically remove the device and force all clients to `release/renew` their IP leases.

### B. Rogue Access Point (AP)
A physical wireless access point plugged into a wall jack without authorization.
* **The Motivation:** Could be a "helpful" employee trying to get better Wi-Fi or a malicious actor.
* **The Risk:** Opens a wide, unmanaged hole in the network perimeter, often bypassing firewalls and NAC.
* **Defense:**
  * **802.1X (NAC):** Requires the AP itself to authenticate to the switch before the port is enabled.
  * **Wireless Intrusion Prevention Systems (WIPS):** Scans for unauthorized SSIDs.


### C. Wireless Evil Twin
A malicious version of a Rogue AP designed for deception.
* **The Attack:** The attacker configures an AP with the **exact same SSID** and security settings as the legitimate corporate or public Wi-Fi.
* **The Tactics:**
  * **Radio Power:** The attacker increases the broadcast power to "overpower" the real AP, forcing devices to roam to the stronger (evil) signal.
  * **Captive Portals:** Duplicating the login page of a hotel or coffee shop to steal credentials.
* **Defense:** * **Encryption:** Always use a VPN or HTTPS so the attacker sees only encrypted "gibberish."
  * **User Education:** Training users to look for certificate warnings.

## 10. On-Path Attacks (Final Summary)
The video emphasizes that **On-Path (Man-in-the-Middle)** is the *ultimate goal* of many of these attacks (ARP poisoning, Rogue DHCP, Evil Twins).
* **Core Action:** Receive, examine/modify, and forward.
* **Primary Mitigation:** **Encryption at all layers.** Even if the attacker sits in the middle, they cannot read the payload if it is encrypted via TLS/HTTPS or a VPN.

## 11. Social Engineering
Social engineering is the psychological manipulation of people into performing actions or divulging confidential information.

### A. Phishing
The most common social engineering attack, typically delivered via email.
* **The Goal:** To trick the user into clicking a malicious link or downloading an infected attachment.
* **Red Flags:**
  * **Mismatched Domains:** The "From" address doesn't match the actual company (e.g., an email from "Rackspace" coming from an `@icloud.com` address).
  * **Urgency & Threats:** "Your account has been suspended! Click here now to confirm."
  * **Visual Inconsistencies:** Poor grammar, spelling errors, mismatched fonts, or low-resolution logos.
* **Technical Countermeasure:** Check the destination URL by hovering over the link without clicking. Use a sandboxed Virtual Machine (VM) to investigate suspicious links safely.

### B. Shoulder Surfing
An attacker looks over a user's shoulder to view sensitive information on a screen or watch them type a password.
* **Public Risks:** Airports, coffee shops, and airplanes are high-risk environments.
* **Advanced Methods:** Attackers may use binoculars, telescopes, or compromised webcams to view screens from a distance.
* **Prevention:**
  * **Privacy Filters:** Physical screens placed over a monitor that make the display appear black to anyone not sitting directly in front of it.
  * **Environmental Awareness:** Positioning your screen away from windows and sitting with your back to a wall in public.


### C. Physical Access: Tailgating and Piggybacking
These attacks aim to bypass front-door security (electronic locks, badges) to gain access to a physical office.
* **Tailgating:** An unauthorized person follows closely behind an authorized person through a door before it closes, without the authorized person's knowledge.
* **Piggybacking:** An authorized person **knowingly** allows an unauthorized person to enter. 
  * *Tactic:* The attacker may carry boxes of donuts or heavy equipment, prompting a helpful employee to hold the door open for them.
* **Prevention:**
  * **Access Control Vestibule (Airlock):** A mechanical system with two doors where the first must close before the second can open, allowing only one person through at a time.
  * **Corporate Culture:** Training employees to verify badges and challenge anyone they don't recognize.

### D. Dumpster Diving
Searching through a company's trash to find sensitive information that can be used for future attacks.
* **Valuable Items:** Employee names, contact lists, project details, quarterly reports, and internal memos.
* **Legality:** In many jurisdictions (including the U.S.), garbage is considered public property once it is placed on the curb, unless it is on private property with "No Trespassing" signs.
* **Prevention:**
  * **Shredding:** Using cross-cut shredders for all sensitive documents.
  * **Incineration:** Burning sensitive documents (common in government/military).
  * **Security:** Placing garbage bins in fenced, locked, and camera-monitored areas.

## 12. Malware (Objective 4.2)
Malware (Malicious Software) is a broad term for any software designed to disrupt, damage, or gain unauthorized access to a computer system.

### A. Replication & Spread
* **Virus:** Malware that can replicate itself but **requires human intervention** to spread (e.g., clicking a link or opening an infected file).
* **Worm:** A highly dangerous category of malware that replicates and spreads **automatically** across the network without any human interaction. It exploits unpatched system vulnerabilities (e.g., the EternalBlue exploit).

### B. Access & Deception
* **Trojan Horse:** Malware that disguises itself as legitimate software (like a game or utility). Once executed, it performs its malicious task in the background.
* **Rootkit:** A collection of tools designed to gain high-level (root/admin) access while **hiding its presence** from the operating system and antivirus software. It is notoriously difficult to detect and remove.
* **Backdoor:** A method of bypassing normal authentication to gain remote access to a system. Malware often installs a backdoor as its first step to ensure persistent access.

### C. Financial & Data Impact
* **Ransomware:** Encrypts a user's files and demands payment (usually in cryptocurrency) in exchange for the decryption key.
  * *Purple Team Note:* Modern ransomware often includes "double extortion," where data is also exfiltrated to be leaked if the ransom isn't paid.
* **Spyware:** Secretly monitors user activity (web browsing, webcam, microphone) to collect information.
* **Keylogger:** A specific type of spyware that records every keystroke made by the user to capture usernames and passwords.
* **Adware:** Software that automatically displays or downloads advertising material. While often just annoying, it can be a gateway for more malicious infections.

### D. Targeted Execution
* **Logic Bomb:** Malware that lies dormant until a specific condition is met, such as a specific date/time or an action (e.g., an employee's name being removed from a payroll database).
* **Botnet:** A collection of "zombie" computers (Bots) controlled by an attacker's **Command and Control (C2)** server. These are used for massive DDoS attacks or spam campaigns.


### E. Other System Impacts
* **Bloatware:** Unwanted software pre-installed on new computers. While not always malicious, it wastes system resources (CPU, RAM, Storage) and increases the attack surface.

## 13. Malware Defense Best Practices
* **Patching:** Keeping the Operating System and all applications updated is the #1 defense against worms.
* **User Education:** Training users to never click embedded email links or suspicious pop-ups.
* **Least Privilege:** Ensuring users do not have administrative rights prevents many types of malware from installing themselves.

## 14. Vulnerability Types & Security Posture
Attackers don't just "guess"; they look for specific types of weaknesses to exploit.
* **Zero-Day:** A vulnerability that is unknown to the vendor. There is "zero days" of protection available because no patch exists yet.
* **Legacy Systems:** Older hardware or software that is no longer supported by the manufacturer and cannot be patched (e.g., an old Windows XP machine running a hospital's MRI scanner).
* **Misconfigurations:** Human error, such as leaving a default password on a router or leaving a port open on a firewall that should be closed.
* **Unnecessary Services:** Every open port or running service is a potential entry point. If you aren't using Telnet, disable it to reduce the **Attack Surface**.

## 15. Defense in Depth (Layered Security)
Defense in Depth is the strategy of stacking multiple security controls so that if one fails, others are there to stop the attacker.
* **Physical Layer:** Fences, locks, badges, and cameras.
* **Technical Layer:** Firewalls, IPS, Encryption, and MFA.
* **Administrative Layer:** Security policies, employee background checks, and user training.
* **The Human Layer:** The "final wall." Training users to recognize a phishing email before they click it.

## 16. Security Auditing & Assessment
* **Vulnerability Scanning:** Using automated tools (like Nessus) to find known weaknesses in a system without actually exploiting them.
* **Penetration Testing:** A "Red Team" exercise where an ethical hacker (like **EthicalBlitz**) actively tries to exploit vulnerabilities to see how far they can get.
* **Posture Assessment:** A broad look at the entire organization's security to see if policies are being followed and if the defenses are actually effective.
