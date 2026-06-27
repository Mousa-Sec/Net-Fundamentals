# 🌐 Networking Fundamentals & Network+ Engineering Ledger

A high-fidelity technical repository documenting advanced network engineering, packet analysis, cryptographic transport schemas, and architectural routing configurations modeled after the CompTIA Network+ framework.

---

## 🛠️ Core Engineering & Protocol Vectors Demonstrated

This repository serves as a practical testing ground showcasing core competencies in network security engineering, packet forensics, and enterprise infrastructure management:

* **Packet Analysis & Traffic Forensics:** Dissecting frame encodings, tracking raw TCP/UDP hex socket streams, analyzing multi-way handshakes, and diagnosing network anomalies using protocol analyzers (`Wireshark`, `tshark`).
* **Advanced IP Subnetting & Variable Length Subnet Masking (VLSM):** Dynamic IP allocation, subnet boundary calculation, and CIDR prefix optimization to minimize broadcast domains and maximize IP utilization efficiency.
* **Routing & Switching Architecture:** Implementing and auditing core routing protocols (OSPF, BGP, RIPv2), configuring Virtual LANs (VLANs/802.1Q), spanning-tree loop prevention (STP), and layer 3 routing boundary behaviors.
* **Core Network Infrastructure Daemons:** Managing and auditing standard operational protocol structures including DNS zone records, DHCP scope leases, SSH access criteria, and diagnostic ICMP utilities.
* **Security & Network Hardening Vectors:** Developing perimeter access control lists (ACLs), implementing stateful firewalls, deploying secure virtual private networks (IPsec/SSL VPNs), and mitigating active protocol-level exploits (ARP spoofing, DHCP starvation).

---

## 📂 Repository Architecture

The workspace utilizes a strictly organized, structured directory mapping. Each section is built as a standalone documentation hub featuring deep-dive theoretical concepts, actual PCAP/packet breakdown artifacts, and operational configuration blueprints:

```text
Net-Fundamentals/
├── 01_OSI_Model_&_Protocols/      # Deep-dive encapsulation, headers, and layers 1-7 mappings
├── 02_IP_Addressing_&_Subnetting/  # VLSM design matrices, CIDR notation, and supernetting labs
├── 03_Routing_&_Switching/        # VLAN trunking, inter-VLAN routing, OSPF configurations, & STP
├── 04_Network_Services/           # DNS zone transfers, DHCP handshakes (DORA), and SSH hardening
├── 05_Packet_Analysis_Labs/       # Wireshark PCAP analysis artifacts, TCP stream carving, and traffic logs
└── 06_Network_Security/           # Access Control Lists (ACLs), firewall profiles, and mitigation logs
