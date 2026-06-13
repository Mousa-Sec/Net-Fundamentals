# Day 2: Enterprise Topologies & Cloud Concepts (N10-009)

## Network Architecture Types
* **Star Topology:** All nodes connect to a central device (switch). Single point of failure at the center.
* **Ring Topology:** Nodes form a closed loop, each connecting to exactly two neighbors. Single break splits network.
* **Mesh Topology:** All nodes connect to each other. High fault tolerance, high cost.
* * **Hybrid Topology:** A combination of multiple different architectures (e.g., a network that uses Star in one building, but Mesh to connect to another state).
* **SD-WAN:** Software-Defined WAN. Dynamically routes traffic across multiple connection types (Internet, MPLS, 4G/5G) based on real-time performance.

## Cloud Networking (VPCs)
A **Virtual Private Cloud (VPC)** allows an enterprise to create a private, logically isolated network segment within a public cloud provider (like AWS or Azure). It uses the exact same subnetting and routing rules as a physical network.

## Software-Defined Networking (SDN) & IaC
Modern networks separate the decision-making from the actual data forwarding.
* **Control Plane:** The "brains." The centralized software controller making the routing decisions (forwarding frames, trunking, encrypting).
* **Data Plane:** The "muscle." The physical switches and routers executing the controller's instructions.Contains the routing tables, MAC address tables, and NAT tables that dictate *where* the Data Plane should send traffic.
* * **Management Plane (Application Layer):** The administrative interface used to configure the device (e.g., SSH, Console connection, Web GUI).

# CompTIA Network+ N10-009 — Objective 1.8: Infrastructure as Code (IaC)

### Introduction to Infrastructure as Code (IaC)
* **Definition:** A practice where entire systems, application packages, and core networking architectures (routers, switches, firewalls) are described and configured inside readable text file scripts.
* **In-Bracket Definition:** `(Managing and provisioning an entire corporate network and server setup using code and text files rather than doing it by hand)`
* **Operational Benefit:** Replaces manual individual hardware installations. To spin up matching duplicate instances in a distant data center, you run the exact same script to stand up a mirror infrastructure.

### Infrastructure as Code (IaC) Template
An IaC template is a human-readable, static text file—written in a highly structured configuration language like YAML, JSON, or HCL—that serves as the complete and authoritative blueprint for an entire IT environment. It explicitly defines the desired state of all configuration settings, hardware specifications, network designs, storage layouts, and security policies.

### Playbooks and Automation
* **Definition:** A strict, pre-arranged sequence of automated steps implemented to resolve security alerts or maintain systems.
* **In-Bracket Definition:** `(A step-by-step automated script or workflow that automatically fixes or manages a system problem when triggered)`
* **Incident Handling Example:** Playbooks allow automated responses to malware alerts. The script can drop the infected endpoint, purge the disks, apply a secure base operating system image, and re-launch the system automatically.
* **SOAR Platforms:** Playbooks are centralized and deployed within a Security Orchestration, Automation, and Response (SOAR) management console for unified tracking.

### Benefits of Infrastructure as Code
* **Configuration Drift Prevention:** Overwrites minor unauthorized updates or modifications, pulling systems back to a uniform, standard compliance baseline.
* **Environment Synchronization:** Ensures testing environments exactly replicate live production layouts to eliminate configuration errors during application rollouts.
* **Automated Upgrades:** Editing lines inside an IaC master file enables systems to dynamically target and apply software adjustments or interface overrides.
* **Live System Documentation:** The script text describes active compute properties, acting as an accurate, up-to-date document of your infrastructure state.

### Source Control and Versioning System (Git)
* **Definition:** Utilizing a Central Repository system (such as Git) to manage changes, audit history, and track edits made to IaC files across massive multi-user teams.
* **In-Bracket Definition:** `(A central system like Git that tracks software changes and allows multiple administrators to work on the same file without breaking it)`
* **Branching & Merging:** Engineers split copies of production templates into "branches" to test upgrades safely. Once verified, branches are integrated back into the master config through a "merge" operation.
* **Conflict Management:** Identifies instances where multiple engineers try to submit different revisions to the same line of code, stopping deployment until an administrator resolves the structural conflict.

