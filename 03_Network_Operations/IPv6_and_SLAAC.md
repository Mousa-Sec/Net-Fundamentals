# CompTIA Network+ N10-009 — Objective 3.4: IPv6 and SLAAC

### 1. DHCPv6 (Dynamic Host Configuration Protocol version 6)
* **Overview:** Updates dynamic configuration features for IP version 6 architectures, deploying redundant DHCP setups optimized for corporate enterprise resilience.
* **Administrative Framework:** Matches the standard configuration loops of IPv4 ("Stateful" allocation) where a central server controls a master database pool, hands out assignments, and tracks live device lease states.

### 2. Stateless Addressing Concepts
* **Definition:** An inherent, autonomous feature of IPv6 allowing endpoints to dynamically configure their own unique IP variables to communicate link-wide.
* **In-Bracket Definition:** `(An address assignment method where a device creates its own functional IP address without relying on a central DHCP server)`
* **Key Properties:** Deployed with zero centralized server coordination, zero tracking of MAC-to-IP entries, and zero lease time structures, meaning addresses are permanent and never surrendered back to a pool.

### 3. NDP (Neighbor Discovery Protocol)
* **Definition:** A vital multicasting protocol replacing the legacy, broadcast-heavy Address Resolution Protocol (ARP) to track nearby assets and routing nodes cleanly.
* **In-Bracket Definition:** `(A multi-functional IPv6 protocol that replaces ARP to discover nearby devices and routers efficiently)`
* **Router Solicitation (RS):** A targeted multicast query broadcast by an endpoint looking for an active gateway: *"Hey, I just joined this network. Is there a gateway out there that can give me the local prefix?"*
* **Router Advertisement (RA):** Periodic or solicited packets cast by local routers to supply subnets with critical configuration DNA, including prefix details, prefix lengths, DNS mappings, and interface parameters.

### 4. SLAAC (Stateless Address Autoconfiguration)
* **Definition:** The functional execution framework through which an IPv6 system builds its own network layer parameters without using external address assignment servers.
* **In-Bracket Definition:** `(An automated process where a device combines a router's subnet prefix with its own interface ID to build a unique IPv6 address)`
* **The 128-Bit Address Split:**
  * **First 64 Bits (Network Prefix):** Learned directly from the local router's Router Advertisement (RA) message. Acts like the network neighborhood zip code.
  * **Last 64 Bits (Interface ID):** The host portion that the device must calculate independently on its own.

#### The Two Methods for Generating the 64-Bit Interface ID:
* **Method A (Modified EUI-64):** The device takes its **48-bit physical hardware MAC address**, splits it cleanly down the middle, and injects a static **16-bit hex string (`FF FE`)** right into the center to pad it out to a full 64-bit block. *(Note: It also flips the 7th bit, the universal/local bit, to seal the hardware mapping).*
* **Method B (OS Randomization / Privacy Extensions):** For device hardening and digital privacy, modern operating systems generate a completely **randomized 64-bit value** for the host portion. This hides the machine's permanent physical MAC address footprint, blocking outside malicious actors or advertisers from tracking the device across different physical networks. No `FF FE` is used in this method.

#### Final Security Verification:
* **Duplicate Address Detection (DAD):** A live cryptographic background check carried out via NDP before the device finalized binding its new address.
* **In-Bracket Definition:** `(A background security check that verifies no other device on the network is using the newly generated IP address)`
* **Operational Flow:** The device shouts out a multicast query containing its completed **128-bit address**. If another machine responds, an IP collision is flagged and a new ID is calculated. If the network remains completely silent, the unique address is locked into the interface interface.
