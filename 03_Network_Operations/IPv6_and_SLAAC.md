# CompTIA Network+ N10-009 — Objective 3.4: IPv6 and SLAAC

### 1. DHCPv6 (Dynamic Host Configuration Protocol version 6)
* **Overview:** Updates dynamic configuration features for IP version 6 architectures, deploying redundant DHCP setups optimized for corporate enterprise resilience.
* **Administrative Framework:** Matches the standard configuration loops of ipv4 while integrating natively into multi-server architectures.

### 2. Stateless Addressing Concepts
* **Definition:** An inherent feature of IPv6 allowing endpoints to dynamically configure their own unique IP variables to communicate link-wide.
* **In-Bracket Definition:** `(An address assignment method where a device creates its own functional IP address without relying on a central DHCP server)`
* **Key Properties:** Deployed with zero centralized server coordination, zero tracking of MAC-to-IP entries, and zero lease time structures, meaning addresses are never surrendered back to a pool.

### 3. NDP (Neighbor Discovery Protocol)
* **Definition:** A vital multicasting protocol replacing the legacy, broadcast-heavy Address Resolution Protocol (ARP) to track nearby assets and routing nodes cleanly.
* **In-Bracket Definition:** `(A multi-functional IPv6 protocol that replaces ARP to discover nearby devices and routers efficiently)`
* **Router Solicitation (RS):** A multicast query broadcast by an endpoint looking for an active gateway: "If there's any router out there, please respond back."
* **Router Advertisement (RA):** Periodic or solicited packets cast by routers to supply subnets with prefix details, prefix lengths, DNS mappings, and interface parameters.

### 4. SLAAC (Stateless Address Autoconfiguration)
* **Definition:** The functional method through which an IPv6 system builds its own network layer parameters without using external address assignment servers.
* **In-Bracket Definition:** `(An automated process where a device combines a router's subnet prefix with its own interface ID to build a unique IPv6 address)`
* **Operational Flow:** 1. The host sends an NDP Router Solicitation.
  2. The router answers with an RA defining the **64-bit subnet prefix**.
  3. The client constructs the missing trailing **64-bit Interface ID** via a modified version of its MAC address (padding it with **FF FE** in the middle) or by deploying a randomized hex string.
  4. **Duplicate Address Detection (DAD):** The device runs a final check across the local link to verify that its created host identifier is unique.
  * **In-Bracket Definition:** `(A background security check that verifies no other device on the network is using the newly generated IP address)`