## The Attack Surface
* **Targeting the Control Plane:** If the central SDN controller is compromised, the attacker dictates traffic flow for the entire network.
* **IaC Vulnerabilities:** A single typo in an automation script can deploy hundreds of misconfigured, vulnerable cloud firewalls simultaneously.

## 4. Spine and Leaf Archeticture Objective 1.6 
* **Spine and Leaf (Data Center Architecture):** * *The Connection Rule:* Every Leaf switch connects to every Spine switch. 
  * *The Limitation:* Leaves **never** connect to other Leaves. Spines **never** connect to other Spines.
  * *Performance:* Ensures any device in the data center is never more than one hop (switch) away from any other device.
* **Top-of-Rack (ToR) Switching:** The physical layout of Spine and Leaf. A single Leaf switch is placed at the top of a server rack. All servers in the rack connect to it, keeping cabling clean and self-contained. Can become very expensive at massive scale.
* **Point-to-Point Topology:** A direct link between two locations. Common in legacy WANs (T1 / T3 lines) or linking two campus buildings directly together.

## 5. Enterprise Network Architectures Objective 1.6
* **Three-Tier Architecture:** A highly scalable and redundant hierarchical design used by large enterprises.
  * *Core Layer:* The ultra-fast backbone. Connects major sites and routes traffic to central data centers/servers.
  * *Distribution Layer:* The midpoint. Aggregates connections from all the access switches, enforces routing policies, and provides redundant paths to the core.
  * *Access Layer:* The network edge. The local switches that end-users, desktops, and printers physically plug into (Provides the physical or wireless connections for end devices to access the network). Ensures that critical traffic receives priority over less important traffic and  implements security measures to control network access
* **Collapsed Core Architecture:** A two-tier design for smaller networks. Combines the Core and Distribution layers into a single layer to save money and simplify troubleshooting, but severely reduces overall network redundancy.

## 6. Traffic Flow Classifications Objective 1.6
* **North-South Traffic:** Traffic entering the network from an external source (the Internet) or leaving the network to an external destination. Crosses perimeter security boundaries.
* **East-West Traffic:** Traffic that stays entirely within the local data center (e.g., a local file server communicating with an internal image server). Requires internal micro-segmentation, as perimeter firewalls do not inspect this lateral traffic.

## 7. Cloud Routing & Architecture Objective 1.3
* **Transit Gateway:** Acts as a central "cloud router." It connects multiple different VPCs together, allowing instances in separate VPCs to communicate with each other seamlessly.
* **VPC Endpoint:** creates a private, direct connection between your VPC and services within the same cloud provider (e.g., AWS S3, DynamoDB, or other AWS services) without crossing the public internet.
* **VPN Connection:** Allows remote physical sites or individual users to establish an encrypted tunnel directly into the private VPC over the internet.

## 8. Cloud Gateways & Firewalls Objective 1.3
* **Multitenancy:** Multiple customers sharing one software instance securely.
  * **In-Bracket Definition:** `(A flexible computing model where multiple users share physical hardware and scale resources dynamically)`
* **Elasticity:** The ability to scale system resources up/out during traffic spikes and scale them down/in dynamically when demand decreases to manage expenses.
 * **NSG (Network Security Group):** A firewall applied to a specific Virtual NIC (highly granular) for strict device-level isolation.
   * **In-Bracket Definition:** `(A precise firewall policy applied directly to individual virtual network cards)`
    **Operational Benefit:** Grants high administrative granularity across the exact same subnet, but increases overall management overhead.
* **NSL / NACL (Network Security List):** A firewall applied to an entire Subnet (less granular).
  * **In-Bracket Definition:** `(A firewall list that applies security rules broadly across entire cloud subnets)`
  **Filtering Criteria:** Rules intercept TCP/UDP ports (e.g., 443, 22) and target source/destination IP variables using standard CIDR block notation.
