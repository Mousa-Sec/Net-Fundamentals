# CompTIA Network+ N10-009 — Objective 5.3
## Switching Issues

Data link layer troubleshooting requires a deep understanding of layer 2 loops, Spanning Tree Protocol (STP) mechanics, configuration-level VLAN assignments, and Access Control List (ACL) behaviors.

---

### 1. Layer 2 Broadcast Storms & Loop Dynamics
Unlike Layer 3 routing protocols, Ethernet frames lack built-in expiration mechanisms to prevent infinite propagation.

* **The Framing Flaw:** IPv4 and IPv6 packets contain a Time to Live (TTL) / Hop Limit field that routers decrement at each hop, discarding the packet when it hits zero. Layer 2 Ethernet frames have no TTL field. 
* **The Loop Mechanism:** Switches forward frames based on the destination MAC address in their MAC table. If a frame is a broadcast, multicast, or unknown unicast, the switch floods it out of every single port except the incoming port. 
* **Broadcast Storms:** If a physical loop exists in the wiring closet (e.g., dual active connections between two switches without structural safeguards), flooded frames circle indefinitely. As more broadcast and multicast traffic enters the loop, it rapidly consumes all available link bandwidth and switch processing memory, bringing the entire network domain to a complete halt.

### 2. Spanning Tree Protocol (STP) Mechanics
STP creates a loop-free logical topology over physically redundant switch designs by monitoring and dynamically disabling redundant paths.

* **Bridge Protocol Data Units (BPDUs):** Switches track topology states by sending and receiving BPDUs. These frames are sent as Layer 2 multicasts every **2 seconds** by default.
* **Topology Convergence Failures:** If a switch fails to receive a expected BPDU "Hello" frame, it counts down. If **3 consecutive updates** are missed (6 seconds total), the switch considers the path down, detects a topology change, and recalculates its port states to open alternative loop-free paths.
* **The Root Bridge Election:** When the network initializes, all switches participate in an election to choose a central root bridge.
  * **Bridge ID Configuration:** The switch with the lowest **Bridge ID (BID)** wins the election. The BID consists of a configurable priority value ranging from **0 to 61,440**.
  * **Mac Address Tie-Breaker:** If multiple switches are left at identical priority defaults, the switch carrying the lowest physical MAC address becomes the Root Bridge.
* **STP Port Role Classifications:**
  * **Root Port:** The single designated interface on a non-root switch that offers the lowest path cost back to the Root Bridge.
  * **Designated Port:** Active, loop-free ports authorized to forward traffic onto a specific network segment.
  * **Blocked / Discarding Port:** Ports administratively or logically restricted from forwarding data frames to disrupt the physical loop.


### 3. STP Interface State Lifecycle
Interfaces shift through distinct tracking modes before transitioning into active data forwarding to prevent accidental loop generation during topology shifts.
* **Blocking / Discarding:** Evaluates incoming BPDUs to maintain topology awareness but drops user data frames and refrains from learning MAC addresses.
* **Listening:** Transitions out of blocking to directly listen for adjacent switches on the local broadcast domain. It determines port roles without forwarding data frames.
* **Learning:** Populates the internal MAC address table using the source MAC addresses of passing frames, but does not yet forward user data.
* **Forwarding:** The interface becomes fully operational, actively learning MAC addresses and forwarding data frames.
* **Disabled:** The interface is manually brought down by an administrator (`shutdown`). It takes no part in the Spanning Tree state machine.

### 4. VLAN Mismatches & Configuration Alignment
Virtual Local Area Networks (VLANs) segregate a single physical switch into distinct broadcast boundaries. Misconfigurations isolate endpoints from network access.
* **Access Ports:** Configured to map a physical interface to a single, specific VLAN ID. Endpoints connected to access ports drop untagged frames directly into that VLAN boundary.
* **Mismatched Assignment Faults:** A common operational failure occurs when a device receives a valid IP configuration via DHCP but cannot communicate with local peers. This happens when its physical switch port is accidentally bound to an incorrect VLAN ID (e.g., bound to VLAN 100 instead of VLAN 254). 
* **Remediation:** Troubleshooting requires executing administrative verification commands to trace physical port-to-VLAN mappings and re-assigning the interface or migrating the drop to an aligned port.

### 5. Access Control List (ACL) Misconfigurations
Switches and routers use ACLs to inspect and filter traffic flows passing across interfaces. Improper sequencing can accidentally drop legitimate production data.
* **Implicit Deny Behavior:** Modern networking equipment enforces an invisible **implicit deny all** rule at the absolute bottom of every ACL. If an ACL is bound to an active interface without any explicit permit lines, it functions as a total filter, dropping all incoming and outgoing communication instantly.
* **Rule Logic and Sequencing:** ACLs process rules sequentially from the top down and stop evaluation the moment a match occurs. 
  * Granular, highly specific parameters (e.g., explicit host IPs or individual port boundaries) must sit at the top of the chain.
  * Broader controls (e.g., large subnet masks or catch-all parameters) must reside lower down to prevent shadowing specific rules.
* **Management Safeguard Best Practices:** When modifying or applying complex access lists, engineers should temporarily disable the ACL engine or establish a failsafe timer. An unchecked typo or an out-of-order deny rule can instantly lock an administrator out of the switch CLI, preventing remote management access.
