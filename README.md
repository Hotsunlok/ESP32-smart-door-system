# 🚪 ESP32 Smart Door System
This is a **smart door lock** project using an ESP32, Face ID (Python + OpenCV), and multiple unlocking methods: RFID, fingerprint, keypad, and a web interface.
## 📂 Project Documentation

### 🔥 Full Source Code
- 🚀 [Full Arduino Code (ESP32)](Full_Arduino_Code.md)
- 📸 [Full Python Face Recognition Code](Full_Python_Face_Recognition_Code.md)

### 🛠️ System Components
- ✏️ [ESP32 Overview](ESP32%20Overview.md).
- 🔄 [Servo Motor Control](docs/Servo_Motor.md)
- 🔢 [Keypad Authentication](docs/Keypad.md)
- 📡 [RFID System](docs/RFID.md)
- 🖥️ [LCD Display](docs/LCD.md)
- 🌐 [Web Server](docs/Web_Server.md)

### 📌 Pin Connections & Hardware
- 🛠️ [ESP32 Pin Mappings](ESP32_Pin_Mappings.md)  
  *(Includes all wiring tables for LCD, keypad, RFID, buzzer, servo, and fingerprint sensor.)*


### 📸 Python Face Recognition
- 🤖 [Face Recognition System](docs/Face_Recognition.md)  
  *(Explains how the PC uses OpenCV to detect faces and send unlock signals to ESP32.)*

### 🔌 PCB Design (KiCad)
- 🛠️ [KiCad PCB Schematic & Layout](docs/PCB_Design.md)  
  *(Final PCB circuit layout for ESP32-based smart door lock system.)*


---
## 🖼️ Project Overview

### Breadboard Setup
![Breadboard Setup](https://github.com/Hotsunlok/ESP32-smart-door-system/blob/9c6adecdadc2188d5d92d883d73e239462644fc8/EACBA9AD-779E-45AD-859F-B7F50C0F657A.jpg)

### Full Desk Setup
![Full Desk Setup](https://github.com/Hotsunlok/ESP32-smart-door-system/blob/504fe93a245971ee61a8f2bdae0d332685c17fb9/IMG_9987.jpg)

---

## 🔧 Components Used
| Component | Description | Image |
|-----------|------------|-------|
| **Microcontroller ESP-WROOM-32 Chip CP2102** | Main microcontroller | ![ESP32](https://github.com/Hotsunlok/ESP32-smart-door-system/blob/482a508ad48384088fca2f66ab7106a4a5123ace/IMG_8218.jpg) |
| **RFID RC522 Module** | Scans NFC/RFID cards | ![RFID](https://github.com/Hotsunlok/ESP32-smart-door-system/blob/c5b92e1e99c90cc05f0fa6ddd7725cabe4d1019b/IMG_8212.jpg) |
| **Fingerprint Sensor R307** | Unlocks with a fingerprint | ![Fingerprint](https://github.com/Hotsunlok/ESP32-smart-door-system/blob/421966717244247ce77089965ba3aca17e2ff172/IMG_8226.jpg) |
| **Keypad** | Allows password entry | ![Keypad](https://github.com/Hotsunlok/ESP32-smart-door-system/blob/4478092b2e4120dc1fb2c60b1cf059ac32181918/IMG_8211.jpg) |
| **LCD 1602 I2C** | Shows messages | ![LCD](https://github.com/Hotsunlok/ESP32-smart-door-system/blob/ad300b9bc206309a1d03f3c098b7372628aceaa9/IMG_8222.jpg) |
| **Buzzer** | Beeps for alerts | ![Buzzer](https://github.com/Hotsunlok/ESP32-smart-door-system/blob/68672755558138dd45b397e85a5573a357a4886c/IMG_8230.jpg) |
| **Servo Motor** | Controls door lock | ![Servo](https://github.com/Hotsunlok/ESP32-smart-door-system/blob/e0d2e725ebff193361b7f4becfa9c9968e0833a8/B7D6B00C-865B-4646-9274-3ECE02B5C609.jpg) |

---
## 🏠 How It Works

1️⃣ **User Unlocks the Door Using One of These Methods:**
   - **RFID card** (Scans an NFC card to unlock)
   - **Fingerprint sensor** (Authenticates and unlocks)
   - **Keypad password** (Enters the correct password)
   - **Web interface button** (Toggles lock/unlock)
   - **Face ID detection** (PC recognizes the face and sends unlock signal)

2️⃣ **ESP32 Processes the Unlock Request**
- The ESP32 **receives the signal** from the selected unlock method.
- **If authentication is correct:**
  - ✅ **Servo motor** rotates to unlock the door.
  - ✅ **LCD** displays `"The door is unlocked (by [method])"` (e.g., keypad, RFID).
  - ✅ **Buzzer** beeps **once** as confirmation.
  - ✅ **Web Server** updates with `"The door is unlocked (by [method])"`.

- **If authentication fails:**
  - ❌ **Servo motor** stays in its position (no movement).
  - ❌ **LCD** displays `"Wrong Access"`.
  - ❌ **Buzzer** beeps **twice** as a warning.
  - ❌ **Web Server** updates with `"Wrong Access (by [method])"`.


3️⃣ **ESP32 Web Server Features**
   - The ESP32 **hosts a Wi-Fi network** (`http://192.168.4.1`).
   - Users can **monitor access logs** (see who unlocked the door).
   - bright☀️/dark🌙 background mode is allowed to choose.
   - A **toggle switch button** allows manual lock/unlock control.
   - **Auto-timer lock** re-locks the door after a set time.

4️⃣ **Face ID Integration with Python & OpenCV**
   - A **PC with a USB camera** runs **Python + OpenCV Face Recognition**.
   - If the **detected face matches**, the PC **sends a wireless signal (via HTTP/WebSocket)** to ESP32.
   - The ESP32 **receives the Face ID signal** and **unlocks the door** (same as other methods).
   - If Face ID **fails**, it logs the failed attempt.

5️⃣ **Real-Time System Updates**
- Any unlocking event **updates the web interface in real-time**.
  
- **If authentication is correct:**
  - ✅ **Web interface updates** to `"The door is unlocked (by [method])"`.
  - ✅ **LCD** displays `"The door is unlocked (by [method])"` (e.g., `"by keypad"`, `"by RFID"`).
  - ✅ **System auto-locks** after a set time if no further actions occur.

- **If authentication fails:**
  - ❌ **Web interface updates** to `"Wrong Access (by [method])"`.
  - ❌ **LCD** displays `"Wrong Access"`.
  - ❌ **Buzzer beeps twice** as a warning.
  - ❌ **No failed attempt logging in the system** (only the web server updates).
 
6️⃣ **Auto-Lock Timer Feature**
- After the door is unlocked, a **10-second countdown** begins.
- A **timer display appears on the web server**, showing:
  - **Starts at `00:10`** → **Counts down every second** → **Reaches `00:00`**.
  - Reminds the user to close the door before it auto-locks.
- When the **countdown reaches `00:00`**:
  - ✅ **Servo motor locks the door automatically**.
  - ✅ **Web interface updates** to `"The door is locked (by auto-lock)"`.
  - ✅ **LCD displays** `"The door is locked (by auto-lock)"`.



