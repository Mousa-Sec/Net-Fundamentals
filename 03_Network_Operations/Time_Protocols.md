# CompTIA Network+ N10-009 — Objective 3.4: Time Protocols

### 1. Network Time Protocol (NTP)
* **Definition:** A system-wide time synchronization protocol that automatically aligns the clock settings of all local infrastructure nodes.
* **In-Bracket Definition:** `(A protocol that syncs the clocks of all network devices automatically to ensure identical system timestamps)`
* **Operational Benefit:** Critical for log analysis and event correlation across multiple switches, routers, and firewalls during security incident audits. Keeps time drift down to a few milliseconds.

### 2. NTP Architecture & Port Specifics
* **Layer 4 Vector:** Operates strictly over **UDP Port 123**.
* **Server and Client Roles:** Servers answer time requests, while clients pull updates. A single infrastructure asset can host both functions—using an internal client to query an upstream provider while hosting a server function to distribute time down to local subnets.

### 3. Security Risks and Network Time Security (NTS)
* **The Vulnerability:** Standard NTP is unencrypted. Falsifying time targets core authentication mechanisms like **Kerberos**—which automatically blocks network logins if system clocks drift apart by more than **5 minutes**—allowing attackers to trigger a wide-scale Denial of Service (DoS).
* **Network Time Security (NTS):** Introduces cryptographic authentication to validate time synchronization traffic.
* **In-Bracket Definition:** `(An encrypted authentication framework for NTP that prevents malicious actors from falsifying network time settings)`
* **Handshake Process:** Endpoints run a TLS handshake with an NTS Key Exchange Server to receive a secure cookie. The client then drops that cookie into its standard NTP request, prompting the NTP server to return an authenticated timestamp.

### 4. Precision Time Protocol (PTP)
* **Definition:** An ultra-exact time mapping mechanism designed for automation grids that require accuracy far exceeding standard NTP limits.
* **In-Bracket Definition:** `(A hardware-based time protocol that provides ultra-precise synchronization down to the nanosecond for industrial environments)`
* **Hardware Independence:** Achieves micro-granularity down to the **nanosecond** by utilizing dedicated, independent clock hardware. This isolates the time engine from host OS latency or application processing drops.


### 5. NTP Stratum Hierarchy and Request Logic
* **Definition:** A hierarchical tier network framework based on tracking distance metrics away from an absolute master timekeeper hardware asset. This tier value is defined as a **Stratum**.
* **In-Bracket Definition:** `(A hierarchical numbering system from 0 to 16 that tells you how close and reliable an NTP server is to an absolute master clock source)`

#### The Stratum Tier Rules:
* **Stratum 0:** Atomic clocks, GPS satellite arrays, and physical master tracking components. They create the time but are never directly assigned network IP addresses.
* **Stratum 1:** Servers wired directly via physical hardware connections to a Stratum 0 source.
* **Stratum 2:** Systems that issue time request queries across a standard IP network link to a Stratum 1 server.
* **Stratum 3:** Systems that query a Stratum 2 device over the network.

#### Request Routing Mechanics:
* **The Increment Rule:** Every network hop away from the primary clock source increases the assigned Stratum value by **+1**. (e.g., querying a Stratum 2 provider makes the requesting machine a Stratum 3 node).
* **The Scalability Limit:** The protocol allows synchronization hops to step down cleanly until hitting a maximum boundary threshold of **Stratum 15**.
* **Stratum 16 (Drop State):** Represents an invalid, **unsynchronized** state. If a server registers as a Stratum 16, or completely drops its upstream network connection link, it is flagged as unreliable and clients will ignore its time responses.






