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







