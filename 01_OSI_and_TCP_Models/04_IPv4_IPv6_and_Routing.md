# Day 4: IPv4, IPv6, and Routing Protocols (N10-009)
*Exhaustive Video Mapping: Subnetting Math, IP Standards, and Network Routing*

## 1. Binary Math & Conversions (Objective 1.7 - Transcript Verified)
* **The 8-Bit Octet:** An IPv4 address is made of four 8-bit chunks (octets). An 8-bit chunk contains 8 ones and zeros. 
* **The Conversion Chart:** Based on the powers of 2. Memorize from left to right:
  `128 | 64 | 32 | 16 | 8 | 4 | 2 | 1`
* **Maximum Value:** If an octet is all ones (`11111111`), the mathematical total is exactly **255** (128+64+32+16+8+4+2+1).
* **Binary to Decimal Logic:** Place the binary under the chart. Add the values where a `1` exists. Ignore values where a `0` exists.
* **Decimal to Binary Logic:** Start at 128. If the chart value fits into your target number, place a `1` and subtract. If it doesn't fit, place a `0` and move right.

## 2. IPv4 Addressing & RFC 1918 (Objective 1.7 - Transcript Verified)
* **IPv4 Anatomy:** 32 bits total, divided into 4 octets (8 bits each). Maximum decimal value of a single octet is 255.
* **Core Components:**
  * *IP Address:* The unique identifier for the device.
  * *Subnet Mask:* Determines the boundary between the network portion and the host portion of the IP.
  * *Default Gateway:* The router IP address on the local subnet used to send traffic outside the local network.

* **Private IP Ranges (RFC 1918):** Non-routable on the public internet. Used exclusively for internal LANs.
  * `10.0.0.0 /8` (10.0.0.0 - 10.255.255.255)
  * `172.16.0.0 /12` (172.16.0.0 - 172.31.255.255)
  * `192.168.0.0 /16` (192.168.0.0 - 192.168.255.255)

* **APIPA / Link-Local Addressing:**
  * Auto-assigned when a device cannot reach a DHCP server.
  * *Range:* `169.254.1.0` to `169.254.254.255`.
  * *Limitation:* Traffic cannot be routed outside the local subnet (no internet access).

* **Specialized IP Addresses:**
  * *Loopback:* `127.0.0.1` (to 127.255.255.254). Used to test the local host's internal TCP/IP stack.
  * *Reserved Class E:* `240.0.0.1` to `255.255.255.255`. Set aside for experimental use; never assigned to standard hosts.
  * *VIP (Virtual IP):* An address not bound to a physical network adapter. Used for High Availability, Load Balancers, or Virtual Machines.

## 3. IPv4 Subnet Masks & CIDR (Objective 1.7 - Transcript Verified)
* **The Contiguous Rule:** A subnet mask is **always** a continuous, unbroken string of `1`s from left to right, followed by a continuous string of `0`s. You cannot alternate 1s and 0s.
* **Network vs. Host Boundary:** * The `1`s represent the **Network ID** (the locked routing portion).
  * The `0`s represent the **Host ID** (the usable IP addresses for local devices).
* **CIDR / Slash Notation (Classless Inter-Domain Routing):** A shorthand method of writing subnet masks by counting the exact number of contiguous `1`s.
  * *Example:* A subnet mask of `255.255.255.0` contains 24 ones, so it is written as **`/24`**.
* **The Subnet Mask "Cheat Sheet":** Because the `1`s must be contiguous, an octet in a subnet mask can **only** ever be one of these 8 decimal values. Memorize this table:
  * `10000000` = 128
  * `11000000` = 192
  * `11100000` = 224
  * `11110000` = 240
  * `11111000` = 248
  * `11111100` = 252
  * `11111110` = 254
  * `11111111` = 255

## 4. Calculating Subnets and Hosts (Objective 1.7 - Transcript Verified)
* **Variable Length Subnet Masks (VLSM):** The process of borrowing host bits (0s) and turning them into network bits (1s) to slice a large default network into smaller, customized subnets.
* **Calculating the Number of Hosts:**
  * *Formula:* **2^h - 2** (where *h* is the number of remaining host bits/zeros).
  * *Why subtract 2?:* You can never assign the very first IP (The Network Address) or the very last IP (The Broadcast Address) to a physical host.
