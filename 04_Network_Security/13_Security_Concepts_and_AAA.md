
## 1. Data State Categorization
Understanding how data is handled based on its current state is vital for applying the correct security controls.
* **Data in Transit (Data in Motion):** Data actively moving across a wired or wireless network.
  * *Security Focus:* Protection via encryption protocols like **TLS** (web/app traffic) and **IPsec** (VPN/tunnel traffic).
  * *Hardware Role:* Switches and routers focus on forwarding; firewalls and IPS focus on monitoring/blocking malicious transit.
* **Data at Rest:** Data stored on physical media (HDDs, SSDs, Flash drives, or Tape).
  * *Security Focus:* Protection via **Full Disk Encryption (FDE)**, database encryption, or individual file/folder encryption.
  * *Access Controls:* Use of **ACLs (Access Control Lists)** within the OS to restrict who can read/write stored data.
* **Data in Use (Contextual):** Data currently residing in RAM, CPU registers, or cache while being processed.

## 2. Public Key Infrastructure (PKI) & Trust Models
The management framework for digital certificates and asymmetric encryption.
* **PKI Components:** The set of hardware, software, people, and policies needed to create, manage, distribute, use, store, and revoke digital certificates.
* **Certificate Authority (CA):** The ultimate source of trust.
  * *Public CA:* Third-party authorities (e.g., DigiCert) used for internet-facing trust.
  * *Private/Internal CA:* Organizations build their own CA to sign certificates for internal servers and employees.
* **Trust Models:**
  * *Hierarchical:* All trust flows down from a central Root CA.
  * *Web of Trust:* A distributed model where if A trusts B, and B trusts C, then A can trust C.

## 3. Identity and Access Management (IAM) Concepts
The centralized management of digital identities and their associated permissions.
* **Principle of Least Privilege:** Users are granted the **minimum** level of access necessary to perform their job, and nothing more. (e.g., Standard users should never have local Admin rights).
* **Role-Based Access Control (RBAC):** (Reinforcement) Users are assigned to groups (Roles) based on their department or job function (e.g., "Accounting_Group"). Permissions are assigned to the group, not the individual.

## 4. Geographic Security (Geo-Fencing)
Using location data as a functional security constraint.
* **Location Identification:** Determining a user's position via **IP Address** (less accurate), **GPS** (highly accurate mobile), or **Wireless SSID** proximity.
* **Geo-Fencing:** Dynamically changing permissions based on physical location. 
  * *Example:* A user can only access "Top Secret" files while their GPS shows they are physically inside the Corporate Headquarters building.

## 5. Physical Security & Surveillance
Physical barriers are the first line of defense in the CIA triad (Availability).
* **CCTV (Video Surveillance):** Modern networked cameras provide motion detection, facial recognition, and license plate reading. Feeds are centralized for historical auditing.
* **Electronic Door Locks:**
  * *Methods:* PIN codes, RFID badges, and Biometrics (handprints, retina scans).
  * *Two-Factor Physical Access:* Requiring both an RFID badge (Something you have) AND a PIN (Something you know) to enter a server room.


## 6. The AAA Framework
When a user attempts to access a network, the system must process them through three distinct phases:
* **Identification:** Providing a piece of information that is publicly available to identify who you are (e.g., a username or email address).
* **Authentication (Who are you?):** Proving you are that person using private information (e.g., a password or MFA token).
* **Authorization (What are you allowed to do?):** Checking permissions. Just because you authenticated successfully doesn't mean you have the rights to format the core router.
* **Accounting (What did you do?):** Tracking and logging the user's actions for auditing and non-repudiation (e.g., Syslog tracking who deleted a specific file at 2:00 AM).

## 7. Single Sign-On (SSO) & LDAP
* **SSO:** A user authenticates once (e.g., logging into their Windows machine in the morning) and is automatically granted access to all other trusted network resources (email, databases, intranet) without having to type their password again. Often uses protocols like **Kerberos** or **SAML**.
* **LDAP (Lightweight Directory Access Protocol):** (TCP 389 / Secure TCP 636). The protocol used to query and update the central directory of users and devices (like Microsoft Active Directory).


