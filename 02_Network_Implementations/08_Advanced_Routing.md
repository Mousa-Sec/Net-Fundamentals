# Day 8: Advanced Routing Implementations (N10-009)

* **The Routing Table:** The internal database a router uses to determine the "Next Hop" for a packet. If a router receives a packet destined for an IP address that is not in its routing table, the router will **discard** the packet.
## 1. Static vs. Dynamic Routing
### Static Routing
* **Manual Entry:** The administrator manually enters every route into every router. 
* **Benefits:** Zero overhead (no CPU/RAM used for updates), high security (no fake updates can be injected), and ideal for **Stub Networks** (remote sites with only one way in/out).
* **Drawbacks:** Difficult to manage at scale, no automatic failover if a link goes down, and prone to human configuration errors (like routing loops).

### Dynamic Routing
* **How it works:** Routers automatically discover paths and update each other using multicasts. 
* **Benefits:** Highly scalable and provides automatic rerouting if a link fails.
* **Drawbacks:** Requires router resources (CPU/RAM) and initial planning.

## 2. Dynamic Protocol Classifications
* **IGP (Interior Gateway Protocol):** Used *inside* an organization.
* **EGP (Exterior Gateway Protocol):** Used *between* organizations (The Internet).
  
  * **EIGRP (Enhanced Interior Gateway Routing Protocol):**
  * *Type:* Interior Gateway Protocol (IGP).
  * *Features:* Highly Cisco-centric. Offers very fast convergence and efficient routing updates.
    
* **BGP (Border Gateway Protocol or three napkins protocol):**
  * *Type:* External Gateway Protocol (EGP).
  * *Features:* The routing protocol of the global Internet. Used to connect entirely different Autonomous Systems together
  
  * * **OSPF (Open Shortest Path First):**
* *Use:* It builds a complete map of the network
  * *Type:* Interior Gateway Protocol (IGP) / Link-State.
  * *Features:* An open standard supported by almost all vendors.
  * *Metric:* Uses **Cost** (based on link bandwidth/speed) to determine the best path.
    
## 3. The Routing Table Decision Process
When a router evaluates where to send a packet, it follows this strict hierarchy to break ties:

1. **Longest Prefix Match:** The most specific subnet mask wins (e.g., a /32 route beats a /24 route).
2. **Administrative Distance (AD):** The "Trustworthiness" score. **LOWER IS BETTER**.
   * Connected: 0
   * Static Route: 1
   * EIGRP: 90
   * OSPF: 110
   * RIP: 120
3. **Metrics:** Used only to break a tie between two routes learned from the *same* protocol (e.g., the lowest OSPF Cost).

* **Autonomous System (AS):** A routed network under the control of a single administrative entity.

## 4. Redundancy & Virtualization
* **FHRP (First Hop Redundancy Protocol):** Allows multiple physical routers to share a single **Virtual IP (VIP)**. If the primary fails, the standby takes over the VIP so clients never lose their default gateway.
  * *Implementations:* **HSRP** (Cisco) and **VRRP** (Open Standard).
* **Subinterfaces:** Dividing one physical port into multiple logical ones (e.g., `G0/0.10`).
  * *Use Case:* **Router on a Stick**—using a single trunk link to route traffic between multiple VLANs.