* **Calculating the Number of Subnets:**
  * *Formula:* **2^s** (where *s* is the number of *borrowed* subnet bits past the default class boundary).
  * *Default Boundaries:* Class A defaults to /8. Class B defaults to /16. Class C defaults to /24.
  * *Example:* If you have a Class C address (`192.168.1.x`) but use a `/26` mask, you have borrowed **2** bits past the default /24 boundary. (2^2 = 4 subnets created).

## 5. Magic Number Subnetting Algorithm (Objective 1.7 - Transcript Verified)
A rapid calculation method to determine network boundaries without converting to binary.

* **Step 1: Identify the Interesting Octet:**
  * In the Subnet Mask, any octet that is `255` means the IP address is copied down exactly.
  * Any octet that is `0` becomes a `0` in the Network ID.
  * The octet that is a unique number (e.g., 192, 224, 240) is the "Interesting Octet".
* **Step 2: Calculate the Magic Number:**
  * Formula: **`256 - [Interesting Octet Value]`**.
  * The result is your "Block Size" (the increment between each distinct network).
* **Step 3: Map the Network Boundaries:**
  * *Subnet ID (Network Address):* The starting multiple of your Magic Number that contains the target IP address.
  * *Broadcast Address:* The very last IP before the *next* Magic Number block begins.
  * *First Usable Host:* Subnet ID + 1.
  * *Last Usable Host:* Broadcast Address - 1.

## 6. Seven Second Subnetting (Objective 1.7 - Transcript Verified)
An exam-day shortcut that removes the need for binary conversion. 

* **Step 1: The Whiteboard Chart**
  * *Decimal Mask:* `128 | 192 | 224 | 240 | 248 | 252 | 254 | 255`
  * *Block Size (Addresses):* `128 |  64 |  32 |  16 |   8 |   4 |   2 |   1`
* **Step 2: The Execution Steps**
  1. *Convert to Decimal:* Use the CIDR notation to locate the specific octet and drop down to find the Decimal Mask.
  2. *Find the Subnet Address:* Look at the Block Size row. Count up by that block size from zero until you find the specific range your target IP falls into. The starting number of that block is your Network Address.
  3. *Find the Broadcast Address:* The last number before the *next* block begins.
  4. *Usable Range:* Network + 1 (First IP), and Broadcast - 1 (Last IP).
* **The 255 / 0 Rule:**
  * Mask is `255`: Bring the IP address down exactly as it is.
  * Mask is `0`: Bring down a `0` (for Network ID) or a `255` (for Broadcast ID).

## 7. IPv6 Architecture & Compression (Objective 1.8 - Transcript Verified)
Created to solve the exhaustion of the IPv4 address space.
* **Format:** 128-bit address written in Hexadecimal. Divided by colons into 8 groups of 16 bits.
* **Compression Rule 1 (Leading Zeros):** Any zeros at the very beginning of a 16-bit group can be dropped. 
  * *Example:* `:0050:` becomes `:50:`.
* **Compression Rule 2 (Double Colon):** Two or more continuous groups of all zeros can be replaced with a `::`. 
  * *Constraint:* This can only be done **once** per IPv6 address to prevent routing ambiguity.

## 8. IPv4 to IPv6 Transition Mechanisms (Objective 1.8 - Transcript Verified)
* **Dual Stack Routing:** The preferred migration method. A single network interface is configured with both an IPv4 and an IPv6 address simultaneously, maintaining two independent routing tables.
* **Tunneling (6to4 / 4in6):** Encapsulating IPv6 traffic inside an IPv4 packet (or vice versa) to cross an incompatible legacy network. Largely deprecated.
* **Translation (NAT64 & DNS64):** * *DNS64:* Intercepts a DNS request from an IPv6 client and returns a synthetic IPv6 address that points to a translation router.
  * *NAT64:* The specialized router that receives the IPv6 traffic and actively translates it into an IPv4 packet so it can communicate with a legacy IPv4 server.

## 9. Routing Tables & Static Routing (Objective 2.1 - Transcript Verified)
* **The Routing Table:** The internal database a router uses to determine the "Next Hop" for a packet. If a router receives a packet destined for an IP address that is not in its routing table, the router will **discard** the packet.
* **Static Routing:** The manual configuration of network routes by a network administrator.
  * *Advantages:* Zero CPU/Memory overhead, highly secure (no routing broadcasts), and very fast to implement on small networks.
  * *Ideal Use Case:* **Stub Networks** (remote locations with only a single uplink/exit point to the internet).
  * *Disadvantages:* Does not scale to large enterprise networks, highly prone to human error (e.g., routing loops), and completely lacks fault tolerance (will not automatically reroute traffic if a link fails).

