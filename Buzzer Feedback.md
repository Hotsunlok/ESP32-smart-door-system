## 🔊 Buzzer Feedback

The **buzzer** provides an **auditory response** to indicate whether access is **granted or denied**. It also **beeps once** when the **auto-lock timer** activates.

### 📸 Component Image
![Buzzer Component](https://github.com/Hotsunlok/ESP32-smart-door-system/blob/d2b32e912f32f6dad561519f2477efcfb5e3b2b2/%E8%9E%A2%E5%B9%95%E6%93%B7%E5%8F%96%E7%95%AB%E9%9D%A2%202025-01-30%20054809.jpg)


### 🛒 Purchase Link
[🔗 Buy Buzzer on Amazon](https://www.amazon.co.uk/Buzzer-Electromagnetic-Active-Electronic-Directly/dp/B07Y653F2S)

---
### 📌 **Buzzer Pin Connections**
| **Buzzer Pin** | **ESP32 Pin** |
|--------------|------------|
| GND          | GND        |
| Signal       | GPIO 23    |

---
## 🔊 Buzzer Response - Correct Authentication (1 Beep)

### ✅ **Simple Explanation**
When the user **enters the correct password** (by **Keypad**, **Fingerprint**, **RFID**, or **Web Toggle Button**), the **buzzer will beep once** to confirm successful authentication.

📌 **How It Works:**  
- The ESP32 checks if the entered input is **correct**.  
- If valid, the **buzzer beeps once** for **300 milliseconds**.  
- This provides an **audio confirmation** that the door has been locked or unlocked.

🖼 **Visual Representation:**  
![Buzzer Confirmation](https://github.com/Hotsunlok/ESP32-smart-door-system/blob/5fcc2037695b521aef675372914df5a86b403ccd/%E8%9E%A2%E5%B9%95%E6%93%B7%E5%8F%96%E7%95%AB%E9%9D%A2%202025-01-30%20072436.jpg)  

---

### 🛠 **Technical Explanation**
When the user **authenticates successfully**, ESP32 calls the function:

```cpp
controlDoor(doorLocked, "keypad");   // Example: Correct keypad password
```
Inside `controlDoor()`, the buzzer is triggered as follows:
```cpp
void controlDoor(bool lock, String method) {
    // Beep once for correct authentication
    digitalWrite(BUZZER_PIN, HIGH);  // Turn buzzer ON
    delay(300);                      // Keep it ON for 300ms
    digitalWrite(BUZZER_PIN, LOW);   // Turn buzzer OFF
}
```
🖼 **Technical Representation:** 
![Buzzer Technical Flowchart](https://github.com/Hotsunlok/ESP32-smart-door-system/blob/6f1cfeabb119c8fc78823351eb587660b3ef315f/buzzertechnical.jpg)

---
## 🔊 Buzzer Response - Wrong Authentication (2 Beeps)

### ❌ **Simple Explanation**
When the user **enters the wrong password** (via **Keypad**), the **buzzer will beep twice** to indicate **failed authentication**.

📌 **How It Works:**  
- The ESP32 detects an **incorrect authentication attempt**.  
- If invalid, the **buzzer beeps twice**, each lasting **200 milliseconds**, with a **200ms pause between beeps**.  
- This provides an **audio alert** that access was **denied**.

🖼 **Visual Representation:**  
![Buzzer Wrong Authentication](https://github.com/Hotsunlok/ESP32-smart-door-system/blob/2acbe38b9b17501bb37a8639c6d5226fe32725f5/%E8%9E%A2%E5%B9%95%E6%93%B7%E5%8F%96%E7%95%AB%E9%9D%A2%202025-01-30%20075153.jpg)

---

### 🛠 **Technical Explanation**
When the user **fails authentication**, ESP32 calls the function:

```cpp
showError("keypad");   // Example: Wrong keypad password
```
Inside `showError()`, the buzzer is triggered as follows:
```cpp
void showError(String method) {
    // Beep twice
    for (int i = 0; i < 2; i++) {
        digitalWrite(BUZZER_PIN, HIGH);  // Turn buzzer ON
        delay(200);                      // Keep it ON for 200ms
        digitalWrite(BUZZER_PIN, LOW);   // Turn buzzer OFF
        delay(200);                      // Pause for 200ms before second beep
    }
}
```
🖼 **Technical Representation:** 
![Buzzer Technical Flowchart](https://github.com/Hotsunlok/ESP32-smart-door-system/blob/82af26a70bb708ca3b9d985a18644fa2ebd1b901/smartdoorsystempics.jpg)
## ⏳ Buzzer Response - Auto-Lock Timer (1 Beep)

### 🔄 **Simple Explanation**
When the **auto-lock timer** reaches **10 seconds**, the **door automatically locks**, and the **buzzer beeps once** to notify the user.

📌 **How It Works:**  
- After the user **unlocks** the door, a **10-second countdown** starts.  
- Once **10 seconds** have passed, the **ESP32 automatically locks** the door.  
- The **buzzer beeps once** to confirm the **auto-lock action**.



---

### 🛠 **Technical Explanation**
When **auto-lock is triggered**, ESP32 calls:

```cpp
controlDoor(true, "auto-lock");   // Example: Auto-lock triggered
```
Inside `controlDoor()`, the buzzer is triggered as follows:
```cpp
void controlDoor(bool lock, String method) {
    if (lock) {
        // Auto-lock case
        digitalWrite(BUZZER_PIN, HIGH);  // Turn buzzer ON
        delay(300);                      // Keep it ON for 300ms
        digitalWrite(BUZZER_PIN, LOW);   // Turn buzzer OFF
    }
}
```
In order to trigger the `controlDoor(true, "auto-lock");`, it needs to fulfill the following code condition 
```cpp
// Auto-lock countdown
if (timerActive) {
    unsigned long currentTime = millis();
    if (currentTime - unlockTime >= countdownDuration) {
        doorLocked = true;
        controlDoor(doorLocked, "auto-lock");  // Trigger auto-lock & buzzer
    }
}
```
