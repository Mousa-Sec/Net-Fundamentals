# CompTIA Network+ N10-009: Infrastructure, Protocols, and Ports

## 1. Networking Devices (1.2)
Understanding the physical and logical hardware used to connect, secure, and manage data across modern networks.

* **Routers:** Operate at Layer 3 (Network) of the OSI model. They connect different IP subnets and make forwarding decisions based on IP addresses. To process routing at wire speed, they rely on dedicated hardware called ASICs (Application-Specific Integrated Circuits).
* **Firewalls:**
  * *Traditional:* Filter traffic based on TCP or UDP port numbers and IP addresses.
  * *Next-Generation Firewalls (NGFW):* Perform deep packet inspection at Layer 7 (Application). They identify and block specific applications using signatures, rather than just relying on port numbers.
* **IDS and IPS:**
  * *IDS (Intrusion Detection System):* Monitors and alerts administrators to known inbound attacks (out-of-band).
  * *IPS (Intrusion Prevention System):* Actively blocks detected attacks in real-time (in-line). Today, these features are typically integrated directly into NGFWs (Unified Threat Management).
* **Load Balancers:** Distribute incoming network traffic across multiple servers to ensure high availability and reliability. They also optimize traffic via "TCP offloading," handling encryption/decryption overhead to free up backend server resources.
* **Proxy Servers:** Act as intermediaries for client requests, often caching web pages and filtering malicious content.
  * *Explicit Proxies:* Require manual OS or browser configuration.
  * *Transparent Proxies:* Operate invisibly without endpoint configuration.
* **Network Storage Access:**
  * *NAS (Network Attached Storage):* File-level access; editing a file usually requires downloading the whole file.
  * *SAN (Storage Area Network):* Block-level access; allows modifying just the changed data blocks of a massive file, highly efficient for enterprise databases and Virtual Machines.

## 2. Critical Protocols and Tunneling (1.4)
Protocols used for network diagnostics, management, and secure tunneling.

* **ICMP (Internet Control Message Protocol):** A core Layer 3 protocol used for network diagnostics and error reporting. Utilities like `ping` and `traceroute` rely on ICMP to verify device reachability. It does not carry standard user data.
* **GRE (Generic Routing Encapsulation):** A tunneling protocol that encapsulates various network layer protocols inside virtual point-to-point links. It is **unencrypted** but useful for passing routing protocols (OSPF, BGP) or multicast traffic over networks that wouldn't normally support them.
* **IPsec (Internet Protocol Security):** A Layer 3 protocol suite specifically designed to authenticate and encrypt packets. It provides secure, encrypted communication between networks (e.g., site-to-site VPNs) over a public IP network.

## 3. Demystifying Network Protocols (Context & Function)
To master these for the exam and the real world, you need to know *what* they are actually doing.

### File Transfer
* **FTP (Ports 20/21):** Think of this as a two-lane highway. Port 21 is the control lane (setting up the connection and rules), and Port 20 is the data lane (where the actual file moves). It is entirely unencrypted; anyone listening on the network can see your files and passwords.
* **SFTP (Port 22):** This solves FTP's security problem by wrapping the file transfer inside a Secure Shell (SSH) tunnel. Because it relies on SSH, it uses the SSH port (22).
* **TFTP (Port 69):** Trivial FTP. It uses UDP (connectionless), meaning it just throws the files at the destination without checking if they arrived. It requires no username or password. It's strictly used on local networks for simple tasks like pushing firmware updates to routers or network-booting (PXE) computers.

### Remote Access
* **Telnet (Port 23):** The ancient way to get a remote command-line interface on another machine. It is completely unencrypted. If you type a password, it flies across the network in plain text.
* **SSH (Port 22):** Secure Shell. The modern replacement for Telnet. It creates a heavily encrypted tunnel for you to manage routers, switches, or Linux servers remotely. 
* **RDP (Port 3389):** Remote Desktop Protocol. Unlike SSH which is command-line only, RDP is Microsoft's protocol for giving you a full, graphical desktop environment over the network.

### Email Operations
* **SMTP (Ports 25, 587):** Simple Mail Transfer Protocol. This is strictly for *sending* email from your device to a mail server, or between mail servers. Port 25 is unencrypted; 587 is used for encrypted submission.
* **POP3 (Port 110):** Post Office Protocol. Used for *receiving* email. It downloads the email to your local device and usually deletes it from the server. It is bad for multi-device syncing.
* **IMAP (Port 143):** Internet Message Access Protocol. Also used for *receiving* email, but it keeps the master copy on the server. If you read an email on your phone, it marks it read on your laptop. 

