# Day 3: The Physical Layer & Network Hardware (N10-009)
*Exhaustive Video Mapping: Devices, Copper, Fiber, Connectors, & Proxies*

## 1. Core Networking Devices (Objective 1.2)
* **Hub (Layer 1):** Legacy. Multiports traffic to all devices (half-duplex). Massive security risk.
* **Switch (Layer 2):** Forwards traffic intelligently based on MAC addresses (full-duplex).
* **Router (Layer 3):** Routes traffic between distinct IP subnets using IP addresses.
* **Multilayer Switch (Layer 3):** A high-speed switch that routes traffic based on IP addresses.
* **Repeater / Extender (Layer 1):** Regenerates a weakened signal to push it further.
* **Wireless Access Point / WAP (Layer 2):** Bridges the wired network and the wireless (802.11) network.

## 2. Specialized Network Devices (Objective 1.2)
* **Load Balancer:** Distributes traffic across multiple servers for fault tolerance.
  * *Layer 4 vs Layer 7:* Routes based on TCP ports (L4) or actual URL/Application data (L7).
  * *SSL/TLS Offloading:* Handles the heavy CPU math of encryption/decryption.
  * *Caching:* Stores copies of frequent requests to reduce server load.
  * *QoS (Quality of Service):* Prioritizes latency-sensitive traffic (like VoIP/Video) over bulk traffic (like file downloads).
* **Proxy Server:** A middleman device.
  * *Forward Proxy:* Protects internal users. Hides user IPs and filters outbound URLs.
  * *Reverse Proxy:* Protects internal servers. Scrubs inbound traffic from the internet.
  * *Transparent Proxy:* Intercepts traffic invisibly; users do not know it is there.

## 3. Cabling & Connectors (Objective 1.5)
* **Copper Categories:** Cat 5e (1 Gbps / 100m), Cat 6 (10 Gbps / 55m), Cat 6a (10 Gbps / 100m).
* **Copper Connectors:** RJ45 (Network), RJ11 (Telephone).
* **Plenum Cable:** Fire-retardant jacket required in HVAC drop-ceilings; no toxic fumes.
* **Fiber Optic:** Immune to EMI. 
  * *Single-mode:* Lasers, long-distance WANs.
  * *Multimode:* LEDs, shorter LANs/Data Centers.
* **Fiber Connectors:** LC (clips), ST (bayonet twist), SC (square push-pull).
* **Transceivers:** SFP (1 Gbps), SFP+ (10 Gbps), QSFP (40 Gbps).