## 10. Dynamic Routing Protocols (Objective 2.1 - Transcript Verified)
Dynamic routing allows routers to automatically discover network paths and automatically failover if a link drops. Requires CPU and memory overhead to maintain.

* **Autonomous System (AS):** A routed network under the control of a single administrative entity.
* **OSPF (Open Shortest Path First):**
  * *Type:* Interior Gateway Protocol (IGP) / Link-State.
  * *Features:* An open standard supported by almost all vendors.
  * *Metric:* Uses **Cost** (based on link bandwidth/speed) to determine the best path.
* **EIGRP (Enhanced Interior Gateway Routing Protocol):**
  * *Type:* Interior Gateway Protocol (IGP).
  * *Features:* Highly Cisco-centric. Offers very fast convergence and efficient routing updates.
* **BGP (Border Gateway Protocol):**
  * *Type:* External Gateway Protocol (EGP).
  * *Features:* The routing protocol of the global Internet. Used to connect entirely different Autonomous Systems together.

## 11. Route Selection & Tie-Breakers (Objective 2.1 - Transcript Verified)
When a router has multiple paths to the same destination, it evaluates them in this strict order:
* **1. Prefix Length:** The router always selects the most specific route. A `/32` is preferred over a `/24`.
* **2. Administrative Distance (AD):** If prefix lengths are identical, the router trusts the protocol with the lowest AD.
  * Directly Connected = 0
  * Static Route = 1
  * EIGRP = 90
  * OSPF = 110
  * RIP = 120
* **3. Routing Metric:** If routes are learned from the exact same protocol, the internal metric is used (e.g., lowest OSPF Cost). Metrics cannot be compared across different routing protocols.

## 12. Advanced Routing Operations (Objective 2.1 - Transcript Verified)
* **First Hop Redundancy Protocol (FHRP):** Provides default gateway redundancy by assigning a Virtual IP (VIP) to multiple physical routers. If the active router fails, the standby router seamlessly assumes the VIP.
* **Subinterfaces:** Dividing a single physical router interface into multiple virtual interfaces. Typically used to route traffic for multiple VLANs across a single 802.1Q trunk connection (often called "Router on a Stick").

## 13. Network Address Translation (NAT) (Objective 2.1 - Transcript Verified)
A technology designed to conserve the limited supply of public IPv4 addresses and allow private networks to access the internet.
* **How it works:** A router intercepts outbound traffic, removes the non-routable private Source IP (RFC 1918), and replaces it with a valid Public IP. It maintains a stateful "NAT Table" to reverse the translation when the return traffic arrives.
* **Basic NAT (1-to-1):** Maps a single private IP to a single public IP. Highly inefficient for large networks.
* **PAT (Port Address Translation) / NAT Overload:**
  * The modern standard for internet access. 
  * The router translates both the IP address *and* the Source Port. 
  * By tracking unique port numbers in the NAT table, thousands of internal private devices can simultaneously share a single public IP address to access the internet.

## 14. IPv6 Address Types & Transmission (Exam Patch)
* **Anycast:** A "one-to-nearest" transmission type. Multiple devices share an IP, and the router finds the closest one.
* **No Broadcast:** IPv6 does not use broadcast traffic; it uses Multicast or Anycast instead.

### **Identification Table**
| Address Type | IPv6 Prefix | IPv4 Equivalent |
| :--- | :--- | :--- |
| **Global Unicast** | `2xxx:` or `3xxx:` | Public IP |
| **Link-Local** | `FE80::/10` | APIPA (169.254.x.x) |
| **Unique Local** | `FC00::/7` | Private IP (RFC 1918) |
| **Loopback** | `::1` | 127.0.0.1 |

## 15. Transition Mechanisms Detailed
* **Dual Stack:** Implementing both protocols on one device for seamless migration.
* **NAT64:** Translates between IPv6-only devices and IPv4-only servers.
* **Tunneling (6to4):** Encapsulates IPv6 packets inside IPv4 headers to cross old infrastructure.
