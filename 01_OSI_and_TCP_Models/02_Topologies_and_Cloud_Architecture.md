# Day 2: Enterprise Topologies & Cloud Concepts (N10-009)

## Network Architecture Types
* **Star Topology:** All nodes connect to a central device (switch). Single point of failure at the center.
* **Mesh Topology:** All nodes connect to each other. High fault tolerance, high cost.
* **SD-WAN:** Software-Defined WAN. Dynamically routes traffic across multiple connection types (Internet, MPLS, 4G/5G) based on real-time performance.

## Cloud Networking (VPCs)
A **Virtual Private Cloud (VPC)** allows an enterprise to create a private, logically isolated network segment within a public cloud provider (like AWS or Azure). It uses the exact same subnetting and routing rules as a physical network.

## Software-Defined Networking (SDN) & IaC
Modern networks separate the decision-making from the actual data forwarding.
* **Control Plane:** The "brains." The centralized software controller making the routing decisions.
* **Data Plane:** The "muscle." The physical switches and routers executing the controller's instructions.

**Infrastructure as Code (IaC):** Replacing manual hardware configuration with code (e.g., Python, Terraform). This allows an entire network infrastructure to be deployed, modified, or destroyed via a single automated script.

## The Attack Surface
* **Targeting the Control Plane:** If the central SDN controller is compromised, the attacker dictates traffic flow for the entire network.
* **IaC Vulnerabilities:** A single typo in an automation script can deploy hundreds of misconfigured, vulnerable cloud firewalls simultaneously.

## Advanced Cloud Concepts (Exam Compass Patch)
* **SaaS:** Software for end-users (e.g., Office 365).
* **PaaS:** Platforms for developers to build apps.
* **Multitenancy:** Multiple customers sharing one software instance securely.
* **NSG (Network Security Group):** A firewall applied to a specific Virtual NIC (highly granular).
* **NSL / NACL (Network Security List):** A firewall applied to an entire Subnet (less granular).
* **Internet Gateway:** Allows 2-way internet traffic for cloud instances.
* **NAT Gateway:** Allows outbound internet access but restricts unsolicited inbound traffic.
* **Direct Connect:** A dedicated, private physical connection to a cloud provider (bypasses the public internet).

## 4. Network Topologies (Objective 1.6 - Transcript Verified)
* **Star / Hub-and-Spoke Topology:** All devices connect to a central device (like an Ethernet switch). Devices must communicate through the center hub to talk to each other.
* **Mesh Topology:** Devices connect over multiple parallel network connections. Provides extreme fault tolerance and the ability to load-balance traffic. Commonly used in Wide Area Networks (WANs).
* **Hybrid Topology:** A combination of multiple different architectures (e.g., a network that uses Star in one building, but Mesh to connect to another state).
* **Spine and Leaf (Data Center Architecture):** * *The Connection Rule:* Every Leaf switch connects to every Spine switch. 
  * *The Limitation:* Leaves **never** connect to other Leaves. Spines **never** connect to other Spines.
  * *Performance:* Ensures any device in the data center is never more than one hop (switch) away from any other device.
* **Top-of-Rack (ToR) Switching:** The physical layout of Spine and Leaf. A single Leaf switch is placed at the top of a server rack. All servers in the rack connect to it, keeping cabling clean and self-contained. Can become very expensive at massive scale.
* **Point-to-Point Topology:** A direct link between two locations. Common in legacy WANs (T1 / T3 lines) or linking two campus buildings directly together.
