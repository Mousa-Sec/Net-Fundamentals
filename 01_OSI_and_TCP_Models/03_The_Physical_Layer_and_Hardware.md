# Day 3: The Physical Layer & Network Hardware (N10-009)
*Exhaustive Video Mapping: Devices, Security, Storage, & Cabling*

## 1. Core Infrastructure & Wireless (Objective 1.2)
* **Router (Layer 3):** Routes traffic between different IP subnets across the world.
* **Switch (Layer 2):** Forwards traffic in hardware using ASICs (Application-Specific Integrated Circuits) based on MAC addresses. Can provide Power over Ethernet (PoE).
* **Layer 3 Switch:** A single device containing both Layer 2 switching and Layer 3 routing functionality.
* **Access Point / AP (Layer 2):** Acts exclusively as a bridge translating between the 802.11 wireless network and the 802.3 wired Ethernet network.
* **Wireless LAN Controller (WLC):** A centralized management tool ("single pane of glass") used to configure, update, and monitor dozens of Access Points simultaneously. It manages roaming and access policies. Often proprietary to the AP manufacturer.

## 2. Security & Edge Devices (Objective 1.2)
* **Firewalls:**
  * *Traditional:* Filters traffic strictly by TCP/UDP port numbers.
  * *Next-Generation Firewall (NGFW):* Application-aware filtering. Usually acts as a Layer 3 router, handles Network Address Translation (NAT), and provides encrypted VPN tunnels.
* **IDS vs. IPS:**
  * *IDS (Intrusion Detection System):* Monitors for known exploits (buffer overflows, XSS) and triggers alerts/alarms. Cannot block traffic.
  * *IPS (Intrusion Prevention System):* Sits in the flow of traffic and actively blocks attacks before they enter the network.
* **Proxy Server:** A middleman device that makes requests on the user's behalf to perform URL filtering, caching, and malware scanning.
  * *Explicit Proxy:* Requires configuration on the user's OS or browser.
  * *Transparent Proxy:* Operates invisibly on the network without user configuration.
* **Load Balancer:** Distributes inbound traffic across multiple backend servers to maintain uptime. 
  * *Features:* TCP/SSL Offloading (handling the encryption math), Caching, and Quality of Service (QoS) to prioritize specific traffic.

## 3. Network Storage (Objective 1.2)
* **NAS (Network Attached Storage):** Provides **File-level access**. When modifying a document, the entire file must be pulled across the network to memory, changed, and written entirely back.
* **SAN (Storage Area Network):** Provides **Block-level access**. Functions like a local hard drive. If you change one paragraph in a massive file, it only rewrites those specific modified blocks. Highly efficient, often placed on isolated high-bandwidth networks.

## 4. Cabling & Connectors (Objective 1.5)
* **Copper Categories:** Cat 5e (1 Gbps / 100m), Cat 6 (10 Gbps / 55m), Cat 6a (10 Gbps / 100m).
* **Copper Connectors:** RJ45 (Network), RJ11 (Telephone).
* **Plenum Cable:** Fire-retardant jacket required in HVAC drop-ceilings; does not release toxic fumes.
* **Fiber Optic:** Immune to EMI. 
  * *Single-mode:* Lasers, long-distance WANs.
  * *Multimode:* LEDs, shorter LANs/Data Centers.
* **Fiber Connectors:** LC (clips), ST (bayonet twist), SC (square push-pull).
* **Transceivers:** SFP (1 Gbps), SFP+ (10 Gbps), QSFP (40 Gbps).

## 4. Copper Cabling Standards (Objective 1.5 - Transcript Verified)
* **Twisted Pair Mechanics:** Wires are paired and twisted at different rates to cancel out Electromagnetic Interference (EMI). The 802.3 Ethernet standard dictates the minimum category required (e.g., 1000BASE-T requires at least Cat 5).
  * *Categories:* Cat 5e (1 Gbps / 100m), Cat 6 (10 Gbps / 55m), Cat 6a (10 Gbps / 100m).
* **Coaxial Cable:** A single copper conductor surrounded by an insulator, shielding, and a jacket that all share a common axis.
  * *RG-6:* The standard coaxial cable used for bringing ISP/Cable Modem connections into a data center or home.
* **Twinaxial Cable (Twinax):** Contains two inner conductors. 
  * *Use Case:* Directly connects 10 Gbps Ethernet using SFP+ transceivers. 
  * *Limitations:* Extremely fast (lower latency than twisted pair) and cheap, but strictly limited to very short distances (**Maximum 5 meters**). Used mostly for connecting switches within the same server rack.
* **Cable Jackets (Plenum vs. Non-Plenum):**
  * *Non-Plenum (PVC):* Made of standard Polyvinyl Chloride. Releases toxic fumes and heavy smoke if burned. Never put in shared airspaces.
  * *Plenum-Rated (FEP):* Made of Fluorinated Ethylene Polymer or low-smoke PVC. Fire-retardant. Mandatory for drop-ceilings and shared HVAC airspaces. Less physically flexible than standard PVC.

## 5. Optical Fiber Standards (Objective 1.5 - Transcript Verified)
Uses light instead of electrical RF signals. Completely immune to Radio Frequency Interference (RFI/EMI) and highly secure (wiretapping physically breaks the light connection and instantly triggers alerts).
* **Fiber Cable Construction:**
  * *Core:* The highly reflective center layer where the light actually travels.
  * *Cladding:* A low-reflective layer surrounding the core that keeps the light bouncing inward.
  * *Buffer Coating:* The outer protective plastic coating.
  * *Ferrule:* The rigid physical cylinder at the very tip of the connector that protects and aligns the delicate fiber core.
* **Multimode Fiber (MMF):**
  * *Distance:* Short-range communication (Up to ~2 kilometers).
  * *Light Source:* Inexpensive LEDs.
  * *Core Size:* Relatively large core. The light bounces off the cladding in multiple different paths or "modes".
* **Single-mode Fiber (SMF):**
  * *Distance:* Extreme long-distance communication (Up to 100 kilometers without needing a signal repeater).
  * *Light Source:* Expensive Lasers or highly intense LEDs.
  * *Core Size:* Very small core. The light travels in a single, direct path (mode) straight down the center.
