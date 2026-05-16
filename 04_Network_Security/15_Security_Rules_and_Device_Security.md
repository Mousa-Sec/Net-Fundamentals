
## 5. Port-Level Hardening & Layer 2 Security
Securing physical switch infrastructure prevents attackers from gaining unauthorized access simply by plugging a cable into an active wall jack.

### A. Attack Surface Management: Open Ports & Services
* **The Vector:** Every network service running on a device opens an active port number (ranging from `0` to `65,535`). Each open port represents a potential entry point for exploitation.
* **Discovery:** Attackers and defensive engineers use network reconnaissance tools like **Nmap** to scan systems and advertise open ports across the network.
* **Remediation:** If a service is non-essential, it must be completely disabled, which closes the associated network port and shrinks the overall attack surface.

### B. Default Credential Hardening
* **The Risk:** Most networking devices (routers, switches, firewalls) ship with factory-set default usernames and passwords (e.g., `admin/admin`). Databases like *routerpasswords.com* compile these standard combinations for thousands of devices.
* **The Threat:** Failure to alter these default settings during deployment allows any local or remote actor to easily gain administrative control over the entire network appliance.

### C. Switch Port Security Mechanics
Port Security is an automated Layer 2 defense feature built into switches to enforce hardware boundaries on physical ports.
* **Operation:** The switch restricts incoming frame transmission based on the **MAC (Media Access Control) Address** of the connected device. It prevents unauthorized hardware from hijacking an existing cable/wall jack.
* **Configuration Parameters:**
  * **MAC Address Limitations:** Administrators define the maximum number of allowed MAC addresses per physical interface (typically limited to exactly `1` for standard workstations).
  * **Static vs. Dynamic Binding:** The switch can be programmed to accept only a specifically listed MAC address or automatically learn the first MAC address that initiates traffic.
* **Violation Handling:** If an unapproved device connects and exceeds the permitted MAC address ceiling, Port Security triggers. The enterprise standard reaction is to immediately drop traffic, **administratively disable (shutdown)** the interface, and generate an alert (such as an SNMP trap or Syslog entry) to the security team.

### D. Interface Disabling vs. 802.1X (NAC)
* **Unused Port Shut Down:** A foundational physical best practice is to manually configure all unassigned network interfaces (e.g., wall drops in conference rooms or break rooms) to an **administrative down** state until explicitly requested.
* **802.1X Network Access Control (NAC):** To avoid the high administrative overhead of manually toggling ports, organizations implement **802.1X**. Devices connecting to a port are blocked from network communication and prompted for explicit credentials (username/password or certificate) via an authentication server before the switch interface opens up traffic.

### E. MAC Address Filtering & Obscurity
* **The Mechanism:** Enforcing a whitelist of allowed hardware MAC addresses on a network or wireless access point.
* **The Flaw:** MAC address filtering is classified purely as **Security through Obscurity**. 
* **Exploitation:** Because MAC addresses are transmitted in plaintext over the air or wire, an attacker can use a packet sniffer to capture authorized traffic, wait for that device to disconnect, and administratively **spoof** their own network interface card to match the authorized MAC address, bypassing the filter seamlessly.

---

## 6. Key Management Systems (KMS)
As enterprise environments scale, managing thousands of cryptographic secrets, certificates, and authentication credentials becomes impossible to handle manually.


### A. Centralized Management Consolidation
A Key Management System (KMS) provides a singular control plane to create, distribute, store, renew, and revoke cryptographic keys across an entire infrastructure.

### B. Managed Cryptographic Asset Types
* **SSL/TLS Certificates:** Tracks public-facing certificates used by web servers. The console tracks exact expiration timelines, preventing sudden operational downtime caused by expired HTTPS bindings.
* **SSH Keys:** Manages the public/private key pairs used by administrators to securely authenticate to Linux servers and core networking devices.
* **Cloud Provider & Service Keys:** Tracks API tokens and access keys utilized to authenticate microservices across multi-cloud deployments.

### C. Lifecycle Auditing & Non-Repudiation
* **Granular Reporting:** A central KMS logs exactly *when* a key is used, *who* requested it, and *which* target system was accessed. 
* **Revocation Control:** If an administrator leaves the organization or a private key file is compromised, the key can be instantly revoked globally from the central dashboard, severing access to all linked internal infrastructure.
