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

![WebSocket Diagram](docs/images/websocket_diagram.png)

---

### ✅ **Next Steps**
- Explain **UI features** (toggle switch, themes, access logs).  
- Show WebSocket **implementation in ESP32 code**.  

