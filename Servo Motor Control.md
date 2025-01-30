### **🔄 Servo Motor Control**
The **SG90 Servo Motor** is the key mechanism responsible for **unlocking and locking the door** in the smart door system.

#### **🛒 Buy Here**
[🔗 Buy SG90 Servo Motor on Amazon](https://www.amazon.co.uk/Geared-Compatible-Arduino-Projects-Raspberry/dp/B0DRFCFWT2/ref=sr_1_1_sspa?dib=eyJ2IjoiMSJ9.eJdCue8wRYZOWumPf2F_0IXqHXtMphoFaz7lwEcx5AAGsoEXelEwz1qYj68epFg2V2RzX17eZ1G3f5mW43nOgJkIu0kb6XxsrfYZH_KZvsb5keTTrhFUUQalPdjmZPPsnP-gfFU4Pr_MsUAUfEbYEuzTYexJKwhEBALoOeM4TvG51fwMXnLN3h9W-_2CKkiYPNupOmsAJeXzfG17BQ0WixMz4237dv3fm0-42A6BAhcWWLofjnhBCYc9SEQvok5p3lXEoeBMW7y1P8x5HGW3OMS4oLmXzLAqS6MCktn5AnU._H4W4-Y0YmUKhS_B8mo606woGC2SVUx41fzAjR6HlNI&dib_tag=se&keywords=servo+sg90&qid=1738209640&sr=8-1-spons&sp_csd=d2lkZ2V0TmFtZT1zcF9hdGY&psc=1) 
#### **📸 Component Image**
![SG90 Servo Motor](https://github.com/Hotsunlok/ESP32-smart-door-system/blob/48c461fd77a8ed1667eb8eb9cc37e306ed4d006d/Servo-Motor-Wires.png) 

---

### **🔑 Role of the Servo Motor in the Door Lock System**
The **servo motor** moves a **steel rod**, which **pushes or pulls** the **sliding bolt lock**, allowing the door to be securely locked or unlocked.

#### **📸 How the Servo Motor Connects to the Door Lock**
![Servo Motor Door Lock Mechanism](https://github.com/Hotsunlok/ESP32-smart-door-system/blob/e42f3587dad0d0657aba5c3ec854f671800a46de/smartdoorsystempics.jpg) 

---
## 🔄 Servo Motor SG90

The **SG90 servo motor** is responsible for **locking and unlocking** the door. It connects to the ESP32 using the following pin configuration:

| **Servo Pin** | **ESP32 Pin** |
|--------------|--------------|
| VCC          | 5V           |
| GND          | GND          |
| Signal       | GPIO 13      |

The **signal pin** on **GPIO 13** receives commands from the ESP32 to rotate the servo, which in turn moves the **metal rod** to **lock or unlock** the sliding bolt door lock.

---
## ⚠️ **Very Important: Servo Motor Angles**
The **SG90 servo motor** operates at **two critical angles** to control the **door lock mechanism**:

| **Angle (Degrees)** | **Action** |
|-------------------|-------------|
| 🔒 **110°**      | **Locks** the door (pushes the steel rod into the sliding bolt lock). |
| 🔓 **50°**       | **Unlocks** the door (pulls the steel rod out of the sliding bolt lock). |

👉 The ESP32 sends a signal to **rotate the servo** between these two positions when a valid unlock method is used.

⚡ **Key Implementation from Code:**
```cpp
servo.write(lock ? 110 : 50); // Moves the servo to lock (110°) or unlock (50°)
```
---
### ✅ Unlock Case (Successful Authentication)
When a correct password, fingerprint, or RFID card is used, the **servo motor moves to 50°**, unlocking the door.

```cpp
servo.write(50); // Move servo to unlock position
```

📌 **Visual Representation:**
![Unlock Flow](https://github.com/Hotsunlok/ESP32-smart-door-system/blob/fc451fee8161c6a70298a61cfaf564ad6ce5002c/servomotorunlock.jpg)

---

### 🔒 Lock Case (Successful Authentication)
When the system locks the door (e.g., user manually locks it via keypad or toggle button), the **servo motor moves to 110°**, locking the door.

```cpp
servo.write(110); // Move servo to lock position
```

📌 **Visual Representation:**
![Lock Flow](docs/servo_lock.png)

---

### ❌ Wrong Access Case (Failed Authentication)
If authentication fails (incorrect password, fingerprint, or unregistered card), the **servo motor does not move** and stays in its current position.

```cpp
// No servo movement, remains in the last state
```

---

### ⏳ Auto-Lock Timer Case (Timeout Mechanism)
If the door is unlocked but no action is taken within **10 seconds**, the **servo motor automatically moves back to 110°** to lock the door.

```cpp
if (millis() - unlockTime >= countdownDuration) {
    servo.write(110); // Auto-lock door
}
```

📌 **Auto-lock timer starts counting down from 10s on the web server display.**

---