## 8. Centralized Authentication Protocols (Exam Focus)
Instead of putting a username/password database on every single switch and router, networks use a central AAA server.
* **RADIUS (Remote Authentication Dial-In User Service):**
  * *Transport:* Uses **UDP** (Ports 1812/1813 or 1645/1646).
  * *Encryption:* Only encrypts the **password** in the packet. The username and rest of the payload are sent in plain text.
  * *AAA Handling:* Combines Authentication and Authorization into a single process.
* **TACACS+ (Terminal Access Controller Access-Control System Plus):**
  * *Transport:* Uses **TCP** (Port 49).
  * *Encryption:* Encrypts the **entire payload** (username, password, and commands).
  * *AAA Handling:* Strictly separates Authentication, Authorization, and Accounting. Highly favored in Cisco-heavy enterprise environments for granular command control.


*Purple Team Perspective:* RADIUS is notoriously vulnerable to offline brute-force attacks if the shared secret is weak. If you sniff RADIUS traffic on the wire, the plaintext usernames provide immediate targets, and the lightly hashed passwords can be cracked to gain network access.

## 9. LDAP Deep-Dive (X.500 & Attributes)
LDAP is the "phone book" of the network, using the **X.500 standard** to organize objects into a searchable hierarchy.
* **Attributes:** Key-value pairs used to provide context to an object.
  * *CN (Common Name):* The name of the specific device or user (e.g., "widget-web-01").
  * *OU (Organizational Unit):* The department managing the device (e.g., "Marketing").
  * *O (Organization):* The company name (e.g., "WidgetCorp").
  * *L (Locality):* The physical city (e.g., "London").
  * *DC (Domain Component):* The network domain (e.g., "widget", "com").
* **The Tree Structure:**
  * **Containers:** Higher-level objects that hold other items (e.g., Countries, Departments).
  * **Leaf Objects:** The actual end-points (e.g., individual Users, Printers, Servers).

## 10. SAML (Security Assertion Markup Language)
An open standard used specifically for **Web-Based Authentication and Authorization**.
* **The Flow:**
  1. **The Client (Principal):** The user with a web browser.
  2. **The Resource Server (Service Provider):** The application the user wants to access.
  3. **The Authorization Server (Identity Provider):** The central server that verifies the user and issues a **Token**.
* **Limitation:** SAML was originally built for web browsers and was not natively designed for mobile applications.

## 11. RADIUS vs. TACACS+ (Final Review Table)
| Feature | RADIUS | TACACS+ |
| :--- | :--- | :--- |
| **Protocol** | UDP (1812/1813) | TCP (49) |
| **Encryption** | Only the Password | **Entire Payload** |
| **AAA Model** | Combines Auth/Authz | Separates Auth/Authz/Acct |
| **Origin** | Open Standard | Cisco-Centric (Now Open) |


