# CompTIA Network+ N10-009 — Objective 5.3
## Routing and IP Issues

Layer 3 network problems occur due to missing routes, configuration mismatches, or IP address allocation failures. Troubleshooting these problems requires analyzing routing tables and monitoring DHCP state changes.

---

### 1. Routing Tables and Path Validation
A router reads its routing table to choose the best "next hop" for a packet. If a router cannot find a route matching a packet's destination, it deletes the packet entirely.

* **Destination Host Unreachable:** When a router drops a packet due to a missing route, it sends an ICMP "Host Unreachable" message back to the source device to flag the missing pathway.
* **The Asymmetric Path Rule:** Traffic requires a continuous, bidirectional path to function. When troubleshooting, you must inspect the routing tables of every single router along the path from point A to point B, **and** verify that those same routers have valid routes to get back from point B to point A.
* **Gateway of Last Resort (Default Route):** A catch-all destination used when a packet's target IP fails to match any explicit entry in the routing table.
  * **Network Notation:** Written as `0.0.0.0/0`, representing every host on every network.
  * **Example:** If a corporate router knows how to reach internal subnets (`10.1.0.0/16`) but receives a packet destined for an internet IP like `8.8.8.8`, it passes that packet directly to the Gateway of Last Resort (the upstream Internet Service Provider router).

### 2. DHCP Address Pool Exhaustion and APIPA
When a client joins a network, it broadcasts a DHCP request to lease an IP address. If the server cannot provide one, connectivity drops to local limitations.

* **Pool Exhaustion:** Occurs when a DHCP server runs out of unassigned IP addresses within its configured scope. This frequently happens in environments with high device turnover, such as public Wi-Fi hotspots or corporate lobbies.
* **APIPA (Automatic Private IP Addressing):** If a client fails to receive a response from a DHCP server, the operating system assigns itself a temporary link-local IP address.
  * **IP Range:** `169.254.0.1` through `169.254.255.254`.
  * **Limitation:** APIPA addresses are non-routable. Devices can communicate with other local peers on the exact same switch segment, but cannot talk to different subnets or access the internet.
* **Remediation & IPAM Control:**
  * **IPAM (IP Address Management):** Centralized software consoles used to monitor address pools and track real-time DHCP scope utilization across an enterprise.
  * **Lease Time Optimization:** To fix pool exhaustion in high-turnover areas, administrators decrease the DHCP lease time (e.g., lowering it from 8 days down to 2 hours) so that inactive IP addresses are returned to the pool quickly.

### 3. IP Parameter Mismatches
Receiving or manually configuring incorrect Layer 3 information immediately breaks network communication paths.

* **The Core Parameters:**
  * **IP Address:** The unique identifier for a device on a network.
  * **Subnet Mask:** Defines which portion of the IP address belongs to the network domain and which portion belongs to the individual host.
  * **Default Gateway:** The router interface IP that local devices must send packets to when trying to reach destinations outside their local subnet.
* **Mismatch Example:** If a machine is assigned `192.168.1.50` with an incorrect subnet mask of `255.255.255.240` instead of `255.255.255.0`, it will incorrectly calculate its network boundaries, treat local neighbors as remote systems, and fail to communicate.
* **Troubleshooting Step-by-Step Verification Loop:**
  1. **Ping Localhost (`127.0.0.1`):** Verifies that the local computer's TCP/IP software stack is initialized and operational.
  2. **Ping Local IP:** Verifies that the local Network Interface Card (NIC) is properly bound and functional on the OS.
  3. **Ping Default Gateway:** Verifies Layer 1 and Layer 2 connectivity to the local router interface.
  4. **Ping Remote Target:** Verifies that the default gateway is successfully routing traffic across external Layer 3 paths.

### 4. Duplicate IP Address Conflicts
An IP conflict occurs when two distinct physical devices on the exact same broadcast domain attempt to use the same IP address simultaneously.

* **Common Root Causes:**
  * **Static Overlaps:** A technician manually configures a static IP on a device that falls directly inside an active, dynamic DHCP server allocation pool.
  * **Rogue DHCP Servers:** A misconfigured or unauthorized secondary DHCP server begins handing out conflicting IP leases on the subnet.
* **Modern OS Detection:** Modern operating systems broadcast Gratuitous ARP requests when initializing their network interface. If another device responds claiming that IP, the incoming device flags a duplicate IP conflict error and disables its network interface to prevent disruption.
* **Tracking Conflicts with the ARP-to-Switch Mapping Technique:**
  1. Ping the conflicting IP address from an administrative workstation to populate your local cache.
  2. Run `arp -a` to extract the exact **MAC Address** associated with that device.
  3. Log into the local managed switch and search its MAC address table for that specific address.
  4. Identify the exact physical switch port the conflicting device is plugged into and disable the port to isolate it.