### Core Infrastructure
* **DNS (Port 53):** Domain Name System. The internet's phonebook. It translates human-friendly names (google.com) into computer-friendly IP addresses (142.250.190.46). It uses UDP for quick queries and TCP for large zone transfers.
* **DHCP (Ports 67/68):** Dynamic Host Configuration Protocol. Automatically assigns IP addresses, subnet masks, and default gateways to devices joining a network so you don't have to type them in manually.
* **NTP (Port 123):** Network Time Protocol. Keeps every device's clock perfectly synced to the millisecond. This is shockingly important; if a server's clock is 5 minutes off, security certificates will fail, and log files become impossible to correlate during a cyber attack.

### Web & Directory Services
* **HTTP (Port 80) / HTTPS (Port 443):** The foundation of web browsing. HTTP is unencrypted plain text. HTTPS adds a TLS/SSL encryption layer so your credit card data can't be stolen in transit.
* **LDAP (Port 389) / LDAPS (Port 636):** Lightweight Directory Access Protocol. Think of this as the corporate directory. It's how a network queries Active Directory to verify if "John Doe" has the correct password and permissions to access a specific folder. LDAPS is the secure, encrypted version.
* **SMB (Port 445):** Server Message Block. The standard Windows protocol for sharing files, folders, and printers across a local network.

### Management & Voice
* **SNMP (Ports 161/162):** Simple Network Management Protocol. Used to monitor network health. Port 161 is used by an admin server to ask a router, "How is your CPU usage?" Port 162 is used by the router to shout, "Alert! My fan just broke!" (These alerts are called "Traps").
* **Syslog (Port 514):** A standard way for network devices to send their event messages and error logs to a centralized logging server for security and troubleshooting.
* **SIP (Ports 5060/5061):** Session Initiation Protocol. The protocol that rings the phone, sets up the connection, and hangs up the call in VoIP (Voice over IP) telephone systems. 

## 4. The Quick-Reference Port Table
| Protocol | Port Number(s) | Transport | Description |
| :--- | :--- | :--- | :--- |
| **FTP** | 20, 21 | TCP | File Transfer (20 Data / 21 Control) - Insecure |
| **SSH** | 22 | TCP | Secure remote console / Encrypted tunnel |
| **SFTP** | 22 | TCP | Secure File Transfer (Runs over SSH) |
| **Telnet** | 23 | TCP | Unencrypted remote console (Legacy) |
| **SMTP** | 25, 587 | TCP | Sending email |
| **DNS** | 53 | TCP/UDP | Resolves domains to IP addresses |
| **DHCP** | 67, 68 | UDP | Auto-assigns IP configurations |
| **TFTP** | 69 | UDP | Trivial/Lightweight file transfer (No auth) |
| **HTTP** | 80 | TCP | Unencrypted web traffic |
| **POP3** | 110 | TCP | Receives/downloads email to local device |
| **NTP** | 123 | UDP | Synchronizes network clocks |
| **IMAP** | 143 | TCP | Receives/syncs email across devices |
| **SNMP** | 161, 162 | UDP | Network device monitoring and alerts |
| **LDAP** | 389 | TCP/UDP | Directory lookups (Active Directory) |
| **HTTPS** | 443 | TCP | Encrypted web traffic |
| **SMB** | 445 | TCP | Windows file and printer sharing |
| **Syslog** | 514 | UDP | Centralized event logging |
| **LDAPS** | 636 | TCP | Secure directory lookups |
| **MS-SQL** | 1433 | TCP | Microsoft SQL Server database access |
| **RDP** | 3389 | TCP | Windows Remote Desktop |
| **SIP** | 5060, 5061 | TCP/UDP | VoIP session setup and teardown |

---

## 5. Memorization Strategy: Transport Layer Grouping
Grouping protocols by how they transmit data makes memorizing the port numbers much more intuitive. 

