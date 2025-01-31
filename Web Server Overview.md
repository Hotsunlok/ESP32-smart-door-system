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
![Web Server Screenshot](https://github.com/Hotsunlok/ESP32-smart-door-system/blob/dde994014058fbe2cb3d35e2f19313285ace89e0/IMG_2781.PNG)

---
## 🔐 Web Interface Password Verification

Once the device has just entered the **web interface**, a **password verification box** will be displayed.  
This step ensures that only **authorized users** can control the **toggle switch button** to lock/unlock the door, preventing **unauthorized access**.

---

### 📸 Password Verification Box  
![Password Verification Box](https://github.com/Hotsunlok/ESP32-smart-door-system/blob/db617ef3196e590160f9f0fb567348502f46f174/IMG_2789.jpg
)  

---

### 🚫 Case 1: User Did Not Enter Password & Tried to Toggle the Switch  

If the user **does not enter the password** but **attempts to slide the toggle switch** to the **right (green)**, the system will:

📌 **What Happens:**  
- Display an **alert message**: `"Please enter the correct password first."`
- The **toggle switch visually changes**, but the **servo motor does NOT move**.
- The **auto-lock timer does NOT activate**.

🖥️ **Code Section**  
The following JavaScript function ensures that users **must enter the correct password** before using the toggle switch:

```javascript
function handleChange() {
    if (!passwordVerified) {
        alert('Please enter the correct password first.');
        document.getElementById('switch-1').checked = doorLocked; // Reset switch state
        return;
    }
}
```
🖼 **Flowchart Representation:**  
![Password Verification Flowchart](https://github.com/Hotsunlok/ESP32-smart-door-system/blob/8a93a33afe6b42ea867b3a3e95d8c6104a9650ee/password%20vertifcationfirst.jpg)
---

### ❌ Case 2: User Enters Wrong Password

If the user enters the **wrong password** (e.g., `"123"`) and presses the green **"Verify"** button, the system will:

📌 **What Happens:**  
- Display an **alert message**: `"Incorrect password. Please try again."`
- The **toggle switch still allows sliding**, but **no message is sent to ESP32**.
- The **servo motor remains locked** and does NOT move.
- **Timer behavior**: The toggle switch remains in a fixed **green (right)** state, but it does **NOT** activate the auto-lock timer.

🖥️ **Code Section**  
The JavaScript function below checks whether the entered password is **correct or incorrect** and handles the response accordingly:

```javascript
function verifyPassword() {
    const password = document.getElementById('password').value;
    if (password === '1234') {  // Correct Password
        passwordVerified = true;
        document.getElementById('passwordBox').style.display = 'none'; // Hide input box
        alert('Password correct! You can now use the switch to unlock or lock the door.');
    } else {  // Incorrect Password
        alert('Incorrect password. Please try again.');
    }
}
```
🖼 **Flowchart Representation:**  
![Password Incorrect Flowchart](https://github.com/Hotsunlok/ESP32-smart-door-system/blob/main/docs/images/password_wrong_flowchart.png)


## 🎨 **Toggle Switch: Light/Dark Mode**

The web server allows users to **toggle between Bright and Dark Mode** for the background color.
The toggle switch button is located at right corner of the web server.

| **Toggle Switch State** | **Website Background Color** |
|------------------------|---------------------------|
| 🌞 Bright Mode  | ![Light Mode Preview](https://github.com/Hotsunlok/ESP32-smart-door-system/blob/22d66dc8e34a175a1ffef51a429d923c3def3629/IMG_2774.PNG) |
| 🌙 Dark Mode  | ![Dark Mode Preview](https://github.com/Hotsunlok/ESP32-smart-door-system/blob/634c33304805957b2c7cbcf3c1b20e0222ee1fb8/IMG_2805%20(1).PNG) |


