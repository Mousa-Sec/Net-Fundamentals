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