| TCP (Connection-Oriented) | UDP (Connectionless) | Both (TCP / UDP) |
| :--- | :--- | :--- |
| **FTP:** 20, 21<br>**SSH:** 22<br>**SFTP:** 22<br>**Telnet:** 23<br>**SMTP:** 25, 587<br>**HTTP:** 80<br>**POP3:** 110<br>**IMAP:** 143<br>**LDAP:** 389<br>**HTTPS:** 443<br>**SMB:** 445<br>**LDAPS:** 636<br>**MS-SQL:** 1433<br>**RDP:** 3389 | **TFTP:** 69<br>**DHCP:** 67, 68<br>**NTP:** 123<br>**SNMP:** 161, 162<br>**Syslog:** 514 | **DNS:** 53<br>**SIP:** 5060, 5061 |


---

## 6. Web Filtering: Proxies vs. SWGs
Proxy servers operate at the Application Layer (Layer 7), allowing them to analyze full URLs rather than just IP addresses. 

* **How Proxies Filter Traffic:**
  * **URL Filtering:** Compares destination URLs against databases of categorized websites (e.g., blocking gambling or known malware hosts).
  * **Deep Content Inspection:** Opens data packets to scan for malicious code or phishing links before they reach the user.
  * **SSL/TLS Inspection:** Decrypts HTTPS traffic to scan content that would otherwise remain hidden.

* **Proxy vs. Secure Web Gateway (SWG)**
  While traditional proxies focus on privacy and basic site blocking, a **Secure Web Gateway (SWG)** is a dedicated security solution. SWGs provide comprehensive threat prevention, advanced malware scanning, sandboxing, and Data Loss Prevention (DLP).

## 7. Wireless Expansion: WAPs vs. Extenders
Expanding a wireless network requires understanding the "backhaul"—how the device communicates back to the main network.

| Feature | Wireless Access Point (WAP) | Wi-Fi Extender (Repeater) |
| :--- | :--- | :--- |
| **Backhaul Connection** | Wired (Ethernet cable) | Wireless (Wi-Fi) |
| **Performance** | Full speed (dedicated wired link) | Reduced speed (halves bandwidth to talk to router and client) |
| **Network Name (SSID)** | Seamless roaming (uses the same SSID) | Usually creates a new network (e.g., "Home_WiFi_EXT") |
| **Best For** | Enterprise, gaming, permanent setups | Quick fixes, small areas without Ethernet access |

## 8. Enterprise Wireless Infrastructure
In a professional environment, Access Points are rarely plugged directly into a router. Instead, they rely on Switches and Controllers.

### The Switch (The Muscle & Power)
Enterprise networks use switches to connect dozens or hundreds of APs.
* **Power over Ethernet (PoE):** APs typically do not plug into wall outlets. A PoE switch delivers both the data connection and the electrical power over a single Ethernet cable, making ceiling mounting easy.
* **Traffic Handling:** Switches handle local device-to-device traffic, freeing up the router to focus strictly on routing internet traffic and managing the firewall.

### The Wireless LAN Controller (The Brain)
Logging into 50 individual APs to change a Wi-Fi password is not scalable. A Wireless LAN Controller (WLC) provides a single management dashboard for the entire wireless fleet.

* **Centralized Configuration:** Push a single SSID, password, or security policy to all APs simultaneously.
* **Radio Resource Management (RRM):** Automatically adjusts AP channels and transmission power to prevent them from interfering with each other.
* **Seamless Roaming:** Manages the client "hand-off," ensuring devices smoothly transition from a weak AP to a stronger one without dropping connections.
* **Security:** Detects unauthorized "rogue" APs plugged into the network and monitors for interference.
* **Evolution of the WLC:** Controllers used to be physical hardware appliances in a server rack. Today, they are often **Cloud-Managed** (e.g., Cisco Meraki) or **Embedded**, where one physical AP acts as the master controller for the rest.

* ## 16. The IPsec Protocol Suite (Internal Mechanics)
To secure a VPN, IPsec relies on three specific sub-protocols:
* **AH (Authentication Header):** Provides **Integrity** and **Authentication**. It does NOT encrypt data.
* **ESP (Encapsulating Security Payload):** Provides **Confidentiality (Encryption)**. This is the part that hides the data.
* **IKE (Internet Key Exchange):** Manages the "handshake" and exchanges security keys to set up the tunnel.

## 17. GRE vs. IPsec Recap
* **GRE:** Can tunnel any protocol but offers **zero encryption** or security on its own.
* **IPsec:** Provides massive security but is limited in what it can tunnel (no multicast/routing protocols natively).
* **Solution:** "GRE over IPsec" combines the flexibility of GRE with the encryption of IPsec.
EOF