* **Internet Gateway (IGW):** A two-way connection. Allows cloud instances in a public subnet to be accessed from anywhere on the public internet (e.g., a public-facing web server). Allows 2-way internet traffic for cloud instances.
* **Network Security Group (NSG):** A highly granular cloud firewall. Rules are applied directly to a specific **Virtual NIC (Network Interface Card)** attached to a single virtual machine.
* **VPC NAT Gateway:** Translates private internal IPs to public IPs. Allows internal servers to reach out to the internet (for software updates), but explicitly restricts and drops unsolicited inbound connections from the outside. In short, allows outbound internet access but restricts unsolicited inbound traffic.
* **Direct Connect:** A dedicated, private physical connection to a cloud provider (bypasses the public internet).

### Network Function Virtualization (NFV)
* **Definition:** Emulating physical network appliances (routers, switches, firewalls) purely as software-based virtual components within a hypervisor system.
* **In-Bracket Definition:** `(Replacing physical network hardware like routers and firewalls with virtual software instances)`
* **Key Concept:** Consolidates hundreds of physical servers and hardware network devices down into virtual instances running inside unified, high-powered hardware nodes.

## 9. Cloud Deployment Models Objective 1.3
* **Public Cloud:** Infrastructure is available to anyone over the public internet (managed by third-party providers like AWS or Azure).
* **Private Cloud:** A virtualized data center built and maintained exclusively for internal use by a single organization.
* **Hybrid Cloud:** A mix of Public and Private clouds. Often used when an organization wants the scalability of the public cloud but needs to keep highly sensitive data on-premises.

## 10. Cloud Service Models & The Responsibility Matrix Objective 1.3
* **Software as a Service (SaaS):** On-demand software. The provider manages the entire application, underlying engine, and hardware. The user simply logs in. 
  * *Examples:* Office 365, Google Mail.
* **Platform as a Service (PaaS):** The provider manages the hardware and the underlying operating system engine. The customer is given the "building blocks" to develop and maintain their own custom applications. 
  * *Example:* Salesforce platform development.
* **Infrastructure as a Service (IaaS):** Also known as **Hardware as a Service (HaaS)**. The provider only manages the physical data center and virtual hardware. The customer is responsible for installing the OS, the software, and managing all security.
* **The Responsibility Rule:** No matter which cloud model is used, the customer is **always** responsible for the security of their own data, their user accounts (Identity/Directory), and their local end-user devices.

## 11. SD-WAN Characteristics Objective 1.8
A wide area network built to manage the complexities of decentralized cloud environments.
* **Application-Aware:** Identifies the specific application being used (e.g., cloud email vs. internal database) and dynamically routes it to the most efficient destination, bypassing central data center bottlenecks.
* **Transport Agnostic:** Can route traffic securely over any connection type simultaneously (Fiber, DSL, 5G/LTE).
* **Central Policy Management:** Administrators configure rules on a single "pane of glass" controller, which instantly pushes the updates to all SD-WAN routers globally.
* **Zero-Touch Provisioning:** Remote devices automatically configure and update themselves upon being plugged in, requiring no local IT intervention.

# CompTIA Network+ N10-009 — Objective 1.8: Virtual Extensible LAN (VXLAN)

### The Problem: Data Center Interconnection (DCI)
* **Definition:** Linking separate data centers together so applications can scale, run, or migrate globally without being limited by local IP layouts or physical media.
* **The Challenge:** Different data centers use conflicting IP configurations and transport media (fiber, copper, Metro Ethernet), making seamless application transitions difficult.

### Understanding VXLAN
* **Definition:** An advanced encapsulation standard that runs virtual Layer 2 overlay networks across a physical Layer 3 routable network.
* **In-Bracket Definition:** `(A method that stretches a Layer 2 network over a routable Layer 3 network by hiding Ethernet frames inside IP packets)`
* **VLAN vs. VXLAN Scaling Limits:**
  * **VLANs:** Layer 2 bounded, non-routable, 12-bit ID tag with a maximum cap of ~4,000 virtual networks.
  * **VXLANs:** Layer 3 routable, 24-bit ID tag allowing up to 16 million virtual networks; tailored for large multi-tenant cloud data centers.

