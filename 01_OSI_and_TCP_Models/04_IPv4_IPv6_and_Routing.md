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
