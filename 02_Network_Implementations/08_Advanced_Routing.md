# Day 8: Advanced Routing Implementations (N10-009)
*Objective 2.1: Compare and contrast routing technologies and bandwidth management concepts.*

## 1. Dynamic Routing Protocols (IGP vs. EGP)
Dynamic routing allows routers to automatically discover network paths and automatically failover if a cable is cut, eliminating the need for manual static route configuration.
* **IGP (Interior Gateway Protocol):** Used to route traffic *inside* a single Autonomous System (AS) - e.g., your company's internal corporate network.
* **EGP (Exterior Gateway Protocol):** Used to route traffic *between* different Autonomous Systems - e.g., connecting your corporate network to the ISP, or the ISP to the broader internet.

## 2. OSPF (Open Shortest Path First)
* **Type:** IGP / Link-State Protocol.
* **How it works:** Every router builds a complete topological map of the entire network. If a link goes down, OSPF recalculates the math instantly.
* **Metric:** Uses **Cost**. Cost is calculated based on the bandwidth (speed) of the link. A 10 Gbps fiber link has a much lower cost (and is therefore more preferred) than a 1 Gbps copper link.
* **Areas:** OSPF networks are divided into "Areas" to keep routing tables small and efficient. All traffic passing between different areas *must* go through **Area 0** (The Backbone Area).

## 3. BGP (Border Gateway Protocol)
* **Type:** EGP / Path-Vector Protocol.
* **How it works:** BGP is the routing protocol that runs the global internet. It does not look at link speed; it looks at the path of Autonomous Systems (ASNs) it must cross.
* **Metric:** Uses **Path Attributes** (specifically, AS Path length). It prefers the route that hops through the fewest number of distinct ISP networks.

## 4. Route Selection & Tie-Breakers
When a router receives a packet, it might have three different ways to get to the destination. It decides the path based on this strict top-down hierarchy:
1. **Longest Prefix Match:** The router always chooses the most specific subnet mask first. A route to `10.1.1.0 /24` will always beat a broad route to `10.0.0.0 /8`.
2. **Administrative Distance (AD):** If the prefix lengths are identical, the router trusts the protocol with the lowest AD (trustworthiness score). 
   * Directly Connected = 0
   * Static Route = 1
   * OSPF = 110
3. **Metric:** If two routes have the exact same Prefix Length and the exact same AD (e.g., both are learned via OSPF), the router looks at the protocol's internal metric (e.g., the lowest OSPF Cost).

## 5. First Hop Redundancy Protocol (FHRP)
If the default gateway (the main router) for a VLAN dies, the entire subnet loses internet access. FHRP fixes this.
* **How it works:** Two physical routers are clustered together and share a single "Virtual IP" (VIP) and a virtual MAC address. 
* **The Failover:** The PCs point to the VIP. If the primary physical router catches fire, the standby router instantly takes over the VIP. The PCs never know a failure occurred. 
* *Common FHRP implementations:* **VRRP** (Vendor neutral) and **HSRP** (Cisco proprietary).

## 1. Static vs. Dynamic Routing
### Static Routing
* **How it works:** The administrator manually enters every route into every router. 
* **Benefits:** Very low overhead (no CPU/RAM used for updates), high security (no fake updates can be injected), and ideal for **Stub Networks** (remote sites with only one way in/out).
* **Drawbacks:** Difficult to manage at scale, no automatic failover if a link goes down, and prone to human configuration errors (like routing loops).

### Dynamic Routing
* **How it works:** Routers automatically discover paths and update each other on changes using multicasts. 
* **Benefits:** Highly scalable and provides automatic rerouting if a link fails.
* **Drawbacks:** Requires router resources (CPU/RAM) and initial complex planning.

## 2. Common Dynamic Protocols
* **EIGRP (Enhanced Interior Gateway Routing Protocol):** Largely Cisco-proprietary. Very efficient, low overhead, and fast convergence.
* **OSPF (Open Shortest Path First):** A non-proprietary **Link-State** protocol. It builds a complete map of the network and uses **Cost** as its metric, which is based on bandwidth/speed.
* **BGP (Border Gateway Protocol):** Known as the "three napkins protocol". It is the **Exterior Gateway Protocol (EGP)** that runs the global internet.

## 3. The Routing Table Decision Process
When a router evaluates where to send a packet, it follows this strict "tie-breaking" hierarchy:

1. **Longest Prefix Match:** The most specific subnet mask always wins. 
   * Example: A route to `192.168.1.6/32` will beat a route to `192.168.1.0/24`.
2. **Administrative Distance (AD):** If the prefix length is a tie, the router trusts the protocol with the lowest AD:
   * **Connected:** 0
   * **Static Route:** 1
   * **EIGRP:** 90
   * **OSPF:** 110
   * **RIP:** 120
3. **Metrics:** Used only to break a tie between two routes learned from the *same* protocol (e.g., two OSPF routes with different costs).

## 4. Redundancy & Virtualization
* **FHRP (First Hop Redundancy Protocol):** Allows two physical routers to share a single **Virtual IP (VIP)**. Clients point to the VIP so that if one physical router fails, the other takes over seamlessly.
  * *Implementations:* **HSRP** (Cisco) and **VRRP** (Open Standard).
* **Subinterfaces:** Dividing one physical port into multiple virtual ones (e.g., `G0/1.10`, `G0/1.20`).
  * *Use Case:* **Router on a Stick**—allowing a single cable to route traffic between multiple VLANs via a trunk link.
