# 🖐️ Fingerprint Sensor (R307)

The **R307 Fingerprint Sensor** is one of the four methods to access the **Smart Door System**.  
It allows users to **place their fingerprint on the sensor**, and the system will **verify it** against the **pre-stored fingerprints** to determine if the door should be **locked or unlocked**.

---

## 📸 Component Image  
![Upload Fingerprint Sensor Image Here](https://github.com/Hotsunlok/ESP32-smart-door-system/blob/6ca8ccffe5c2c3b5e3d2222b82a1830413777334/41JBJ81tQzL._AC_US100_.jpg)  

---

## 🛒 Purchase Link  
[Upload Amazon Purchase Link Here](https://www.amazon.co.uk/Fingerprint-Optical-Control-Attendance-Recognition/dp/B07SDG2LWM)

---

## 🔌 Fingerprint Sensor R307 - Pin Configuration

The **R307 Fingerprint Sensor** communicates with the **ESP32** using **UART serial communication**.  
It has **four required connections** and **two additional wires that are not needed**.

---

### 📌 **Pin Connection Table**
| **R307 Fingerprint Sensor Pins** | **ESP32 Pins** |
|----------------------------|----------------|
| **TXD**  | GPIO 16 (**RX**) |
| **RXD**  | GPIO 17 (**TX**) |
| **5V**   | Vin |
| **GND**  | GND |

---

## 🎥 **Correct Pin Connection - Video Tutorial**
For a step-by-step guide on how to **correctly wire the R307 Fingerprint Sensor**, watch the **YouTube tutorial** below:  
[Upload YouTube Video Link Here](https://www.youtube.com/watch?v=s5zPQPJ4h9k&t=1s)

---

## 📖 **Fingerprint Enrollment & Matching Tutorials**
In this section, we assume that you are **already familiar with the fingerprint detection and enrollment process**.  
If you need a reference, please visit the following **tutorials**:

🔹 **Fingerprint Enrollment and Matching Guide:** [Upload Enrollment and Matching Tutorial Link Here](https://www.instructables.com/Interfacing-Fingerprint-Sensor-With-Arduino/)  

---

## ⚠️ **Precaution Reminder - Fingerprint Enrollment is Required!**

Before using the **R307 Fingerprint Sensor**, you must **store fingerprints into a storage location**.  
This is **especially important for the enrollment process** because the code assumes that **your fingerprint has already been saved**.

📌 **Important Note:**  
- The current system assumes that **your thumb fingerprint is stored in location `1`**.  
- **If you have not saved your fingerprint,** the sensor **WILL NOT** recognize your fingerprint.  
- Follow the **fingerprint enrollment guide** to **correctly store fingerprints** before testing.

---

## ✅ **Fingerprint Enrollment - Thumb Database Stored**  

I have successfully **used the enrollment code** from the provided tutorial to **store my thumb fingerprint** into **storage location `1`**.  

📌 **What This Means:**  
- The system is now **ready to recognize my thumb fingerprint** for unlocking/locking the door.  
- Any **fingerprint that is not stored** will **not be recognized** by the sensor.  

---

### 🖼 **Thumb Fingerprint Enrollment Record**  
![Thumb Fingerprint Enrollment](UPLOAD_YOUR_IMAGE_HERE)

