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
![Buzzer Confirmation](docs/images/buzzer_correct_simple.png)  

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
---
![Buzzer Technical Flowchart](docs/images/buzzer_technical_flowchart.png)
