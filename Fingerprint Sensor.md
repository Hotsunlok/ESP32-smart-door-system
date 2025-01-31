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

I have successfully **used the enrollment code** from the provided tutorial to **store my right thumb fingerprint** into **storage location `1`**.  

📌 **What This Means:**  
- The system is now **ready to recognize my right thumb fingerprint** for unlocking/locking the door.  
- Any **fingerprint that is not stored** will **not be recognized** by the sensor.  

---

### 🖼 **Right Thumb Fingerprint Enrollment Record**  
![Thumb Fingerprint Enrollment](https://github.com/Hotsunlok/ESP32-smart-door-system/blob/5f299af6d74d09be147f61894c4203287f9f7841/correctfinger.jpg)

---

## 🔗 ESP32 Communication with R307 Sensor  

The **ESP32** communicates with the **R307 fingerprint sensor** using the **correct RX/TX pins**.  
Before reading a fingerprint, the **ESP32 first verifies** if the fingerprint sensor is connected correctly.

📌 **finger.verifyPassword()** is used to **check the fingerprint sensor**:  
- ✅ **If successful:** It prints `"Found fingerprint sensor!"` to the **Serial Monitor**.  
- ❌ **If failed:** It prints `"Did not find fingerprint sensor :("` to the **Serial Monitor**.  

---

### 🛠 **Code Execution: Checking Sensor Connection**  
```cpp
mySerial.begin(57600, SERIAL_8N1, 16, 17);  // RX = GPIO16, TX = GPIO17 for Serial1

if (finger.verifyPassword()) {
    Serial.println("Found fingerprint sensor!");
} else {
    Serial.println("Did not find fingerprint sensor :(");
    while (1) { delay(1); }  // Halt execution if sensor is not detected
}
```
## 🔎 Capturing Fingerprint for Verification  

Once the **fingerprint sensor** is **confirmed to be working**,  
it proceeds to **capture the user's fingerprint** when they place their finger on the sensor.

### 🔥 `getFingerprintID()` function:
- **Detects when a fingerprint is placed** on the sensor.
- **Stores the scanned fingerprint** in the variable `p`.
- If the fingerprint is **not detected or incorrect**, it does nothing.

---

### 🛠 **Code Execution: Capturing Fingerprint**
When the **user places a finger** on the sensor, the ESP32 captures the fingerprint data.

```cpp
void getFingerprintID() {  
    uint8_t p = finger.getImage();   // Capture fingerprint image  

    if (p != FINGERPRINT_OK) {  
        return;  // No finger or error, do nothing  
    }  
}
```
## 🔄 Converting Fingerprint Image into Template  

After **capturing**, the fingerprint image needs to be **converted into a digital template**.  
This is necessary because **raw fingerprint images cannot be directly compared**.

### 🔥 How It Works?  
✔ **The function `finger.image2Tz()` converts** the captured image into a fingerprint **template**.  
✔ The **template will be used for comparison** with pre-stored fingerprints.  

---

### 🛠 **Code Execution: Converting Fingerprint Image**  

```cpp
p = finger.image2Tz();

if (p != FINGERPRINT_OK) {
    // Could not process fingerprint
    showError("fingerprint");
    return;
}
```
## 🔍 Fingerprint Verification  

Once the **fingerprint template** is ready, it will be **compared with pre-stored fingerprints**.

### 🔥 What Happens?  
✔ **If matched**, the system **unlocks/locks** the door and updates the **LCD, buzzer, and log**.  
❌ **If not matched**, the system **triggers an error message** and **beeps twice**.  

---

### 🛠 **Code Execution: Fingerprint Verification**  

```cpp
p = finger.fingerSearch();

if (p == FINGERPRINT_OK) {
    // Check if matched fingerprint ID is 1 (user's fingerprint)
    if (finger.fingerID == 1) {
        // Correct fingerprint
        doorLocked = !doorLocked;  // Toggle door status
        controlDoor(doorLocked, "fingerprint");
    } else {
        // Not the user's fingerprint
        showError("fingerprint");
    }
} else {
    // Fingerprint not recognized
    showError("fingerprint");
}
```
### 📸 **fingerprint sensor whole process flow:**
![fingerprint sensor whole process flow](https://github.com/Hotsunlok/ESP32-smart-door-system/blob/1a545b59880407005bbf9f95d3cf70a360adaa20/fingerprintvertfity2.jpg)
