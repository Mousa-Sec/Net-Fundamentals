# CompTIA Network+ N10-009 — Objective 5.4
## Wireless Issues

Wireless engineering requires managing finite, shared airwave frequencies. Troubleshooting performance degradation in wireless networks involves correcting frequency overlaps, mitigating physical attenuation, conducting site surveys, and preventing data link management layer exploits.

---

### 1. Channel Planning and Spectrum Management
Wireless access points (APs) and client radios must share the same localized airspace. If multiple transmitters use conflicting frequencies, data frame collisions degrade network performance.

* **Automated vs. Manual Optimization:** Modern enterprise access points run automated spectrum management protocols to sweep local bands, identify clear frequencies, and dynamically move to an optimal channel to bypass interference. In fixed environments, engineers handle this manually.
* **The 2.4 GHz Spectrum Constraints:**
  * **Channel Capacity:** The 2.4 GHz band provides an extremely tight frequency window with only three non-overlapping channels in North America: **Channel 1, Channel 6, and Channel 11**.
  * **The Overlap Hazard:** A frequent implementation mistake occurs when an administrator configures an AP on an improper channel like Channel 8. As a result, its signal footprint bleeds directly into adjacent communications on both Channel 6 and Channel 11, corrupting frames for all devices on those channels.
* **Higher Frequency Expansion (5 GHz and 6 GHz):** Upgrading infrastructure to modern standards bypasses 2.4 GHz crowding by opening massive frequency windows:
  * **5 GHz Band:** Delivers significantly more independent, non-overlapping channels to minimize adjacent-cell interference.
  * **6 GHz Band (Wi-Fi 6E / Wi-Fi 7):** Introduces a massive, clear frequency spectrum that reduces channel reuse conflicts and eliminates legacy interference bottlenecks.

### 2. Legacy Support Bottlenecks
Access points contain settings to maintain backward compatibility with older wireless hardware standards.

* **The Performance Tax:** Enabling legacy fallback protections forces modern multi-antenna APs to run slower, less efficient processing loops to accommodate aging devices.
* **Remediation:** If all production endpoints support modern high-speed standards, administrators disable legacy mode support in the AP configuration. This optimizes radio processing and boosts overall wireless cell throughput.

### 3. Signal Attenuation and Antenna Optimization
As wireless radio waves travel away from an access point's antenna, the signal naturally loses strength.

* **Attenuation Analysis:** Administrators use wireless analyzer tools to measure signal drop-offs across a physical site, looking for weak zones caused by distance or structural building barriers.
* **Remediation Techniques:**
  * **Power Transmit Control:** If supported by the hardware, engineers increase the radio output power on the AP to extend its coverage footprint. Conversely, if high-power APs bleed into each other and cause co-channel interference, reducing the transmit power fixes the issue.
  * **Antenna Gain Enhancements:** Adding a high-gain external antenna amplifies signal transmission and reception over longer distances without pulling additional raw power from the internal radio engine.
  * **Coaxial Line Loss:** Connecting external antennas via coaxial cables introduces signal loss inside the wire itself, which worsens at higher frequencies. Engineers must keep coaxial cable runs as short as possible and inspect the shielding for kinks or leaks that waste signal power.

### 4. Wireless Site Surveys and Heat Mapping
Maintaining clear wireless coverage across changing enterprise environments requires regular physical audits.

* **External Environment Scanning:** Site surveys identify rogue or neighboring access points that compete for the same frequencies, helping engineers map out alternative channel layouts.
* **Heat Map Visualization:** Engineers perform a walkthrough using a Wi-Fi analyzer tool to map out real-time coverage boundaries. 
  * Bright, warm colors indicate excellent signal coverage and high signal-to-noise ratios (SNR).
  * Cool colors indicate dead zones or heavy attenuation, marking exactly where additional access points are needed.
* **Periodic Re-Surveys:** Because neighboring businesses constantly change, modify, or upgrade their wireless hardware throughout the year, engineers perform regular follow-up site surveys to adapt their channel layouts to new interference.

### 5. Deauthentication/Disassociation DoS Attacks
Legacy wireless standards carry a severe security flaw within their layer 2 management frame structures.

* **The Exploitation Mechanism:** Under older 802.11 standards, management frames (like disassociation or deauthentication requests) are transmitted completely unencrypted and unauthenticated. An attacker can sniff the network, spoof a legitimate AP's MAC address, and send forged disassociation frames directly to a client device.
* **Symptoms:** The target device is abruptly and repeatedly dropped from the wireless network. It stalls in a constant loop of disconnecting, re-authenticating, and disconnecting again, resulting in a total Denial of Service (DoS).
* **Detection & Mitigation:**
  * **Packet Capture Analysis:** Engineers run wireless packet captures to locate the rogue disassociation frames and track down the physical location of the attacking transmitter.
  * **802.11w Protection (Remediation):** Upgrading to modern security standards implements **Protected Management Frames (PMF)** under the 802.11w amendment. This encrypts and signs management traffic, neutralizing spoofed deauthentication attacks completely.

### 6. ESSID Deployment and Roaming Configurations
An Extended Service Set Identifier (ESSID) links multiple physical access points together under a single, shared network name.

* **Seamless Roaming:** A shared ESSID allows mobile clients to move between different rooms or buildings, seamlessly dropping their connection from a fading AP and connecting to a closer, stronger AP without user intervention.
* **Configuration Drift Faults:** For roaming to work perfectly, every participating access point must share identical backend configurations. If a single AP has a slight typo in its security settings, encryption type, or passphrase, a roaming client will be dropped completely when moving into its area and forced to prompt the user for credentials again.
