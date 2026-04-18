# Day 6: Switching Technologies (N10-009)
*Fact-Checked against Professor Messer N10-009 Objective 2.2*

## 1. Virtual LANs (VLANs) & Layer 3 Switching
A Local Area Network (LAN) is traditionally a single broadcast domain. VLANs logically separate a single physical switch into multiple isolated broadcast domains.
* **Separation:** Devices in VLAN 10 cannot natively communicate with devices in VLAN 20, improving both security and network performance by containing broadcast traffic.
* **Voice over IP (VoIP) Separation:** Phones and computers often share the same physical cable. VLANs separate the bursty data traffic (PC) from the latency-sensitive voice traffic (Phone) to prevent congestion.
* **Layer 3 Switches:** Combine Layer 2 switching and Layer 3 routing into one physical device. To route traffic between different VLANs internally, a Layer 3 switch uses an **SVI (Switched Virtual Interface)**.

## 2. Trunking & 802.1Q
To connect multiple VLANs across two different switches using a single cable, you must configure a Trunk Port.
* **802.1Q (The Modern Standard):** The IEEE standard for VLAN trunking. It injects a 12-bit VLAN tag into the middle of the Ethernet frame (right after the source MAC address). This supports up to 4,094 distinct VLANs.
* **ISL (Inter-Switch Link):** A legacy, proprietary trunking protocol. You will almost always use 802.1Q today.
* **Native VLAN:** A designated VLAN on a trunk that traverses the link *without* an 802.1Q tag. Often used for management or switch-to-switch notification traffic. 
  * *Critical Rule:* The Native VLAN configuration **must be identical** on both connecting switches, or the switch logs will generate continuous errors.

## 3. Spanning Tree Protocol (STP / 802.1D)
Connecting redundant cables between switches creates a **Switching Loop**. Because Layer 2 frames do not have a TTL (Time to Live) mechanism, broadcast frames will loop infinitely, quickly overwhelming and crashing the switches.
* **How STP Works:** It detects loops and places specific ports into a **Blocking** state to break the loop, while leaving others in a **Forwarding** state.
* **Port Roles:**
  * *Root Port (RP):* The interface that provides the shortest path back to the Root Bridge.
  * *Designated Port (DP):* A port that is allowed to forward traffic.
  * *Blocked Port:* A port disabled by STP to prevent the loop.
* **Convergence (802.1D vs. 802.1w):** * *Original STP (802.1D):* If a link drops, STP clears its tables, goes into a "Listening" phase, and unblocks a redundant port. This convergence takes **30 to 50 seconds**.
  * *RSTP (Rapid Spanning Tree Protocol / 802.1w):* The modern standard. Reduces the convergence downtime to approximately **6 seconds**. It is entirely backwards compatible with legacy 802.1D devices.

## 4. MTU & Jumbo Frames
* **MTU (Maximum Transmission Unit):** The absolute largest amount of data a network interface can send in a single packet without fragmenting it.
* **Standard MTU:** 1500 bytes for standard Ethernet.
* **Jumbo Frames:** Frames with an MTU larger than 1500, typically up to **9000 bytes**. 
* **Use Case:** Used primarily in high-speed SANs (Storage Area Networks) or database replication to move massive blocks of data efficiently by drastically reducing the header overhead. *Note: Every single switch, router, and NIC in the path must be manually configured to support Jumbo Frames, or the oversized frames will be dropped.*

## 5. Port Mirroring / SPAN
* **Function:** Takes all the traffic crossing one switch port (or an entire VLAN) and sends an exact copy of it out a different port.
* **Use Case:** You plug a laptop running Wireshark (Protocol Analyzer) or an IDS (Intrusion Detection System) into the mirror port to monitor network traffic without taking the actual devices offline or putting your security tools in-line with the data flow.
