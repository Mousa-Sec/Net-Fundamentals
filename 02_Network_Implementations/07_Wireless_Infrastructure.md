# Day 7: Wireless Infrastructure & Security (N10-009)
*Fact-Checked against Professor Messer N10-009 Objective 2.4*

## 1. The 802.11 Wireless Standards
You must memorize the frequencies and specific features of these IEEE standards:
* **802.11a:** 5 GHz frequency. Up to 54 Mbps. (Legacy).
* **802.11b:** 2.4 GHz frequency. Up to 11 Mbps. (Legacy).
* **802.11g:** 2.4 GHz frequency. Up to 54 Mbps. (Backwards compatible with 802.11b).
* **802.11n (Wi-Fi 4):** 2.4 GHz and 5 GHz. Introduced **MIMO** (Multiple-Input Multiple-Output) using multiple antennas. Up to 600 Mbps.
* **802.11ac (Wi-Fi 5):** 5 GHz exclusively. Introduced **MU-MIMO** (Multi-User MIMO), allowing the access point to transmit to multiple clients simultaneously. Up to ~7 Gbps.
* **802.11ax (Wi-Fi 6):** 2.4 GHz and 5 GHz. Introduced **OFDMA** (Orthogonal Frequency-Division Multiple Access), drastically improving efficiency in highly congested environments. Up to ~9.6 Gbps.

## 2. Frequencies & Channel Selection
* **2.4 GHz Band:** Travels further and penetrates walls easily, but is highly susceptible to interference from microwaves, Bluetooth, and cordless phones. 
  * *The Rule of 3:* To avoid overlapping interference, you must strictly use **Channels 1, 6, or 11**.
* **5 GHz Band:** Extremely fast and much less crowded, but has higher physical attenuation (it struggles to penetrate physical walls and travels shorter distances). Dozens of non-overlapping channels are available.

## 3. Wireless Encryption Protocols
* **WPA2 (Wi-Fi Protected Access 2):** Uses **AES** (Advanced Encryption Standard) for encryption and **CCMP** for data integrity. Vulnerable to offline dictionary/brute-force attacks if the 4-way handshake is captured.
* **WPA3 (Wi-Fi Protected Access 3):** The modern standard. Replaces PSK with **SAE** (Simultaneous Authentication of Equals), entirely mitigating offline dictionary attacks. Also forces Perfect Forward Secrecy.
* **Authentication Modes:**
  * *Personal (PSK):* Uses a Pre-Shared Key. Everyone types in the exact same Wi-Fi password.
  * *Enterprise (802.1X):* Requires a centralized authentication server (like RADIUS). Users log in to the Wi-Fi using their own unique corporate username and password.

## 4. Antennas
* **Omnidirectional Antennas:** Radiate signal outward in a 360-degree circle. Used for standard office/home APs (e.g., standard dipole).
* **Directional Antennas:** Focus all radio energy in a single, narrow beam. Used for point-to-point bridging between two buildings. Examples include **Yagi** and **Parabolic**.
