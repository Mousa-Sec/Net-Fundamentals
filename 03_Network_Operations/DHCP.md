
# CompTIA Network+ N10-009 — Objective 3.4: DHCP (Dynamic Host Configuration Protocol)

### 1. Evolution of Automatic IP Addressing
* **Manual Layouts:** Early networks required manual data entry of IP details, subnet masks, gateways, and DNS servers on every endpoint, which didn't scale and caused configuration errors.
* **BOOTP:** An early automation attempt that failed to provide complete automatic configuration loops and couldn't reclaim IP allocations when hardware disconnected.
* **DHCP:** The modern standard that automates configuration tracking. It dynamically assigns, handles, and automatically reclaims IP address blocks.
* **In-Bracket Definition:** `(A network protocol that automatically provides and manages IP address leases for connected devices)`

### 2. The DORA Process (Four-Step Handshake)
* **Discover (D):** The client broadcasts a message to locate any active DHCP servers on the subnet.
  * *Addressing:* Source: `0.0.0.0` | Destination: `255.255.255.255` | Ports: UDP 68 to UDP 67.
* **Offer (O):** Available servers reply with a proposed network address profile.
  * *Addressing:* Source: Server IP | Destination: `255.255.255.255` | Ports: UDP 67 to UDP 68.
* **Request (R):** The client selects an offer and broadcasts its choice to accept that specific profile.
  * *Addressing:* Source: `0.0.0.0` | Destination: `255.255.255.255` | Ports: UDP 68 to UDP 67.
  * *Purpose:* Notifies the accepted server while alerting other servers to reclaim their declined offers.
* **Acknowledgement (A):** The server sends a final confirmation to seal the lease.
  * *Addressing:* Source: Server IP | Destination: `255.255.255.255` | Ports: UDP 67 to UDP 68.

### 3. DHCP Relay / IP Helper Functionality
* **The Constraint:** DHCP discovery relies heavily on broadcasts, which are strictly blocked by routers. This prevents clients from reaching servers located outside their local subnet.
* **DHCP Relay / IP Helper:** A router capability that allows centralized server management by bridging separate subnets.
* **In-Bracket Definition:** `(A router feature that converts local DHCP broadcasts into unicast packets to safely forward them to a central DHCP server on a different subnet)`
* **Traffic Conversion:** The router intercepts local client broadcasts, swaps the source address with its own interface IP, and forwards the packet as a **unicast** straight to the remote DHCP server. It reverses this process for the server's reply, making remote addressing seamless.


# CompTIA Network+ N10-009 —Objective 3.4: Configuring DHCP

### 1. DHCP Scopes and Pools
* **DHCP Scope:** A structural grouping representing an administrative setup of parameters tailored for a specific network subnet.
* **In-Bracket Definition:** `(The full range of IP settings and addresses allocated for a single local network segment)`
* **Key Components:**
  * **Address Pool:** The contiguous block of numbers the server handles for automated assignment.
  * **Exclusions:** Specific IPs kept safe from automation to prevent deployment conflicts.
  * **Lease Variables:** Dictates the expiration countdown assigned to each configuration lease.

### 2. Address Reservations
* **The Manual Baseline Issue:** Punching static IPs directly into servers or printers doesn't scale and increases management friction during structural address redesigns.
* **Address Reservation:** Often called Static DHCP, this binds fixed entries directly into the central server table.
* **In-Bracket Definition:** `(Configuring the DHCP server to always lock and deliver the exact same IP address to a specific hardware MAC address)`
* **Operational Flow:** Ties a device's hardware MAC address to an explicit target IP. When that specific network card boots, it automatically receives its designated matching IP from the central pool every single time.

### 3. DHCP Lease Timers and Renewal Operations
* **Core Concept:** DHCP leases are non-permanent and run on ticking expiration windows. Clients monitor state health via two distinct background timers:
* **T1 Timer (Renewal):** Set at **50%** of total lease life. The device sends a **unicast** message to the original DHCP server to renew its duration.
* **T2 Timer (Rebinding):** Set at **87.5% (7/8)** of lease life. If the primary server fails to reply during T1, the device broadcasts a message to **any backup server** on the link to preserve its address.
* **Math Example (8-Day Lease):** T1 fires at Day 4 (targeted unicast check); T2 fires at Day 7 (segment-wide broadcast check).

### 4. DHCP Options
* **Definition:** Custom parameter fields used to push special infrastructure information down to client platforms during addressing initialization.
* **In-Bracket Definition:** `(Numbered data tags inside a DHCP packet used to deliver specialized software and proxy server settings to devices)`
* **Scalability:** Contains up to 254 unique numbered settings fields. Success depends on whether the active server software fully supports the required target option block.
* **Technical Examples:**
  * **Option 129:** Dynamically delivers the target IP profile for a Voice over IP (VoIP) Call Server directly to hardware phones.
  * **Option 135:** Feeds destination IP configurations for an interactive HTTP Proxy Server straight to web clients.