### VXLAN Architecture & Components
* **VXLAN Tunnel Endpoint (VTEP):** The physical switch or virtual host function that manages tunnel creation, packet encapsulation, and decapsulation.
  * **In-Bracket Definition:** `(The device on either end of the tunnel that packages and unpackages the data)`
  * **Addressing:** Each VTEP holds a distinct, routable Layer 3 IP address to cross the core infrastructure network.
* **VXLAN Network Identifier (VNI):** The specific 24-bit tag assigned to a discrete virtual network domain to isolate tenant traffic.
  * **In-Bracket Definition:** `(The unique ID code used to separate different virtual networks inside the tunnel)`

### The Encapsulation Process
* **Data Flow:**
  1. The host VM builds a standard Ethernet frame (Ethernet Header + IP Header + Payload).
  2. The source VTEP wraps the frame inside a **VXLAN Header** (containing the VNI tag), nests it into a **UDP Packet**, and applies an **Outer IP Header** addressed directly to the destination VTEP.
  3. The packet routes over standard Layer 3 routers.
  4. The receiving VTEP strips away the outer wrapper and drops the clean, original Layer 2 frame straight into the destination host system.
 

# CompTIA Network+ N10-009 — Objective 1.8: Zero Trust

### Core Concept of Zero Trust
* **Definition:** A holistic security architecture built on the premise that every user, device, and application is inherently untrusted, regardless of whether they reside inside or outside the network boundary.
* **In-Bracket Definition:** `(A security model where no user or device is trusted by default, requiring continuous verification at every step)`
* **Key Requirements:** Moves away from perimeter-only focus by deploying internal security safeguards, including micro-segmentation, encryption, multi-factor authentication (MFA), and constant monitoring.

### Adaptive Identity (Policy-Based Authentication)
* **Definition:** A dynamic access management approach that analyzes multiple real-time contextual risk indicators before approving a login request.
* **In-Bracket Definition:** `(Checking who, where, and how a user is logging in to adjust security requirements dynamically)`
* **Risk Factors Assessed:**
  * **User History:** Distinguishes between internal staff members and external vendors.
  * **Geographical Origin:** Scans for standard local logins vs. sudden foreign connection sources.
  * **Connection Vector:** Reviews the source IP address, network type, and time of day.
* **Adaptive Rule Execution:** A user logging in during regular business hours from the office may face low friction, while a connection from an unusual location or at odd hours prompts a forced denial or an elevated MFA challenge.

### Context-Aware Authorization & Least Privilege
* **Definition:** Tailoring an authenticated user's exact operational permissions based on their role, device state, and location metrics.
* **In-Bracket Definition:** `(Limiting what a user can see and do based on their job role and current security risk level)`
* **Principle of Least Privilege:** Restricting users to the absolute minimum levels of system access essential for their job functions. 
* **Malware Mitigation:** Limiting broad administrative privileges helps block malware from inheriting deep system access rights, preventing lateral infection across the enterprise network.

### SASE (Secure Access Service Edge)
* **Definition:** A decentralized, cloud-native architecture that combines software-defined networking with advanced cybersecurity features directly at the service edge.
* **In-Bracket Definition:** `(A cloud-based system that combines network routing and security services to protect users working from anywhere)`
* **Core Pillars:**
  * **Network as a Service (NaaS):** Manages global traffic engineering, routing, and Quality of Service (QoS).
  * **Security as a Service (SECaaS):** Implements Zero Trust Network Access (ZTNA), Firewall as a Service (FWaaS), and DNS security inline.
* **Operational Workflow:** Endpoints utilize an automated SASE client to run secure, policy-enforced cloud tunnels in the background without requiring manual user activation.
