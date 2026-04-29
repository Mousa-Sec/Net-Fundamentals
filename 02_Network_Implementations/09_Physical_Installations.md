# Day 9: Physical Installations & Environment (N10-009)
*Fact-Checked against Professor Messer N10-009 Objective 2.4*

## 1. Distribution Frames (MDF vs. IDF)
* **MDF (Main Distribution Frame):** The primary, central point of the network. Usually the main data center where the external WAN (ISP) connection arrives.
* **IDF (Intermediate Distribution Frame):** Extension of the MDF. These are smaller wiring closets located on different floors or in different buildings, connecting local users back to the MDF.

## 2. Rack Infrastructure
* **Standard Width:** Most racks are **19 inches wide**.
* **Rack Units (U):** Equipment height is measured in "U." One U is exactly **1.75 inches** (4.4 cm).
* **Physical Security:** Racks can be open or **Locking Cabinets** to prevent unauthorized physical access to hardware.

## 3. Data Center Cooling (HVAC)
Cooling is critical to prevent hardware failure. Data centers use specifically engineered **HVAC** (Heating, Ventilating, and Air Conditioning) systems.
* **Hot Aisle / Cold Aisle:** Servers pull cold air into the front and vent hot air out the back. Racks are arranged so the front of the servers face each other (creating a **Cold Aisle**) and the backs face each other (creating a **Hot Aisle**).
* **Containment:** Plastic barriers or raised floors are used to keep the cold air concentrated on the equipment.

## 4. Cable Termination
* **Patch Panels:** Used to terminate horizontal cabling from wall ports. This allows for "Moves, Adds, and Changes" without touching the permanent wires behind the walls.
* **110 Block:** A standard punch-down block used on the back of copper patch panels.
* **Fiber Distribution Panel:** Similar to patch panels but for fiber optic runs. 
  * *Critical Rule:* Must respect the **Bend Radius** to prevent internal glass breakage.
  * *Service Loop:* Extra coiled fiber left in the panel to provide flexibility for future changes.
