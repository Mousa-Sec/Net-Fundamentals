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
