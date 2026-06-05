# CompTIA Network+ N10-009 — Objective 3.4: An Overview of DNS

### 1. The Core Purpose of DNS
* **Definition:** A distributed hierarchical name-to-address directory service designed to map alphanumeric host strings into machine-routable IP addresses.
* **In-Bracket Definition:** `(A network service that acts like a directory to map friendly web addresses into the numeric IP addresses computers need to talk to each other)`
* **Operational Dependencies:** Essential for web browsing, Active Directory structural lookups, and backend enterprise application routing.

### 2. The Hierarchical DNS Tree Structure
* **The Root Zone ( . ):** The peak of the tree, consisting of 13 global root server clusters representing over 1,000 physical or virtual server nodes.
* **Top-Level Domains (TLDs):**
  * **Generic TLDs (gTLDs):** Global designations including `.com`, `.org`, and `.net`.
  * **Country Code TLDs (ccTLDs):** Regional boundaries such as `.us`, `.ca`, and `.uk`.
* **Fully Qualified Domain Name (FQDN):** An absolute host address detailing all domain tiers down to the host machine (e.g., `www.professormesser.com`).

### 3. Redundancy & Server Roles (Primary vs. Secondary)
* **Primary DNS Server:** The master server hosting authoritative configuration records.
  * **In-Bracket Definition:** `(The master server where administrators manually input, modify, and manage all active DNS record configurations)`
* **Secondary DNS Server:** A redundant backup server holding a **read-only copy** of zone configurations synchronized from the primary node via an automated **Zone Transfer**.
  * **In-Bracket Definition:** `(A backup server that pulls a read-only copy of setting files from the master server to handle user requests)`

### 4. Local Name Resolution (The Hosts File)
* **Definition:** A localized name resolution override mechanism embedded directly inside the host operating system.
* **In-Bracket Definition:** `(A local text file on a computer that overrides external DNS servers by mapping specific IP addresses to web domains manually)`
* **Windows File Path:** `C:\Windows\System32\drivers\etc\hosts`. Requires administrative write privileges to bypass read-only locks for testing or overriding faulty server information.

### 5. Resolution Direction (Forward vs. Reverse Lookups)
* **Forward Lookups:** Standard query path where a client submits an alphanumeric **name** and receives a **numeric IP address**.
* **Reverse Lookups:** The inverse path where a client queries using an **IP address** and the server returns the associated **host name**.

### 6. Authoritative vs. Non-Authoritative Responses
* **Authoritative Server:** The primary name server hosting the definitive source-of-truth master zone file.
* **Non-Authoritative Server:** A secondary or public proxy resolver delivering an answer pulled directly from its temporary local memory cache.
* **Time to Live (TTL):** A parameter inside the authoritative zone record specifying exactly how many seconds a non-authoritative server is allowed to cache data before it must be deleted.

### 7. Recursive DNS Query Mechanics
* **Definition:** An all-or-nothing query pattern where a client workstation (the **resolver**) passes name resolution tracking duties to its local DNS server.
* **The Search Hierarchy:** If an address is missing from the local cache, the local server queries a **Root Server**, receives a referral to a **TLD Server**, queries the TLD to get a referral to the **Authoritative Server**, and finally pulls the true IP to cache and return to the client.

### 8. DNS Security Frameworks (DNSSEC, DoT, DoH)
* **DNSSEC:** Enhances data integrity by attaching cryptographic digital signatures to DNS responses.
  * **In-Bracket Definition:** `(A security framework that adds digital signatures to DNS responses to prove the data came from the real server and wasn't altered by an attacker)`
* **DoT (DNS over TLS):** Encrypts name resolution traffic using a dedicated cryptographic tunnel over **TCP Port 853**.
  * **In-Bracket Definition:** `(A security protocol that packs cleartext DNS queries inside an encrypted TLS tunnel over a custom network port to prevent snooping)`
* **DoH (DNS over HTTPS):** Encrypts lookups by hiding them inside standard HTTPS web packets over **TCP Port 443**, blending DNS lookups into regular web traffic.
  * **In-Bracket Definition:** `(A privacy protocol that hides encrypted DNS lookups inside standard web traffic so they cannot be blocked or monitored)`
 
# CompTIA Network+ N10-009 — Objective 3.4: DNS Records

### 1. Introduction to Resource Records
* **Resource Records:** The specific configuration entries within a DNS zone file that define data properties, manage addressing mappings, and authorize external application traffic.

### 2. Start of Authority (SOA) Record
* **Definition:** The foundational record entry heading a DNS zone profile.
* **In-Bracket Definition:** `(A master configuration record that provides structural overview information about a DNS zone, including the administrative domain and sync timers)`
* **Parameters:** Controls zone serial numbers (incrementing for secondary sync tracking), retry windows, and master file lifespans.

### 3. Address Forwarding Records (A vs. AAAA)
* **A Records:** Maps an alphanumeric host name directly to an **IPv4** address.
* **AAAA Records (Quad-A):** Maps an alphanumeric host name directly to a 128-bit **IPv6** hex address.

### 4. Canonical Name (CNAME) Records
* **Definition:** Tracks aliases for a domain, redirecting alternate lookup names back to a single, true host string.
* **In-Bracket Definition:** `(An alias record that redirects multiple different host names to a single true, canonical domain name)`
* **Transaction Loop:** Querying an alias returns the true domain name. The client must then issue a second recursive query to find the actual A/AAAA record of that target to resolve the path.

### 5. Mail Exchanger (MX) Records
* **Definition:** Identifies the designated mail servers authorized to handle inbound mail routing for the organization.
* **In-Bracket Definition:** `(A dedicated routing record that tells external email servers which specific mail servers accept messages for your domain)`

### 6. Text (TXT) Records
* **Definition:** Multi-functional, string-based fields utilized primarily to hold public text keys and implement security controls.
* **In-Bracket Definition:** `(A generic text slot inside DNS used to store security records, verification strings, and public cryptographic keys)`
* **Security Controls:**
  * **SPF:** Restricts email spoofing by listing the specific server IPs authorized to send mail on behalf of the domain.
  * **DKIM:** Protects message authenticity by storing the public cryptographic key needed to verify digital signatures on outbound emails.

### 7. Name Server (NS) Records
* **Definition:** Declares the authoritative name servers that hold the official zone data files for the domain.
* **In-Bracket Definition:** `(A record that identifies the official name servers that hold the true master files and settings for a domain)`

### 8. Pointer (PTR) Records
* **Definition:** Used during reverse DNS lookups to map an IP address back to its FQDN host string.
* **In-Bracket Definition:** `(A reverse-lookup record that takes a numeric IP address and resolves it back to its matching text domain name)`
* **Format:** Records target IPs in reverse notation order inside the internal file structure to cleanly process address-to-name translations.







