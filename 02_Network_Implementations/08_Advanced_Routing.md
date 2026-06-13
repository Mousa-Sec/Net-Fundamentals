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

# Interior Gateway Protocols (IGP)
* Definition: Routing protocols used to exchange routing information inside a single Autonomous System (your internal corporate office or campus network).

* Goal: To find the fastest path between local internal subnets and routers.

The Protocols (Classified by how they calculate paths):

### Distance-Vector (Hop Count):

* RIP (Routing Information Protocol): Calculates the best path purely by counting how many routers (hops) are in the way. Max hop count is 15. Uses **Hop Count** metric

### Link-State (Bandwidth/Speed):

* OSPF (Open Shortest Path First): Builds a complete map of the local network topology and calculates the fastest path based on cable speed (bandwidth). Very fast and scales well internally. Uses the **Cost** metric

* IS-IS (Intermediate System to Intermediate System): Functions similarly to OSPF; used heavily within large service-provider internal backbones.

### Advanced Distance-Vector (Hybrid):

* EIGRP (Enhanced Interior Gateway Routing Protocol): A Cisco-engineered protocol that looks at both bandwidth and line delay to find the best internal path.
* The Metric: Composite Metric (primarily Bandwidth and Delay).

# Exterior Gateway Protocols (EGP) / BGP
Definition: Routing protocols used to route data between different, independent Autonomous Systems. This is the routing infrastructure that runs the global Internet.

* The Only Protocol Used: BGP (Border Gateway Protocol).

* Note on Terminology: "EGP" was an old, specific protocol used at the dawn of the internet. It was completely replaced by BGP. Today, EGP is the category name, and BGP is the actual protocol used to fulfill it.

* How BGP Works (Path-Vector): BGP does not care about cable speed or hop count inside a company. It looks at the internet as a collection of massive corporate entities (Autonomous Systems). It routes traffic based on network path policies, business agreements, and security rules between ISPs.
* Metric: Path Attributes
 
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
