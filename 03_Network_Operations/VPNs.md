# CompTIA Network+ N10-009 — Objective 3.5: VPNs

### 1. Fundamental VPN Concepts & Mechanics
* **Core Definition:** A system that encapsulates and encrypts unencrypted data payloads, allowing them to transit safely over insecure public networks like the internet.
* **In-Bracket Definition:** `(A secure, encrypted tunnel that protects private network data as it travels across the public internet)`
* **VPN Concentrator:** A central connection gateway responsible for terminating tunnels and handling real-time encryption/decryption processing. Typically integrated into Next-Gen Firewalls, it can be deployed via standalone hardware appliances or host server software.

### 2. Client-to-Site vs. Site-to-Site Architectures
* **Client-to-Site (Remote Access):** Links an individual remote endpoint back into the central corporate facility. Can be initiated manually by the user or configured as an automated, background **Always-On** link that launches at machine boot.
  * **In-Bracket Definition:** `(A secure connection linking a single remote employee's laptop back to the main corporate network)`
* **Site-to-Site VPNs:** A permanent, always-on encrypted tunnel established between boundary firewalls to transparently bridge the networks of two separate, fixed office locations over the internet.
  * **In-Bracket Definition:** `(A permanent, always-on encrypted connection that links the networks of two separate office buildings together)`

### 3. Clientless VPNs
* **Definition:** A remote access solution that requires no dedicated software installation or device administrative rights.
* **In-Bracket Definition:** `(A secure VPN that runs entirely inside a standard web browser without needing any extra software installed)`
* **Technical Framework:** Runs completely inside an **HTML5-compliant** browser web window, leveraging native browser-level **Web Cryptography APIs** to initialize and manage the encrypted tunnel.

### 4. Full Tunneling vs. Split Tunneling
* **Full Tunneling:** Pushes 100% of all client-generated network traffic straight into the encrypted VPN tunnel. Public internet traffic must route through the corporate concentrator to be decrypted and forwarded out, ensuring maximum security auditing at the cost of corporate bandwidth bottlenecks.
  * **In-Bracket Definition:** `(A routing setting where all computer traffic, including corporate data and public web browsing, is forced through the secure corporate tunnel)`
* **Split Tunneling:** Logically separates traffic at the client workstation. Work-related data routes securely inside the encrypted VPN tunnel, while public, third-party web traffic bypasses the tunnel completely to route directly over the user's local internet connection.
  * **In-Bracket Definition:** `(A routing layout that sends work traffic through the secure corporate tunnel, while public web browsing goes directly out to the regular internet)`






