# 🏠 Real-World Testing: Smart Door System

After confirming all the methods and components are working fine, a **real-world use case** can be conducted by **combining different components** and **multiple methods** to unlock and lock the door.  
This ensures that the **Smart Door System** operates **smoothly** and that all components work together **effectively**.

### 🔄 **Testing Sequence**
To test the performance and **reliability** of the system, the following sequence is used:

1️⃣ **Unlock using Keypad**  
2️⃣ **Lock using RFID Card**  
3️⃣ **Unlock using Fingerprint Sensor**  
4️⃣ **Lock using Web Interface (Toggle Switch Button)**  

By **switching between different methods**, this test confirms that the **Smart Door System** can handle **seamless transitions** across all unlocking and locking methods.  
This ensures **smooth performance** and provides a **reliable** access control system.

📸 **Overall Sequence Testing - Smart Door System**
![Upload Image Here](https://github.com/Hotsunlok/ESP32-smart-door-system/blob/46a39429787227c524423c66d8675fc626f4df13/assets/Overallsequencetesting.jpg)

---

### 🔢 Step 1: Unlocking with Keypad

Once the code is uploaded, the **Smart Door System** starts in a **locked state**.  

#### 📌 **User Action**
- The user enters **`1234`** on the **keypad** to unlock the door.

#### 📟 **LCD Response**
- The **LCD displays** the entered password.  
- Then, it **updates to** `"Unlocked"` at the end.

#### 🔊 **Other Component Responses**
- **Buzzer beeps once** to confirm unlocking.  
- **Servo motor rotates** to **50°**, pulling the door lock **to unlock**.  

#### 🌐 **Web Interface Response**
- The **toggle switch** button **automatically slides to green (right)**.  
- The **access log box updates** to: `2. The door is unlocked (by keypad).`


📸 **Overall Physical System - Unlocking with Keypad**  
![Upload Image Here](https://github.com/Hotsunlok/ESP32-smart-door-system/blob/61217594ae0c56df7eb9f97c5458340369792abb/assets/step1.jpg)

---
### 📡 Step 2: Locking with RFID

After the door was **unlocked by keypad**, this time the user **uses the RFID card module** to **lock the door back**.

#### 📌 **User Action**
- The user **attaches the correct RFID card** to the **RC522 card module**.

#### 📟 **LCD Response**
- The **LCD first updates** to `"Thinking..."`.  
- Then, it **displays `"Locked"` at the end**.

#### 🔊 **Other Component Responses**
- **Buzzer beeps once** to confirm locking.  
- **Servo motor rotates** to **110°**, pushing the **door lock into the locked position**.  

#### 🌐 **Web Interface Response**
- The **toggle switch** button **automatically slides to red (left)**.  
- The **access log box updates** to: `3. The door is locked (by RFID).`



📸 **Overall Physical System - Locking with RFID**  
![Upload Image Here](Your-Image-Link-Here)

