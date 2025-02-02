# 🛠️ **KICAD PCB SCHEMATIC AND LAYOUT**  

In this section, we will **transform the breadboard prototype into a PCB design** using **KiCad 8.0**, the latest **industrial-grade PCB design software**.  

🔗 **Download KiCad 8.0 (Latest Version):** [KiCad Official Website](https://www.kicad.org/download/windows/)

We will guide you step by step on:  
✔️ **Creating the schematic** in KiCad  
✔️ **Designing the PCB layout**  
✔️ **Generating drill files, BOM (Bill of Materials), and CPL files**  
✔️ **Ordering the PCB from JLCPCB**  

This tutorial will cover **everything** from **schematic design to final PCB ordering**! 🏭📦  

---
## 🎯 **Before Starting: Learning KiCad 8.0 for PCB Design**  

I am also a **beginner** in PCB design, but if you want to **learn how to use KiCad 8.0** for designing PCBs, I highly recommend watching this **YouTube tutorial**! 🎥💡  

📌 **This tutorial is very useful** and will help you understand the basics of:  
✔️ **Creating schematics** in KiCad  
✔️ **Designing PCB layouts**  
✔️ **Generating Gerber files** for manufacturing  

🔗 **Watch the tutorial here:** [https://www.youtube.com/watch?v=3E5REDAQk_A]  

---

📢 **Once you've learned the basics, continue with Step 1 to start designing the PCB for our ESP32 project! 🚀**  

---
# Step 1️⃣: Choosing Pin Holders and ESP32 Placement  

Since most of the components are connected using **jumper wires**, we can simplify the PCB design by using **pin headers** instead of direct soldered components.  

---

### 📌 **Important Guidelines for Pin Holders:**  
✔️ **Use 2.54mm pin holders** for all components.  
✔️ **ESP32 requires two 15-pin holders** (one for each side).  
✔️ The **ESP32 pin headers should be spaced exactly 25.4mm apart**.  
✔️ Ensure **all pins are correctly connected** for each component before finalizing.  

---

### 🖼️ **Schematic Overview**  
In the **center of the schematic**, we place the **ESP32 with the 2x15 pin holders**.  
Each **connected component (keypad, LCD, fingerprint sensor, RFID, buzzer, etc.)** should have its corresponding **pin headers assigned properly**.  

🔹 **Double-check the connections before proceeding to the PCB layout step!** 🚀  

 ## Step 1️⃣: Choosing Pin Holders and ESP32 Placement  

Since most of the components are connected using **jumper wires**, we can simplify the PCB design by using **pin headers** instead of direct soldered components.  

---

### 📌 **Important Guidelines for Pin Holders:**  
✔️ **Use 2.54mm pin holders** for all components.  
✔️ **ESP32 requires two 15-pin holders** (one for each side).  
✔️ The **ESP32 pin headers should be spaced exactly 25.4mm apart**.  
✔️ Ensure **all pins are correctly connected** for each component before finalizing.  

---

### 🖼️ **Schematic Overview**  
In the **center of the schematic**, we place the **ESP32 with the 2x15 pin holders**.  
Each **connected component (keypad, LCD, fingerprint sensor, RFID, buzzer, etc.)** should have its corresponding **pin headers assigned properly**.  

🔹 **Double-check the connections before proceeding to the PCB layout step!** 🚀  
---
### 🖼️ **Schematic Diagram Screenshot**  

Below is the **schematic diagram** for our ESP32-based project.

📷 **Schematic Image:**  
![Schematic Diagram](https://github.com/Hotsunlok/ESP32-smart-door-system/blob/ba315e3f5b43bbd971710a7208daeaa188669320/assets/%E8%9E%A2%E5%B9%95%E6%93%B7%E5%8F%96%E7%95%AB%E9%9D%A2%202025-02-02%20164745.jpg)  

---

# Step 2️⃣: Choosing the Correct Footprint  

Before transitioning to **PCB design**, we must ensure that every **component** has the **correct footprint** assigned.  

📌 **What is a Footprint?**  
A **footprint** is the physical representation of a component on the PCB, defining **pad sizes, hole placements, and overall dimensions**.  

---

### 🛠️ **How to Choose the Correct Footprint?**  

✔️ Ensure all components **match the correct footprint** from KiCad’s **library**.  
✔️ Double-check **pin spacing, hole sizes, and part dimensions**.  
✔️ The **YouTube tutorial above** explains in detail **how to choose the right footprint for each component**. 🎥✅  
---

🔹 **Once all footprints are assigned correctly, we can proceed to the PCB layout step! 🚀**  


---

# Step 3️⃣: Transition to PCB Design  

Now that all **footprints are assigned**, we can transition to the **PCB layout** stage in **KiCad 8.0**.  

### 📌 **Key PCB Design Rules to Follow:**  
✔️ **Separate** each component properly to ensure easy soldering.  
✔️ **Connect components to their correct pins** following the schematic.  
✔️ **Set trace widths correctly:**  
   - **Signal lines:** **0.25mm**  
   - **Power lines:** **0.7mm**  
✔️ **Avoid 90° trace bends** to prevent signal interference—use **45° angles instead**.  
✔️ **Use a ground plane** to simplify connections and reduce noise.  

---

### 🌍 **Understanding the Ground Plane (Blue Area)**  
In the **PCB layout**, the **blue area represents the ground plane (GND)**.  

🔹 **Why use a Ground Plane?**  
- **Reduces the number of ground wires**, making routing easier.  
- **Improves electrical performance** by reducing noise and interference.  
- **Creates a solid return path** for signals, enhancing stability.  

📌 **Instead of manually routing GND traces, we use a ground plane to connect all GND pins efficiently.**  

---

### 🎥 **Learn How to Add a Ground Plane in KiCad**  
📌 **Watch this tutorial to learn how to create a ground plane in KiCad:**  
🔗 **[https://www.youtube.com/watch?v=8NV5cuPbVm0]**  

---

### 🖼️ **PCB Layout Screenshot**  
📷 **Here is the current PCB layout preview:**  
![PCB Layout](https://github.com/Hotsunlok/ESP32-smart-door-system/blob/6f796f0997bf94123e59bae4679f8643c5f55ef5/assets/%E8%9E%A2%E5%B9%95%E6%93%B7%E5%8F%96%E7%95%AB%E9%9D%A2%202025-02-02%20170924.jpg)  

🔹 **Ensure all traces and the ground plane are properly set up before proceeding! 🚀**  

---

# Step 4️⃣: 3D View of the PCB Design  

Now that the **PCB layout is complete**, we can preview the **3D design** in KiCad to check component placements and trace routing.  

### 📌 **Key Observations:**  
✔️ The **traces are well-separated** to avoid short circuits.  
✔️ All **components will be connected to the pin headers**.  
✔️ The **final PCB design is almost finished! 🎉**  

---

### 🖼️ **3D PCB Views**  

📷 **Front View of the PCB:**  
![Front View](https://github.com/Hotsunlok/ESP32-smart-door-system/blob/c17789294cb9d61b73185ff2ce788facb6ddb51b/assets/%E8%9E%A2%E5%B9%95%E6%93%B7%E5%8F%96%E7%95%AB%E9%9D%A2%202025-02-02%20171132.jpg)  

📷 **Back View of the PCB:**  
![Back View](https://github.com/Hotsunlok/ESP32-smart-door-system/blob/3dc1f53f3c900b0d71ae52437a77a6815c146dc0/assets/%E8%9E%A2%E5%B9%95%E6%93%B7%E5%8F%96%E7%95%AB%E9%9D%A2%202025-02-02%20171749.jpg)  

🔹 **Ensure all components and traces are correct before generating the Gerber files! 🚀**  

## Step 5️⃣: Generating the PCB Files  

Before we can **order the PCB**, we need to **generate the required manufacturing files**.  

### 📌 **What Files Are Needed?**  
To send our PCB for production, we must generate these files:  
- **Gerber Files (GBR)** → Defines each PCB layer (copper, mask, silkscreen, etc.).  
- **Drill Files (DRL)** → Specifies hole placements for vias and through-hole components.  
- **Job File (GBO or similar)** → Contains manufacturing details for the PCB factory.  
- **BOM File (Bill of Materials)** → A list of all electronic components used in the PCB.  
- **CPL File (Component Placement List)** → Defines the position and orientation of surface-mounted components.

📌 **The YouTube tutorial from Step 1 explains how to generate these files correctly!** 🎥✅  

---

### 📦 **Final Step: Create a ZIP File**  
✔️ **Put all generated files into a single ZIP file** before uploading to the PCB manufacturer.  
✔️ This ensures all necessary **PCB fabrication data** is included.  

---

### 🖼️ **Example of Generated Files**  
📷 **Here is the final output before uploading:**  
![Generated Files](https://github.com/Hotsunlok/ESP32-smart-door-system/blob/1291ae6130610d9dcfed67a1fa0cf0d831de5b75/assets/%E8%9E%A2%E5%B9%95%E6%93%B7%E5%8F%96%E7%95%AB%E9%9D%A2%202025-02-02%20172147.jpg)  

📷 **BOM File Screenshot (Bill of Materials):**  
![BOM File](https://github.com/Hotsunlok/ESP32-smart-door-system/blob/5e44a782f54f593402d4f327f5e5815f86daca99/assets/%E8%9E%A2%E5%B9%95%E6%93%B7%E5%8F%96%E7%95%AB%E9%9D%A2%202025-02-02%20173300.jpg)  

📷 **CPL File Screenshot (Component Placement List):**  
![CPL File](https://github.com/Hotsunlok/ESP32-smart-door-system/blob/15f6470dfbba74b0726e36caea87c4dbf34a0048/assets/%E8%9E%A2%E5%B9%95%E6%93%B7%E5%8F%96%E7%95%AB%E9%9D%A2%202025-02-02%20173039.jpg)  

# 📤 Upload Your PCB Files to JLCPCB

Now that all necessary files are ready, it's time to **upload them to JLCPCB** for fabrication! 🎉  
You will need to upload **three files**:  

1️⃣ **Gerber ZIP File** (Contains all PCB manufacturing files)  
2️⃣ **BOM (Bill of Materials) File** (Lists all components for assembly)  
3️⃣ **CPL (Component Placement List) File** (Defines where each component is placed)  

### 🛠️ **Recommended: PCBA Assembly**
For beginners, **PCB assembly (PCBA)** is highly recommended ✅  
**Why?** Because **soldering small components manually is difficult** and may lead to errors. Let JLCPCB do the soldering for you!  

🔗 **Upload your files here:** [JLCPCB Official Website](https://jlcpcb.com/)  
