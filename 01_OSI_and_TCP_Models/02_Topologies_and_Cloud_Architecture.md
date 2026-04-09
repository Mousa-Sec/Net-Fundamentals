# Day 2: Enterprise Topologies & Cloud Concepts (N10-009)

## Network Architecture Types
* **Star Topology:** All nodes connect to a central device (switch). Single point of failure at the center.
* **Ring Topology:** Nodes form a closed loop, each connecting to exactly two neighbors. Single break splits network.
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

## 5. Enterprise Network Architectures (Objective 1.6 - Transcript Verified)
* **Three-Tier Architecture:** A highly scalable and redundant hierarchical design used by large enterprises.
  * *Core Layer:* The ultra-fast backbone. Connects major sites and routes traffic to central data centers/servers.
  * *Distribution Layer:* The midpoint. Aggregates connections from all the access switches, enforces routing policies, and provides redundant paths to the core.
  * *Access Layer:* The network edge. The local switches that end-users, desktops, and printers physically plug into.
* **Collapsed Core Architecture:** A two-tier design for smaller networks. Combines the Core and Distribution layers into a single layer to save money and simplify troubleshooting, but severely reduces overall network redundancy.

## 6. Traffic Flow Classifications (Objective 1.6 - Transcript Verified)
* **North-South Traffic:** Traffic entering the network from an external source (the Internet) or leaving the network to an external destination. Crosses perimeter security boundaries.
* **East-West Traffic:** Traffic that stays entirely within the local data center (e.g., a local file server communicating with an internal image server). Requires internal micro-segmentation, as perimeter firewalls do not inspect this lateral traffic.

## 7. Cloud Routing & Architecture (Objective 1.3 - Transcript Verified)
* **VPC (Virtual Private Cloud):** A logically isolated virtual network within a public cloud environment where you deploy infrastructure (servers, load balancers, etc.).
* **Transit Gateway:** Acts as a central "cloud router." It connects multiple different VPCs together, allowing instances in separate VPCs to communicate with each other seamlessly.
* **VPC Endpoint:** creates a private, direct connection between your VPC and services within the same cloud provider (e.g., AWS S3, DynamoDB, or other AWS services) without crossing the public internet.
* **VPN Connection:** Allows remote physical sites or individual users to establish an encrypted tunnel directly into the private VPC over the internet.

## 8. Cloud Gateways & Firewalls (Objective 1.3 - Transcript Verified)
* **Internet Gateway (IGW):** A two-way connection. Allows cloud instances in a public subnet to be accessed from anywhere on the public internet (e.g., a public-facing web server).
* **NAT Gateway:** Translates private internal IPs to public IPs. Allows internal servers to reach out to the internet (for software updates), but explicitly restricts and drops unsolicited inbound connections from the outside.
* **Network Security List (NSL / NACL):** A broad cloud firewall. Rules applied here affect the **entire Subnet** or Virtual Cloud Network simultaneously. Lacks fine-grained control.
* **Network Security Group (NSG):** A highly granular cloud firewall. Rules are applied directly to a specific **Virtual NIC (Network Interface Card)** attached to a single virtual machine.

## 9. Cloud Deployment Models (Objective 1.3 - Transcript Verified)
* **Public Cloud:** Infrastructure is available to anyone over the public internet (managed by third-party providers like AWS or Azure).
* **Private Cloud:** A virtualized data center built and maintained exclusively for internal use by a single organization.
* **Hybrid Cloud:** A mix of Public and Private clouds. Often used when an organization wants the scalability of the public cloud but needs to keep highly sensitive data on-premises.

## 10. Cloud Service Models & The Responsibility Matrix (Objective 1.3 - Transcript Verified)
* **Software as a Service (SaaS):** On-demand software. The provider manages the entire application, underlying engine, and hardware. The user simply logs in. 
  * *Examples:* Office 365, Google Mail.
* **Platform as a Service (PaaS):** The provider manages the hardware and the underlying operating system engine. The customer is given the "building blocks" to develop and maintain their own custom applications. 
  * *Example:* Salesforce platform development.
* **Infrastructure as a Service (IaaS):** Also known as **Hardware as a Service (HaaS)**. The provider only manages the physical data center and virtual hardware. The customer is responsible for installing the OS, the software, and managing all security.
* **The Responsibility Rule:** No matter which cloud model is used, the customer is **always** responsible for the security of their own data, their user accounts (Identity/Directory), and their local end-user devices.

## 11. Software-Defined Networking (SDN) (Objective 1.8 - Transcript Verified)
Separates the traditional networking functions into three distinct, virtualized layers:
* **Data Plane (Infrastructure Layer):** The physical or virtual forwarding engine. Does the "heavy lifting" (forwarding frames, trunking, encrypting).
* **Control Plane (Control Layer):** The logic engine. Contains the routing tables, MAC address tables, and NAT tables that dictate *where* the Data Plane should send traffic.
* **Management Plane (Application Layer):** The administrative interface used to configure the device (e.g., SSH, Console connection, Web GUI).

## 12. SD-WAN Characteristics (Objective 1.8 - Transcript Verified)
A wide area network built to manage the complexities of decentralized cloud environments.
* **Application-Aware:** Identifies the specific application being used (e.g., cloud email vs. internal database) and dynamically routes it to the most efficient destination, bypassing central data center bottlenecks.
* **Transport Agnostic:** Can route traffic securely over any connection type simultaneously (Fiber, DSL, 5G/LTE).
* **Central Policy Management:** Administrators configure rules on a single "pane of glass" controller, which instantly pushes the updates to all SD-WAN routers globally.
* **Zero-Touch Provisioning:** Remote devices automatically configure and update themselves upon being plugged in, requiring no local IT intervention.
