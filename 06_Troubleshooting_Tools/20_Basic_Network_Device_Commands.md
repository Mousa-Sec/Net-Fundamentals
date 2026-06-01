
# CompTIA Network+ N10-009 — Objective 5.5: Basic Network Device Commands

### 1. MAC Address Table (`show mac address-table`)
* **Purpose:** Displays the switch’s internal database mapping hardware MAC addresses to specific physical switch ports.
* **In-Bracket Definition:** `(Lists which device MAC address is plugged into which switch port)`
* **Usage:** Used to verify if a switch has successfully learned a connected device's identity and to check if a switch is approaching its stored entry threshold.

### 2. Routing Table (`show route` / `show ip route`)
* **Purpose:** Displays the path options and metrics stored inside a router to determine where inbound network traffic should be forwarded.
* **In-Bracket Definition:** `(Shows the map of network paths a router uses to send packets to external networks)`
* **Key Indicators:**
  * **C:** Directly Connected route (the target network is physically attached to an active interface on the router).
  * **R:** Learned dynamically via the Routing Information Protocol (RIP).
  * Entries display the destination network, the next-hop gateway IP address, and the exact outbound interface utilized.

### 3. Interface Status and Performance (`show interface`)
* **Purpose:** Provides deep hardware configuration status, physical layer line health, and performance statistics for a selected port.
* **In-Bracket Definition:** `(Checks if a network port is physically up/down and shows traffic/error statistics)`
* **Key Components:**
  * **Line & Protocol Status:** Shows if the interface is physically up (receiving signal) and if the data link layer protocol is active.
  * **Hardware Specs:** Shows port speed (e.g., FastEthernet for 100 Mbps), slot/port labels, duplex state, and media type (e.g., RJ45).
  * **Error Counters:** Reports CRC errors, dropped frames, and total broadcast packet metrics to flag performance drops.

### 4. Device Configuration (`show config` / `show running-config`)
* **Purpose:** Outputs a full, readable text representation of the network device's active configuration settings.
* **In-Bracket Definition:** `(Displays the actual text-based settings file currently running in the device memory)`
* **Key Components:** Displays file size, software versions, system timestamps, assigned port IP addresses, subnet masks, and interface variables.

### 5. ARP Cache Table (`show arp`)
* **Purpose:** Displays the Address Resolution Protocol (ARP) table on the switch or router, mapping Layer 3 IP addresses directly to their corresponding Layer 2 physical MAC addresses.
* **In-Bracket Definition:** `(Maps an IP address to a hardware MAC address inside a network device)`
* **Key Components:** Lists the protocol, IP address, corresponding hardware MAC address, and the associated physical interface.

### 6. Virtual Local Area Networks (`show vlan`)
* **Purpose:** Details the current internal VLAN segmentation profile active on a switch.
* **In-Bracket Definition:** `(Shows what VLANs exist and which switch ports belong to each one)`
* **Key Components:** Identifies the default VLAN structure and cross-references active interfaces with their assigned isolation broadcast domains.

### 7. Power over Ethernet Status (`show power` / `show power inline`)
* **Purpose:** Monitors the available power pool, distribution states, and consumption metrics for Power over Ethernet (PoE) enabled switches.
* **In-Bracket Definition:** `(Monitors how many watts of electricity the switch has available and how much power devices are pulling)`
* **Key Components:** Displays if PoE is running on an interface, records active device wattage draw, and computes remaining switch wattage capacity to prevent overloading.
