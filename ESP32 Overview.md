# ✏️ ESP32 Overview

## 📸 Component Image
![ESP32-WROOM-32](https://github.com/Hotsunlok/ESP32-smart-door-system/blob/0cfe98380a1be4ccb8e51283876fd8ed71c71059/612qHR9BZgL._AC_SY450_.jpg)

### **Microcontroller Model:**  
ESP-WROOM-32 Chip CP2102  

### **Where to Buy:**  
[Amazon Link](https://www.amazon.co.uk/ESP-32S-Development-Bluetooth-Microcontroller-ESP-WROOM-32/dp/B08DR5T897/ref=asc_df_B08DR5T897?mcid=ee047e54ce243e93859155bdb9b40a0a&tag=googshopuk-21&linkCode=df0&hvadid=696386561245&hvpos=&hvnetw=g&hvrand=16974580648507383228&hvpone=&hvptwo=&hvqmt=&hvdev=c&hvdvcmdl=&hvlocint=&hvlocphy=9045836&hvtargid=pla-954584712882&gad_source=1&th=1)  

---

## 🚀 Setting Up ESP32 in Arduino IDE
Before uploading the code, install ESP32 board support in Arduino IDE.  
Follow this tutorial: [ESP32 Setup Guide](https://randomnerdtutorials.com/installing-the-esp32-board-in-arduino-ide/)  

---

## 🌐 Creating Wi-Fi Access Point (AP Mode)
ESP32 **creates a Wi-Fi hotspot** so that devices (like an iPad or laptop) can connect and access the smart door system via a web browser.

### **📌 Wi-Fi Setup Code**
```cpp
const char* ssid = "ESP32_AP";
const char* password = "12345678";
WiFi.softAP(ssid, password);
```
---

## 📶 Connecting to ESP32's Wi-Fi on iPad

After ESP32 **creates the Wi-Fi network**, users can connect by selecting **ESP32_AP** from the available Wi-Fi networks.

---

### 📸 **Example Screenshot from iPad Wi-Fi Settings:**
![iPad Wi-Fi Screenshot](https://github.com/Hotsunlok/ESP32-smart-door-system/blob/fc73af3fad89f7f9fe778beef999e5649898aa7c/%E5%9C%96%E7%89%871.png)

✅ **Wi-Fi Name:** `ESP32_AP`  
🔑 **Password:** `12345678`  

Once connected, open a web browser and go to **[http://192.168.4.1](http://192.168.4.1)** to access the web server.

⚠️ **Note:** `192.168.4.1` is the default **ESP32 AP mode IP address**, but it **may be different** depending on your configuration.

---

## 🔗 Setting Up a Web Server on ESP32
Want to learn how to set up a **web server on ESP32**? Check out this tutorial:  
📖 [ESP32 Web Server Tutorial](https://randomnerdtutorials.com/esp32-web-server-arduino-ide/)


---

# 🔷 ESP32’s Role in the Smart Door System

ESP32 processes **all authentication methods** and sends commands to **output components**:

| **Unlock Method**        | **ESP32 Action** |
|--------------------------|----------------|
| 🔢 **Keypad**            | Checks password and calls `controlDoor()` |
| 🛡️ **Fingerprint Sensor** | Recognizes fingerprint and calls `controlDoor()` |
| 📡 **RFID/NFC**          | Reads card ID and calls `controlDoor()` |
| 🤖 **Face Recognition (PC)** | PC sends unlock command via `/face_toggle` API |
| 🌐 **Web Server Toggle Button** | Web interface changes state dynamically |

---

## ⚡ `controlDoor(bool lock, String method)`

Whenever a user **successfully authenticates**, ESP32 calls this function (FOR EXAMPLE):

```cpp
controlDoor(doorLocked, "fingerprint");
```
---

# 🌐 ESP32 and Web Server Integration

ESP32 hosts a real-time **web server** where users can:

- 📜 **Monitor** logs of access events.
- 🔓 **Manually lock/unlock** the door.
- ⏳ **View auto-lock countdown timer.**

---

## 📌 Web Server Setup
```cpp
server.on("/", HTTP_GET, handleRoot);
ws.onEvent(onWebSocketEvent);

