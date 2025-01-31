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
![Green Toggle Switch Unlock](UPLOAD_YOUR_IMAGE_HERE)
