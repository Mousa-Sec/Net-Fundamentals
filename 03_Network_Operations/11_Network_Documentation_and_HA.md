# Day 11: Network Operations, Documentation, & HA (N10-009)
*Fact-Checked against Professor Messer N10-009 Objectives 3.1 & 3.3*

## 1. Network Documentation (Objective 3.1)
You cannot manage, secure, or troubleshoot a network if you don't know what it looks like or how it normally behaves.

* **Logical Network Diagrams:** Show the architecture and data flow of the network. They focus on IP subnets, VLANs, routing protocols, and firewall zones. They do *not* care about physical locations.
* **Physical Network Diagrams:** Show the exact physical locations of devices. They detail rack positions, specific port connections, cable runs, and building floor plans. Crucial for tracing a broken wire.
* **Standard Operating Procedures (SOPs):** Detailed, step-by-step instructions for routine tasks (e.g., configuring a new switch, onboarding a new user). Ensures consistency and prevents human error regardless of which engineer is doing the work.
* **Baselines:** A historical record of what the network looks like when it is operating "normally" (e.g., standard CPU utilization, average bandwidth usage). If you don't have a baseline, you cannot tell if a sudden spike in traffic is a normal business process or a cyber attack.

## 2. Reliability Metrics & Disaster Recovery (Objective 3.3)
Metrics used to calculate how long hardware will last, and how fast we can recover it when it inevitably breaks.

* **MTBF (Mean Time Between Failures):** The manufacturer's prediction of how long a device will operate before it fails. (Higher is better).
* **MTTR (Mean Time to Repair/Restore):** The average time it takes to fix a broken system and get it fully functional again. (Lower is better).
* **RTO (Recovery Time Objective):** The maximum amount of downtime the business can survive before suffering unacceptable losses.
* **RPO (Recovery Point Objective):** The maximum amount of *data loss* the business can survive (e.g., "We can only afford to lose the last 4 hours of data"). Dictates how often backups must be run.

### Recovery Sites
When the primary data center is destroyed, the business moves to a backup facility.
* **Hot Site:** An exact replica of the main data center. Data is continuously synced in real-time. Can take over operations in minutes. (Most expensive).
* **Warm Site:** Has the racks, power, and hardware pre-installed, but it does *not* have the current live data. Administrators must load the latest backups before the site is usable. Takes hours to days to spin up.
* **Cold Site:** Just an empty room with power and HVAC. No hardware, no data. Administrators must order new servers and build the network from scratch. Takes weeks to spin up. (Cheapest).

## 3. Network Redundancy & High Availability (Objective 3.3)
High Availability (HA) ensures there is no Single Point of Failure (SPOF) in the infrastructure.

* **Active-Active:** Multiple devices (like load balancers or firewalls) process traffic simultaneously. They share the workload. If one dies, the other simply takes over 100% of the traffic, often with zero dropped packets.
* **Active-Passive:** One device handles 100% of the live traffic while the secondary device sits in "standby" mode. The standby device continually monitors the primary, and only takes over if the primary fails.
* **FHRP (First Hop Redundancy Protocol):** (Tying back to Phase 2). Protocols like HSRP or VRRP that allow multiple physical routers to share a single Virtual IP (VIP). This provides gateway redundancy so end-users never lose their connection to the internet.

## 4. Extended Documentation & Asset Management (Exam Patch)
Professor Messer highlights several granular operational documents and tools that are critical for enterprise management:

* **Rack Diagrams:** A physical front-facing view of a server rack showing exactly which Rack Unit (U) a device occupies. Critical for instructing "remote hands" or technicians (e.g., "Reboot the server in Rack W, Unit 15").
* **Cable Maps:** Maps detailing the exact physical paths of wires above drop-ceilings or under raised floors, tracking a network drop at a specific desk back to a specific patch panel port in the IDF/MDF.
* **Layered Overlays:** Advanced diagrams that combine Layer 1 (physical interfaces/ports), Layer 2 (MAC addresses), and Layer 3 (IP addresses) into one comprehensive view.
* **Asset Management & Tags:** Using physical labels (barcodes or RFID) on hardware to track financial depreciation, verify warranty status, and quickly identify problem devices in trouble tickets.
* **IPAM (IP Address Management):** Centralized software used to plan, track, and monitor DHCP scopes and IPv4/IPv6 assignments. Crucial for auditing and security, as it tracks which specific user/device had a dynamic IP address at any given date and time.
* **SLA (Service Level Agreement):** A contractual guarantee between an organization and a service provider defining the absolute minimum acceptable level of service (e.g., guaranteeing 99.99% uptime or a maximum 4-hour response time to an outage).
* **Wireless Site Surveys & Heat Maps:** The process of auditing a physical space to chart frequency usage and identify interference. A **Heat Map** provides a visual, color-coded floor plan showing wireless signal strength and propagation.

## 5. Disaster Recovery Testing & Planning (Exam Patch)
* **DRP (Disaster Recovery Plan):** The comprehensive document detailing exactly how the organization will respond to and recover from an outage.
* **Tabletop Exercise:** A simulated test where personnel sit in a conference room and verbally step through a disaster scenario. Tests the logistics of the plan without the high cost and downtime of physically moving equipment.
* **Validation Testing:** A full-scale physical test of the DRP (e.g., actively failing over to the backup site) to document what works and what needs improvement.

## 6. Additional Documentation Details (Exam Patch)
* **Diagramming Software:** Standard third-party tools for creating physical and logical maps include Microsoft Visio, OmniGraffle, and Gliffy.
* **Asset Database Licensing:** Tracking physical hardware assets is mandatory for maintaining **Software License compliance** and avoiding legal/financial penalties from software vendors.
