# Day 1: The OSI & TCP/IP Models (N10-009 Objective 1.1)

## The OSI Reference Model (The 7 Layers)
The Open Systems Interconnection (OSI) model is the universal framework used by engineers to describe how data moves from an application down to the physical wire.

1. **Layer 7: Application** - The interface where user applications access network services (e.g., HTTP, HTTPS, SSH, DNS).
2. **Layer 6: Presentation** - Formats, encrypts, and compresses data (e.g., SSL/TLS, JPEG, ASCII).
3. **Layer 5: Session** - Establishes, maintains, and terminates communication sessions between devices (e.g., RPC, NetBIOS).
4. **Layer 4: Transport** - Manages the delivery of data, dividing it into **Segments**. Handles multiplexing using ports.
   * **TCP (Transmission Control Protocol):** Connection-oriented, guarantees delivery, adds overhead.
   * **UDP (User Datagram Protocol):** Connectionless, fast, no guaranteed delivery.
5. **Layer 3: Network** - Handles logical addressing (IP addresses) and routing data across different networks into **Packets**. 
6. **Layer 2: Data Link** - Handles physical addressing (MAC addresses) and switches data within a local network into **Frames**.
7. **Layer 1: Physical** - The actual cables, transceivers, and radio frequencies moving raw **Bits**.

## The TCP/IP Model (The Practical Standard)
While the OSI model is theoretical, the TCP/IP model is what modern networks actually use. CompTIA maps the two together:

* **Application (Layer 4)** -> Maps to OSI Layers 5, 6, and 7.
* **Transport (Layer 3)** -> Maps to OSI Layer 4 (TCP/UDP).
* **Internet (Layer 2)** -> Maps to OSI Layer 3 (IPv4/IPv6).
* **Network Access / Link (Layer 1)** -> Maps to OSI Layers 1 and 2.

## Protocol Data Units (PDUs) - "Speaking like an Engineer"
When discussing data on the network, the terminology changes based on the layer. 
* Layers 5-7: **Data** (Payload)
* Layer 4: **Segment** (TCP) or **Datagram** (UDP)
* Layer 3: **Packet** (Contains IP addresses)
* Layer 2: **Frame** (Contains MAC addresses)
* Layer 1: **Bits** (Electrical signals or light pulses)

## The Purple Team Perspective
An attacker doesn't just memorize the model; they learn where the vulnerabilities live.
* **Layer 2:** ARP cache poisoning, MAC flooding.
* **Layer 3/4:** IP spoofing, bypassing stateless firewalls via fragmented packets, SYN floods.
* **Layer 7:** SQL injection, Cross-Site Scripting (XSS), DNS cache poisoning.

## Hardware & Protocol Mapping (Exam Prep)
When determining what layer a device operates on, look at the highest layer of data it is engineered to understand.

* **Layer 1 (Physical):** `Cables`, `Hubs`, `Repeaters`. (Transmits raw electrical/radio signals; no address understanding).
* **Layer 2 (Data Link):** `Switches`, `Bridges`, `Network Interface Cards (NICs)`. (Reads MAC addresses to route within a LAN).
* **Layer 3 (Network):** `Routers`, `Multilayer Switches`. (Reads IP addresses to route across the WAN/Internet).

* **Layer 4 (Transport Protocols):** `TCP`, `UDP`.
* **Layer 7 (Application Protocols):** `HTTP/HTTPS`, `FTP`, `SMTP/POP3/IMAP`, `DNS`.

## 3. Introduction to IP & Transport Protocols (Objective 1.4 - Transcript Verified)
* **The Encapsulation Process:** Data is packed sequentially to cross the network. 
  * *Analogy:* The Ethernet Frame is the road, the IP Header is the moving truck, the TCP/UDP Header is the moving box, and the Application Data is the physical items inside the box.
* **Transmission Control Protocol (TCP):**
  * *Connection-Oriented:* Sets up a formal session before sending data.
  * *Reliable Delivery:* Uses acknowledgements to guarantee the data arrived.
  * *Error Recovery & Flow Control:* Can request dropped packets to be resent and tell the sender to slow down if overloaded.
* **User Datagram Protocol (UDP):**
  * *Connectionless:* No formal session setup.
  * *Unreliable Delivery:* No acknowledgements. "Fire and forget."
  * *No Error Recovery / Flow Control:* Cannot resend dropped packets. Prioritizes speed over reliability.
* **IPv4 Sockets & Port Numbers:**
  * *Socket Definition:* A combination of an IP Address + Protocol (TCP/UDP) + Port Number.
  * *Total Ports Available:* 0 through 65,535.
  * *Non-Ephemeral Ports (0 - 1,023):* Permanent, well-known ports typically used by Servers (e.g., Web Server on Port 80 or 443).
  * *Ephemeral Ports (1,024 - 65,535):* Temporary ports randomly assigned to Clients for a single session.
  * *Protocol Isolation:* TCP Port 80 is a completely distinct destination from UDP Port 80.
