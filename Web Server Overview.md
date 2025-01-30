# 🌐 Web Server Overview

## 🏠 ESP32 Web Server & Real-Time Communication  
The **ESP32** creates a **local web server** at `192.168.4.1`, allowing users to control the smart door system remotely via a browser (e.g., iPad, laptop, or smartphone).  

This web server is powered by **WebSocket protocol**, which enables **real-time** and **two-way communication** between the ESP32 (server) and the connected devices (clients).  

---

## 🔄 **What is WebSocket?**
WebSocket is a protocol that establishes a **continuous**, **bi-directional** connection between the ESP32 and the client device **without requiring constant page refreshes**.

### 📌 **Why Use WebSocket?**
- 🚀 **Real-Time Updates**: Sends data instantly between the **ESP32 (server)** and **user device (client)**.
- 🔄 **Persistent Connection**: Unlike traditional **HTTP**, WebSocket **does not require reloading the page** to update data.
- ✅ **User-Friendly**: Keeps the access log, toggle switch, and UI state updated **without manual refreshes**.

---

## 🔍 **WebSocket vs. Traditional HTTP**
| Feature         | WebSocket ✅ | Traditional HTTP ❌ |
|---------------|-------------|------------------|
| **Real-Time Communication** | ✅ Yes | ❌ No (Needs page refresh) |
| **Bidirectional Data Flow** | ✅ Yes (Server ↔ Client) | ❌ No (Client → Server Only) |
| **Persistent Connection** | ✅ Yes | ❌ No (New request every time) |
| **Efficiency** | ✅ High | ❌ Low (Repeated Requests) |

📌 **Example Use Case:**  
- **Without WebSocket**: The user must **refresh the page** to see the latest access logs.  
- **With WebSocket**: The **webpage updates automatically** when new data is received.  

---

## 🖼 **WebSocket Concept Illustration**  
*(Upload your WebSocket concept image here)*  

![WebSocket Diagram](https://github.com/Hotsunlok/ESP32-smart-door-system/blob/5098cd4271fb7b87008d8ab8eeb74e560be74fe2/%E5%9C%96%E7%89%872.jpg)

---
## 🔗 **Web Server Installation & Real-Time Update Reference**  
📌 **Web Server Setup & Real-Time Toggle Switch Tutorial:**  
[ESP32 Web Server & Toggle Switch Guide](https://randomnerdtutorials.com/esp32-esp8266-web-server-physical-button/)

---
Below is a **screenshot of the web server interface**, showing the complete layout of the Smart Door System website.

🖼 **Web Server Screenshot:**  
![Web Server Screenshot](your-screenshot-link-here)

---

## 🎨 **Toggle Switch: Light/Dark Mode**

The web server allows users to **toggle between Bright and Dark Mode** for the background color.

| **Toggle Switch State** | **Website Background Color** |
|------------------------|---------------------------|
| 🌞 Bright Mode  | ![Light Mode Preview](your-light-mode-image-link-here) |
| 🌙 Dark Mode  | ![Dark Mode Preview](your-dark-mode-image-link-here) |


