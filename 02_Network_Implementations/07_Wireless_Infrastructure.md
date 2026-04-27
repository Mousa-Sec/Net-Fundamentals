# Day 7: Wireless Technologies & Encryption (N10-009)
*Fact-Checked against Professor Messer N10-009 Objective 2.3*

## 1. Wireless Frequencies & Channels
* **Frequencies:** 802.11 networks commonly use **2.4 GHz, 5 GHz, and 6 GHz** bands.
* **Channels:** Instead of memorizing exact frequencies (e.g., 2.437 GHz), the IEEE groups these into easy-to-use channels (e.g., Channel 6 or Channel 44).
* **Bandwidths:** The amount of frequency used by a channel. Common sizes are **20 MHz, 40 MHz, 80 MHz, and 160 MHz**. 
* **2.4 GHz Spectrum:** Has only **three non-overlapping channels** when using standard 20 MHz bandwidths.
* **Band Steering:** A feature on access points that forces or "steers" a capable client device to the optimal frequency (often the 5 GHz band). Without band steering, a client will simply connect to whatever frequency has the strongest physical signal (usually the crowded 2.4 GHz band), which often results in worse throughput.

## 2. Wireless Interoperability (802.11h & ITU)
The ITU (International Telecommunications Union) provides worldwide guidelines to manage wireless technologies. The **802.11h** standard adds features so different networks can operate in the same geographic area without stepping on each other:
* **DFS (Dynamic Frequency Selection):** The access point automatically scans for existing frequencies in use and selects a channel that will not conflict with neighboring networks. If the environment changes, the AP automatically changes its channel and forces connected clients to move with it.
* **TPC (Transmit Power Control):** Traditionally, client devices (like phones) decided how much radio power to use. TPC puts power control in the hands of the access point. The AP can tell a client to lower its transmit power, reducing interference with third-party networks nearby.

## 3. Wireless Encryption & Cipher Modes
If you are sniffing traffic as a Purple Teamer, you need to know exactly how the packets are secured.
* **WEP (Wired Equivalent Privacy):** The original encryption. Highly vulnerable to cryptographic breaks and quickly replaced.
* **WPA (Wi-Fi Protected Access):** A temporary stopgap replacement designed to run on the exact same legacy hardware as WEP.
* **WPA2:** Introduced in 2004. Uses the **CCMP** block cipher mode (Counter Mode with Cipher Block Chaining Message Authentication Code Protocol). 
  * *Encryption:* Uses **AES** for data confidentiality.
  * *Integrity:* Uses **CBC-MAC** for the message integrity check.
* **WPA3:** Introduced in 2018. Uses the more advanced **GCMP** block cipher mode (Galois/Counter Mode Protocol).
  * *Encryption:* Continues to use **AES** for data confidentiality.
  * *Integrity:* Uses **GMAC** (Galois Message Authentication Code) to verify the data was not tampered with during transit.
