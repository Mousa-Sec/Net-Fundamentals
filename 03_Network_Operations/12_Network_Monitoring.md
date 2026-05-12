

## 1. SNMP (Simple Network Management Protocol)
SNMP is the standard protocol used to monitor and manage network devices (routers, switches, servers) from a central dashboard.
* **The Manager:** The centralized server that collects the data and displays it to the administrator.
* **The Agent:** The software running on the router or switch that gathers the local data.
* **MIB (Management Information Base):** The internal database on the device that stores all the metrics (CPU usage, interface status, routing tables).
* **OIDs (Object Identifiers):** The specific variables queried inside the MIB. They look like long IP addresses (e.g., `1.3.6.1.2.1.2.2.1`).

* **Commands:**
  * *GET:* The Manager actively asks the Agent for a specific metric.
  * *WALK:* The Manager asks the Agent for an entire batch of metrics at once.
  * *TRAP:* An unsolicited alert. The Agent immediately shouts out to the Manager because a critical threshold was crossed (e.g., "Alert: Port G0/1 just went offline!").

### SNMP Versions (Crucial Exam Focus)
* **SNMPv1 & SNMPv2c:** Legacy versions. They transmit all data, including community strings (passwords), in **plain text**. Highly vulnerable to packet sniffing.
* **SNMPv3:** The modern standard. It provides **Authentication** (proves who is querying the device) and **Encryption** (secures the payload so attackers cannot read the network metrics).

* **Port Numbers:** SNMP Polling uses **UDP 161**, while SNMP Traps (alerts) use **UDP 162**.
* **Community Strings:** Used in SNMPv1 and v2c as a plaintext password to access the device. 
  * *Public:* Usually provides read-only access.
  * *Private:* Usually provides read-write access (highly dangerous if sniffed on the wire).

## 2. Syslog & Centralized Logging
If a router crashes, its internal logs are destroyed. Syslog solves this by constantly forwarding log entries to a secure, centralized server.
* **Syslog (Port 514 UDP):** The standard protocol for sending logging messages. 
* **Severity Levels:** Syslog categorizes alerts from 0 to 7. (Lower is more critical).
  * **0 (Emergency):** System is unusable (e.g., total network collapse).
  * **1 (Alert):** Immediate action is needed.
  * **4 (Warning):** An error might occur if nothing is done.
  * **7 (Debug):** Extremely verbose information used only by developers for deep troubleshooting.
* **NTP Requirement:** For Syslog to be useful during a cyber investigation, every single device must have perfectly synchronized clocks via **NTP (Network Time Protocol)**. If the timestamps don't match, you cannot accurately reconstruct the timeline of an attack.

## 3. Traffic Analysis & Flow Data
Sometimes you don't need to capture the entire data packet (which takes up massive storage); you just need the "metadata" of the conversation.
* **NetFlow:** A protocol developed by Cisco to collect IP traffic information. It records the "flow" of traffic.
  * *What is a Flow?* A unidirectional stream of packets between a specific Source IP/Port and Destination IP/Port using the same protocol.
  * *Analogy:* NetFlow is like a phone bill. It tells you *who* called *whom*, at what time, and for how long. It does *not* record the actual audio of the conversation.
* **sFlow (Sampled Flow):** Instead of recording every single conversation, it takes a statistical sample (e.g., 1 out of every 10,000 packets) to provide a general overview of network health with very low CPU overhead.
* **IPFIX:** The open-standard, non-proprietary version of NetFlow.

## 4. SIEM & Log Management (Exam Patch)
* **SIEM (Security Information and Event Management):** The centralized server that aggregates, correlates, and analyzes Syslog data from all network devices to detect anomalies or attacks.
* **Syslog Facility Code:** Along with the Severity Level (0-7), Syslog attaches a "Facility Code" which identifies the specific *program or service* that generated the log.
* **API Automation:** Modern networks use APIs to securely communicate and configure thousands of devices simultaneously, eliminating the need to manually SSH into each individual router.

## 5. Network Discovery & Device Management (Exam Patch)
* **Discovery Protocols:** Used to map out the network by identifying directly connected neighbors. 
  * *LLDP (Link Layer Discovery Protocol):* The vendor-neutral open standard.
  * *CDP (Cisco Discovery Protocol):* Proprietary to Cisco devices.
* **Configuration & Firmware Backups:** You must always backup a router's configuration file. However, configuration files are often tied to specific **Firmware versions**. If a router dies, you may need to downgrade the new replacement router's firmware before it will accept the old configuration file.
* **Availability Monitoring:** The most basic form of monitoring. Usually relies on ICMP (Ping) to simply display if a device is **Up (Green)** or **Down (Red)**.

## 6. Performance Metrics & Baselines
Monitoring software tracks specific metrics to trigger alerts when the network deviates from its baseline.
* **Bandwidth / Utilization:** How much of the physical link capacity is currently being used.
* **Latency:** The time it takes for a packet to travel from the source to the destination.
* **Jitter:** The variation in latency. Highly destructive to real-time traffic like VoIP and video conferencing. (If packets arrive at unpredictable intervals, the phone call sounds robotic and broken).
* **Packet Loss:** Packets that never arrive at their destination. TCP will retransmit them (causing delays), while UDP will simply drop them.

## 7. Interface Errors
When checking a specific switch port, these errors indicate physical layer issues:
* **CRC (Cyclic Redundancy Check) Errors:** The packet arrived, but the mathematical checksum failed. This usually indicates a bad cable, a loose connector, or heavy electromagnetic interference (EMI).
* **Runts:** Frames that are too small (less than 64 bytes). Usually caused by Ethernet collisions.
* **Giants:** Frames that are too large (over 1518 bytes) on a network not configured for Jumbo Frames.


## 10. Tool Distinctions & Troubleshooting (Exam Patch)
* **Nmap vs. Wireshark:**
  * *Nmap:* An active **Network Mapper** used for network discovery, finding live devices, and identifying network topology.
  * *Wireshark:* A passive **Protocol Analyzer** (Packet Sniffer) used to capture and inspect the deep contents of individual packets.
* **SNMP vs. Syslog for Performance:**
  * *SNMP:* Used for **Performance Monitoring** (evaluating efficiency, finding bandwidth bottlenecks, tracking CPU load).
  * *Syslog:* Used strictly for **Event Logging** (collecting error messages and hardware alerts). It does not monitor performance metrics.
* **Flow Analysis vs. Packet Capture:**
  * *NetFlow & sFlow:* Used for **Traffic Flow Analysis** (metadata: who talked to whom, and how much data was sent).
  * *Packet Sniffing:* Used to examine the actual payload/contents of the traffic.
