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
![Password Incorrect Flowchart](https://github.com/Hotsunlok/ESP32-smart-door-system/blob/9a1926dd4d3dac89c83d371273d0576741b9f00f/passwordvertifcationsecond.jpg)
---

### ✅ Case 3: User Entered Correct Password  

If the user **enters the correct password** (`"1234"`) and presses **"Verify"**, the system will:

📌 **What Happens:**  
- Display an **alert message**: `"Password correct! You can now use the switch to unlock or lock the door."`
- The **password verification box disappears**, confirming that the user **does not need to re-enter the password**.
- The user can now **toggle the switch freely** between **left (red) → lock** and **right (green) → unlock**.
- The ESP32 **receives a message**, and the **servo motor moves** to either **push** or **pull** the sliding bolt lock.

🖥️ **Code Section**  
The JavaScript function below ensures the system processes a **correct password** properly:

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

function handleChange() {
    if (!passwordVerified) {  // Prevent action if password is not entered
        alert('Please enter the correct password first.');
        document.getElementById('switch-1').checked = doorLocked;
        return;
    }
    // Send toggle switch state to ESP32
}
```
🖼 **Flowchart Representation:**  
![Password Incorrect Flowchart](https://github.com/Hotsunlok/ESP32-smart-door-system/blob/82e01ab81b100b83fa85416bc8e4eb55b1f338fb/passwordvertficationthird.jpg)

---

## 🎨 **Toggle Switch: Light/Dark Mode**

The web server allows users to **toggle between Bright and Dark Mode** for the background color.
The toggle switch button is located at right corner of the web server.

| **Toggle Switch State** | **Website Background Color** |
|------------------------|---------------------------|
| 🌞 Bright Mode  | ![Light Mode Preview](https://github.com/Hotsunlok/ESP32-smart-door-system/blob/22d66dc8e34a175a1ffef51a429d923c3def3629/IMG_2774.PNG) |
| 🌙 Dark Mode  | ![Dark Mode Preview](https://github.com/Hotsunlok/ESP32-smart-door-system/blob/634c33304805957b2c7cbcf3c1b20e0222ee1fb8/IMG_2805%20(1).PNG) |

---

## 🎨 Floating Dynamic Color Logo - "Smart Door"  

The **"Smart Door" logo** on the web server has two unique **visual effects**:  

1️⃣ **Dynamic Colors**: The text cycles through **pink, orange, yellow, green, and sky blue** in a smooth sequence.  
2️⃣ **Floating Animation**: The logo moves **up and down smoothly** in a **continuous motion**.

🖼 **Dynamic Colors & Floating Logo (Light Mode):**  
![Floating Logo Light Mode](https://github.com/Hotsunlok/ESP32-smart-door-system/blob/6e8be9b2b424b340aeccd2ce705aff499ab92459/brightlogo.jpg)

🖼 **Dynamic Colors & Floating Logo (Dark Mode):**  
![Floating Logo Dark Mode](https://github.com/Hotsunlok/ESP32-smart-door-system/blob/3efdc96c2ba67ea648f76050c5670af9fd4d331b/darklogo.jpg)

---

## 🔄 Toggle Switch Button - Controlling the Servo Motor  

The **toggle switch button** allows users to **manually** slide between **locked (red)** and **unlocked (green)** states, or it can be **automatically updated** by other authentication methods (Keypad, RFID, Fingerprint, etc.).  

### 💡 Why Is This Feature Important?  
✔ **Remote Control:** Users can **lock/unlock the door remotely** by simply sliding the button.  
✔ **User-Friendly:** It is **convenient and intuitive** to operate from a distance.  
✔ **Triggers All Output Components:** The **LCD, Buzzer, and Servo Motor** respond accordingly.  

---
## 🟢 Case 1: Sliding from Locked (Red) to Unlocked (Green)  

When the user **slides the toggle switch from red (locked) to green (unlocked)**, the ESP32 processes the unlock request and **triggers the servo motor to unlock the door**.

🖼 **Green Toggle Switch Button (Unlocked State):**  
![Green Toggle Switch](docs/images/toggle_switch_unlocked.png)  

---

### 🛠 **Code Execution for Unlocking**  
When the **unlock command** is received, the ESP32 updates the `doorLocked` variable and calls `controlDoor()` to activate all output components.  

```cpp
if (message == "unlock") {  
    if (doorLocked) {  
        doorLocked = false;  
        controlDoor(doorLocked, "website");  
    }  
}
