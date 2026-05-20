# CompTIA Network+ N10-009 — Objective 5.2
## Hardware Issues

Network hardware troubleshooting requires a deep understanding of Power over Ethernet (PoE) budget calculations, transceiver wavelength mapping, and optical power budget metrics to prevent transmission degradation and link failure.

---

### 1. Power over Ethernet (PoE) Architectures & Standards
PoE simplifies device deployment by delivering DC power alongside network payloads over a single twisted-pair copper cable.

* **Power Delivery Models:**
  * **Endspan:** Power is injected directly by an upstream Ethernet switch with integrated PoE circuitry.
  * **Midspan:** Power is injected via a standalone midspan device (PoE Injector) situated between a non-PoE switch and the target device.
* **The 3 Core PoE Standards:**
  * **PoE (IEEE 802.3af):** Delivers up to **15.4 Watts** of DC power output at the source with a maximum current of 350 milliamps. Designed for low-power endpoints like basic VoIP phones or entry-level wireless access points.
  * **PoE+ (IEEE 802.3at):** Delivers up to **25.5 Watts** of DC power output with a maximum current of 600 milliamps. Frequently used for higher-demand devices like pan-tilt-zoom (PTZ) cameras or complex multi-radio access points.
  * **PoE++ (IEEE 802.3bt):** Features two distinct operational bands:
    * **Type 3:** Delivers up to **51 Watts** of DC power output at a 600 milliamp maximum current baseline.
    * **Type 4:** Delivers up to **71.3 Watts** of DC power output at a 960 milliamp maximum current limit. Supports high-power endpoints such as modern enterprise laptops or intensive PTZ cameras with built-in heating elements. This standard introduces support for advanced multi-gigabit speeds including 2.5 Gbps, 5 Gbps, and 10 Gbps networks.

### 2. PoE Compatibility & Capacity Planning
Deploying high-density PoE infrastructures requires strict validation of switch capabilities and overall power footprints.
* **Backward Compatibility Constraints:** Upstream switches cannot supply adequate power if they use an inferior standard compared to the endpoint. A PoE+ switch cannot power a device requiring PoE++ operation.
* **Granular Port Allocation:** Commercial managed switches often feature mixed-mode port arrays. Certain ports may offer no power, some may support PoE+, while others offer dedicated PoE++. Engineers must match endpoints to specific port tiers.
* **Power Budget Calculation:** Every PoE switch has a maximum aggregate power budget capacity (e.g., 200W, 720W). To prevent overloading, administrators must calculate the maximum cumulative power draw of all connected devices and verify the sum falls below the switch's total rated threshold.

### 3. Transceiver Selection & Wavelength Mismatch
Modular optical transceivers (such as SFPs) expand infrastructure flexibility but introduce critical hardware alignment parameters.
* **Wavelength Specifications:** Transceivers project optical signals across explicit electromagnetic wave structures, measured in nanometers (e.g., **850 nm** for short-range multimode, **1310 nm** for long-range single-mode).
* **Mismatch Consequences:** The designated wavelength must match uniformly across the entire end-to-end fiber link (transceiver A, fiber path, and transceiver B). Intermixing conflicting wavelengths creates immediate signal loss, triggering high error counters, physical frame degradation, or a dead link state.
* **Physical Identification Challenge:** Optical transceivers often look identical at first glance. Wavelength designations are printed in small type on a side label that becomes unreadable once slid inside a switch cage. Verifying specifications requires consulting the active switch operating system via the CLI/GUI or pulling the unit entirely out of the port slot.

### 4. Optical Power Budgets & Receiver Sensitivity
Ensuring reliable fiber signal delivery across long physical runs requires tracking decibel-milliwatt metrics against structural hardware sensitivity thresholds.
* **Receiver Sensitivity:** The explicit technical value defining the minimum optical signal strength a transceiver's receiving diode requires to accurately process incoming data frames without errors.
* **The Power Budget Equation:**
  1. Determine the baseline optical power emitted by the transmitting transceiver, measured in **decibels relative to one milliwatt (dBm)**.
  2. Compute total path signal loss by multiplying the total fiber run distance against its rated attenuation factor.
  3. Add explicit attenuation penalties for every intermediate connector, fiber coupling, and splice along the run.
  4. Subtract total calculated loss from the original transmission power value to determine the expected **Received Power**.
* **Threshold Verification:** Received power operates on a negative scale (e.g., -10 dBm is a stronger signal than -20 dBm). If an SFP specifies a receiver sensitivity of **-17 dBm**, an engineering calculation yielding a received power of -14 dBm will function perfectly. If path attenuation reduces the signal down to -20 dBm, the transceiver cannot interpret the payload, leading to physical link drops.