## 12. Multi-Factor Authentication (MFA)
To securely authenticate, systems require multiple *categories* of proof. Using two passwords is NOT multi-factor; it is just two instances of "Something you know."
* **Something you know:** A password, PIN, or security question.
* **Something you have:** A smart card, USB token (YubiKey), or an authenticator app on your phone.
* **Something you are:** Biometrics (Fingerprint, retina scan, facial recognition).
* **Somewhere you are:** Geolocation tracking (e.g., denying a login from Russia if the user's phone GPS shows they are in Dubai).
* **Something you do:** Behavioral biometrics (Typing speed, signature dynamics).

## 13. TOTP (Time-based One-Time Password)
The "Something you have" factor typically found in authenticator apps (Google/Microsoft Authenticator).
* **How it works:** Uses a **Secret Key** (shared between client and server) and the **Current Time** to generate a pseudo-random 6-digit code every 30 seconds.
* **Critical Requirement:** **NTP (Network Time Protocol)**. Because the code is based on time, if the client's phone and the server's clock are out of sync, the code will be rejected as invalid.


## 14. Deception Technologies 
Attackers are often automated scripts or programs rather than humans. Deception tools are used to study their behavior and misdirect them.
* **Honeypot:** A single virtual server, workstation, or file designed to look attractive and vulnerable to an attacker. Its sole purpose is to act as a "trap" to observe the attacker's tools and methods.
* **Honeynet:** A more complex, high-interaction deception network composed of multiple honey-devices (virtual routers, switches, servers, and firewalls). It simulates an entire enterprise environment to trick the attacker into thinking they are moving laterally through a real corporate network.
* **Purpose:** Honeypots/nets provide early warning of new "Zero-Day" attacks and help researchers build better signatures for defensive tools (IDS/IPS).

## 15. The Risk Management Lifecycle (Exam Focus)
In security, a "threat" is not the same as a "risk." Use these precise definitions for the exam:
* **Vulnerability:** A specific weakness or flaw in a system (e.g., an unpatched OS, a default password, or a coding error like a Buffer Overflow). 
* **Threat:** A potential danger that could take advantage of a vulnerability. Threats can be **Intentional** (an attacker) or **Accidental** (a flood, fire, or an employee mistake).
* **Threat Agent (Threat Actor):** The specific entity carrying out the threat (e.g., a state-sponsored hacker group or a malicious insider).
* **Exploit:** The specific action or piece of code used to actually take advantage of a vulnerability. 
* **Risk:** The overall mathematical probability that a threat will successfully exploit a vulnerability, resulting in harm to the organization.


## 16. Security Posture Assessment
* **Risk Identification:** The process of documenting potential dangers before they happen. Organizations must constantly assess risk when adding new applications or changing network configurations.
* **Business Decision Impact:** Security is a balance. If the cost of a security control is higher than the value of the data it protects, a business may choose to **Accept** the risk. 

## 17. CIA Triad: Practical Application Summary
* **Confidentiality:** Preventing unauthorized disclosure of data. (e.g., Encryption, Access Controls, Steganography).
* **Integrity:** Ensuring data has not been altered, tampered with, or corrupted. (e.g., Hashing, Digital Signatures, Certificate Authority).
* **Availability:** Ensuring data and systems are accessible to authorized users when needed. (e.g., Redundancy, Load Balancing, UPS, DDoS protection).

* ** CIA Scenario Examples
* **Confidentiality:** If a breach occurs and data is leaked, this leg is broken. *Countermeasure: Encryption.
* **Integrity:** If an attacker changes a bank balance or modifies a configuration file, this leg is broken. *Countermeasure: Digital Signatures & Hashing.*
* **Availability:** If a DDoS attack or a hardware failure takes the website offline, this leg is broken. *Countermeasure: Redundancy & Backups.*

	
## 18. Regulatory Compliance
Compliance ensures that an organization follows the specific laws, policies, and procedures associated with their industry or geographic location.
* **Scope of Impact:** Requirements can be local (state/city), national, or international.
* **Consequences of Non-Compliance:** Can result in massive financial fines, incarceration for leadership, or immediate termination of employment.

## 19. Data Localization
A legal requirement that data collected by a country must remain physically stored within that country’s borders.
* **Purpose:** To ensure that a nation's citizens' data is subject to that nation's specific privacy laws and cannot be easily accessed by foreign governments.
* **Movement Restrictions:** Regulations dictate not only where data is *stored* but also where it is allowed to *move* once it has been gathered.

## 20. GDPR (General Data Protection Regulation)
The most comprehensive data privacy law in the world, originating from the European Union (EU).
* **Target:** Protects the data and privacy of all individuals residing within the EU.
* **Protected Data Types:** Includes names, addresses, photos, email addresses, banking information, and even digital footprints like website visit history.
* **Key Provisions:**
  * **Data Residency:** Information on EU citizens must be stored within EU borders.
  * **Individual Control:** Users have the right to decide where their data goes.
  * **Right to be Forgotten:** Users can request that an organization completely remove their data from their systems.


## 21. PCI DSS (Payment Card Industry Data Security Standard)
Unlike GDPR, this is not a government law, but a set of private industry standards created by credit card companies (Visa, Mastercard, etc.).
* **The Penalty:** Organizations that fail PCI DSS audits can lose their ability to process credit card transactions entirely.
* **Six Core Focus Areas:**
  1. **Secure Networks:** Building and maintaining firewalls and secure systems to protect data in transit.
  2. **Cardholder Data Protection:** Encrypting and protecting sensitive private information.
  3. **Vulnerability Management:** Maintaining active programs to find and patch security flaws.
  4. **Access Control:** Implementing strong measures to ensure only authorized personnel can touch credit card data.
  5. **Monitoring & Testing:** Regularly auditing the network to ensure security policies are actually working.
  6. **Information Security Policy:** Maintaining a broad, company-wide policy that covers all data protection.


## 22. Network Segmentation Enforcement 
Segmentation is the process of splitting a network into smaller, isolated sections to improve performance and security.
* **The "Blast Radius" Concept:** If an attacker compromises a device in one segment, proper enforcement prevents them from "leaping" (lateral movement) into other sensitive segments like the database or management zones.

## 23. Operational Technology: SCADA & ICS
In many environments, the network doesn't just move data; it controls physical infrastructure (Power grids, water treatment, manufacturing lines).
* **ICS (Industrial Control Systems):** The overarching category of hardware and software used to control industrial processes.
* **SCADA (Supervisory Control and Data Acquisition):** A large-scale system used to monitor and control processes distributed across large geographic areas (e.g., a city's electrical grid).


## 24. Mobile Device Deployment Models
How an organization handles mobile devices (phones, tablets) dictates the level of control the security team has over the hardware.
* **BYOD (Bring Your Own Device):** Employees use their personal devices for work. 
  * *Pros:* Low cost for the company; high user comfort.
  * *Cons:* Major security risk. Hard to manage privacy vs. security; difficult to ensure the device isn't "jailbroken" or infected with malware.
* **CYOD (Choose Your Own Device):** The company provides a list of approved devices, and the employee chooses one. The company still manages the security, but the user gets a say in the hardware.
* **COPE (Corporate Owned, Personally Enabled):** The company buys and fully manages the device, but allows the employee to use it for personal tasks (e.g., social media, personal email).
* **COBO (Corporate Owned, Business Only):** The device is strictly for work. Personal use is forbidden. Most secure, but least convenient for the user.



## Additional information (not on prof Messer)

## 25. VLANs and Subnetting (L2/L3 Segmentation)
* **VLANs (Layer 2):** Use switch configurations to logically separate devices regardless of their physical location.
* **Subnetting (Layer 3):** Use IP address boundaries to separate departments (e.g., HR is on 10.1.1.0/24, Finance is on 10.1.2.0/24).
* **Enforcement:** Separation alone isn't enough; you must use a **Firewall** or **Access Control List (ACL)** to dictate exactly which traffic is allowed to cross between these subnets.

## 26. Network Access Control (NAC / 802.1X)
NAC is the "Security Guard" at the door. It performs an assessment of a device *before* allowing it to connect to the network.
* **Posturing / Health Check:** The NAC checks the device for:
  * Up-to-date Antivirus signatures.
  * Active Firewall status.
  * Mandatory OS Security Patches.
* **Persistent vs. Dissolvable Agents:**
  * *Persistent Agent:* Software permanently installed on the device that continuously monitors health.
  * *Dissolvable Agent:* A temporary program that runs once during login to verify health and then disappears.
* **Quarantine Network:** If a device fails the health check (e.g., its antivirus is disabled), the NAC automatically places the device in a restricted "Quarantine VLAN." This allows the user to download updates/patches but prevents them from accessing the rest of the corporate network.


## 27. Jump Servers (Bastion Hosts)
A highly secured "middleman" server used to manage devices in a high-security zone (like a DMZ or a Database tier).
* **The Workflow:** An admin cannot SSH directly into a database server from their desktop. They must first SSH into the **Jump Server**, authenticate there, and then use the Jump Server to access the final destination.
* **Security Benefit:** Provides a single, hardened point to monitor, log, and audit all administrative access to critical infrastructure.

## 28. Honeypots & Honeynets (Enforcement Context)
* While used for deception, these are also enforcement tools. By placing a **Honeypot** in a segment, you can detect unauthorized lateral movement immediately. No legitimate user should ever be attempting to connect to a Honeypot; any traffic hitting it is an automatic "Red Alert" for the SOC.

## 29. Zero Trust Architecture (ZTA)
A modern security framework that removes the concept of a "trusted internal network."
* **The Mantra:** "Never Trust, Always Verify."
* **Micro-segmentation:** Breaking the network down to the individual device or even the individual application level. Every single communication request—even between two servers in the same rack—must be authenticated and authorized.

