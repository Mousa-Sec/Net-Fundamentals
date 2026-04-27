# Day 7: Wireless Infrastructure & Security (N10-009)
*Comprehensive Master Notes: Integrating 802.11 Standards, Interoperability, and Security*

## 1. The 802.11 Wireless Standards
* **802.11a:** 5 GHz frequency. Up to 54 Mbps. (Legacy).
* **802.11b:** 2.4 GHz frequency. Up to 11 Mbps. (Legacy).
* **802.11g:** 2.4 GHz frequency. Up to 54 Mbps. (Backwards compatible with 802.11b).
* **802.11n (Wi-Fi 4):** 2.4 GHz and 5 GHz. Introduced **MIMO** (Multiple-Input Multiple-Output) using multiple antennas. Up to 600 Mbps.
* **802.11ac (Wi-Fi 5):** 5 GHz exclusively. Introduced **MU-MIMO** (Multi-User MIMO), allowing the access point to transmit to multiple clients simultaneously. Up to ~7 Gbps.
* **802.11ax (Wi-Fi 6):** 2.4 GHz and 5 GHz. Introduced **OFDMA** (Orthogonal Frequency-Division Multiple Access), drastically improving efficiency in highly congested environments. Up to ~9.6 Gbps.

## 2. Wireless Architectures & Identifiers
* **IBSS (Independent Basic Service Set):** Also known as an *Ad Hoc* network. Two devices communicating directly with each other without a centralized access point (e.g., initially configuring IoT devices).
* **Identifiers:**
  * **SSID (Service Set Identifier):** The human-readable name of the wireless network (e.g., "Corp_WiFi").
  * **BSSID (Basic Service Set Identifier):** The physical MAC address of the specific access point providing the signal. 
  * **ESSID (Extended Service Set Identifier):** Used when multiple APs share the exact same SSID, allowing users to roam seamlessly across a large building without dropping their connection.
* **Access Point Management:**
  * *Autonomous APs:* Standalone devices that have all routing and configuration intelligence built-in (like a standard home router).
  * *Lightweight APs:* Devices that require a centralized controller to function.
  * *WLC (Wireless LAN Controller) & CAPWAP:* The protocol (CAPWAP) and the central dashboard (WLC) used to configure, monitor, and manage dozens of lightweight APs from a "single pane of glass."

## 3. Frequencies, Channels, & Interoperability
* **Frequencies & Bandwidths:** 802.11 networks commonly use **2.4 GHz, 5 GHz, and 6 GHz** bands. Common bandwidths (the amount of frequency used by a channel) are **20 MHz, 40 MHz, 80 MHz, and 160 MHz**. 
* **2.4 GHz Band:** Travels further and penetrates walls easily, but is highly susceptible to interference. 
  * *The Rule of 3:* To avoid overlapping interference, you must strictly use **Channels 1, 6, or 11**.
* **5 GHz Band:** Extremely fast and much less crowded, but has higher physical attenuation (struggles to penetrate walls). Dozens of non-overlapping channels are available.
* **Band Steering:** An AP feature that intercepts connection requests and "steers" capable dual-band clients to the faster 5 GHz frequency to avoid 2.4 GHz congestion.
* **802.11h (DFS & TPC):**
  * *DFS (Dynamic Frequency Selection):* The AP automatically scans for existing frequencies and changes channels to avoid interfering with priority systems like weather or military radar.
  * *TPC (Transmit Power Control):* The AP dictates how loud the client devices are allowed to broadcast, reducing overall network interference and saving device battery.

## 4. Wireless Encryption & Cipher Modes
* **WEP (Wired Equivalent Privacy):** The original encryption. Highly vulnerable to cryptographic breaks and quickly replaced.
* **WPA (Wi-Fi Protected Access):** A temporary stopgap replacement designed to run on legacy WEP hardware.
* **WPA2 (Wi-Fi Protected Access 2):** Introduced in 2004. Uses the **CCMP** block cipher mode.
  * *Encryption:* Uses **AES** for data confidentiality.
  * *Integrity:* Uses **CBC-MAC** for the message integrity check.
  * *Vulnerability:* Susceptible to offline dictionary/brute-force attacks if the 4-way handshake is captured.
* **WPA3 (Wi-Fi Protected Access 3):** Introduced in 2018. Uses the more advanced **GCMP** block cipher mode.
  * *Encryption:* Continues to use **AES** for data confidentiality.
  * *Integrity:* Uses **GMAC** (Galois Message Authentication Code).
  * *Handshake:* Replaces PSK with **SAE** (Simultaneous Authentication of Equals), entirely mitigating offline dictionary attacks and forcing Perfect Forward Secrecy.
* **Authentication Modes:**
  * *Personal (PSK):* Pre-Shared Key. Everyone types in the exact same password.
  * *Enterprise (802.1X):* Requires a centralized authentication server (like RADIUS). Users log in with individual corporate credentials.
* **Captive Portal:** A web page intercept that requires users to agree to terms, pay, or authenticate before accessing the internet.

## 5. Antennas
* **Omnidirectional Antennas:** Radiate signal outward in a 360-degree circle. Used for standard office/home APs (e.g., standard dipole).
* **Directional Antennas:** Focus all radio energy in a single, narrow beam. Used for point-to-point bridging between two buildings. Examples include **Yagi** and **Parabolic**.
