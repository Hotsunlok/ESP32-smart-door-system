# 🌍 Web Server Toggle Button (Manual)

Before reading this section, it is assumed that you have already read the **[Web Server Overview](Web%20Server%20Overview.md)**, especially the **three cases** of the **toggle switch button behavior**. 

In this section, we will specifically explain **Case 1** and **Case 2** in detail.

---

## 🟢 **Case 1: Sliding from Locked (Red) to Unlocked (Green)**  

When the user **slides the toggle switch to the right (green)**:  

📌 **What Happens?**  
✔ **Servo Motor Moves**: Rotates **50 degrees** to pull the door lock and **unlock** it.  
✔ **Buzzer Feedback**: Beeps **once** to confirm the unlock action.  
✔ **LCD Display Updates**: Changes from `"Welcome Password"` to `"Unlocked"`.  
✔ **Access Log Box Updates**: Displays **"2. The door is unlocked (by website)."**  

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

// Inside controlDoor(), the following actions are triggered:

void controlDoor(bool lock, String method) {
    // Move servo motor to unlock position
    servo.write(50); // 50° unlock position

    // Update LCD display
    lcd.clear();
    lcd.setCursor(0, 0);
    lcd.print("Unlocked");

    // Buzzer feedback
    digitalWrite(BUZZER_PIN, HIGH);
    delay(300);
    digitalWrite(BUZZER_PIN, LOW);

    // Log event in access log
    Serial.println("2. The door is unlocked (by website).");
}
```
### 🖼 Physical System of Toggle Switch Unlocking (Green)  
![Green Toggle Switch Unlock](https://github.com/Hotsunlok/ESP32-smart-door-system/blob/47aa88fa813dff734f4740a02ddb9a84ae1a469c/grenlockphysical.jpg)
---
## 🔴 **Case 2: Sliding from Unlocked (Green) to Locked (Red)**  

When the user **slides the toggle switch to the left (red)**:

### 🔧 **What Happens?**  
✔ **Servo Motor Moves**: Rotates **110 degrees** to push the door lock and **lock** it.  
✔ **Buzzer Feedback**: Beeps **once** to confirm the lock action.  
✔ **LCD Display Updates**: Changes from `"Welcome Password"` to `"Locked"`.  
✔ **Access Log Box Updates**: Displays **"3. The door is locked (by website)."**  

---

### 🛠 **Code Execution for Locking**  
When the **lock command** is received, the ESP32 updates the `doorLocked` variable and calls `controlDoor()` to activate all output components.  

```cpp
else if (message == "lock") {  
    if (!doorLocked) {  
        doorLocked = true;  
        controlDoor(doorLocked, "website");  
    }  
}

void controlDoor(bool lock, String method) {
    if (lock) {
        // Move servo motor to lock the door
        servo.write(110);  

        // Update LCD display
        lcd.clear();
        lcd.setCursor(0, 0);
        lcd.print("Door Locked");

        // Buzzer feedback - Beep once
        digitalWrite(BUZZER_PIN, HIGH);
        delay(300);
        digitalWrite(BUZZER_PIN, LOW);

        // Send real-time update to access log
        ws.textAll("3. The door is locked (by " + method + ").");
    }
}
```
### 🖼 Physical System of Toggle Switch Unlocking (Red)  
![Red Toggle Switch Unlock](https://github.com/Hotsunlok/ESP32-smart-door-system/blob/b9c9badf4eea30a0e114956239290f16a34299ba/redswitchphyscial.jpg)
---
## ⏳ **Case 3: Auto-Lock Timer**  

If the user **slides the toggle switch to the right (green)** to **unlock the door** but does **not** slide it back to the left (red) to lock the door within **10 seconds**, the system will:

### 🔧 **What Happens?**  
✔ **Web Timer Countdown**: Displays countdown from **00:10 to 00:00**.  
✔ **Servo Motor Moves**: Rotates **110 degrees** to push the door lock and **lock** it.  
✔ **Buzzer Feedback**: Beeps **once** to confirm the auto-lock action.  
✔ **LCD Display Updates**: Changes from `"Unlocked"` to `"Locked"`, then back to `"Welcome Password"`.  
✔ **Access Log Box Updates**: Displays **"5. The door is locked (by auto-lock)."**  

---

### 🛠 **Code Execution for Auto-Locking**  
When the **timer reaches 00:00**, the ESP32 **automatically** locks the door and updates all output components.  

```cpp
void autoLockTimer() {
    delay(10000);  // 10-second countdown
    if (!doorLocked) {  
        doorLocked = true;  
        controlDoor(doorLocked, "auto-lock");  
    }  
}

void controlDoor(bool lock, String method) {
    if (lock) {
        // Move servo motor to lock the door
        servo.write(110);  

        // Update LCD display
        lcd.clear();
        lcd.setCursor(0, 0);
        lcd.print("Locked");
        delay(2000);
        lcd.clear();
        lcd.setCursor(0, 0);
        lcd.print("Welcome Password");

        // Buzzer feedback - Beep once
        digitalWrite(BUZZER_PIN, HIGH);
        delay(300);
        digitalWrite(BUZZER_PIN, LOW);

        // Send real-time update to access log
        ws.textAll("5. The door is locked (by " + method + ").");
    }
}
```
### 🖼 Physical System of Auto-Lock Timer  
![Auto-Lock Timer](https://github.com/Hotsunlok/ESP32-smart-door-system/blob/b9c9badf4eea30a0e114956239290f16a34299ba/redswitchphyscial.jpg)
---
