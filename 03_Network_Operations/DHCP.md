
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

