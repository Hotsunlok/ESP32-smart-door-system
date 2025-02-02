# 📟 LCD Display (1602 I2C)  

The **LCD 1602 I2C** is one of the **output components** in the smart door system.  
It provides **visual feedback** after the user enters a password via any of the available unlocking methods:  

✔ **Keypad**  
✔ **RFID Module**  
✔ **Fingerprint Sensor**  
✔ **Face ID**  

---

## 🖼️ LCD 1602 I2C Component Image  

  
![LCD Display Component](https://github.com/Hotsunlok/ESP32-smart-door-system/blob/3d22cdc96ebd30b8fb8d97d718856a8784a3d90a/assets/71xFXYb%2BR%2BL._AC_SY450_.jpg)  

---

## 🛒 Purchase Link  

💰 *Buy the LCD 1602 I2C module on Amazon:*  
[**Purchase Here**](https://www.amazon.co.uk/FREENOVE-Display-Compatible-Arduino-Raspberry/dp/B0B76YGDV4/ref=sr_1_1_sspa?dib=eyJ2IjoiMSJ9.QoGAUGPGEFkc_48vAB2syB8lX3auIPu4gbHn6Ox4bhSbBAc_bbtyFWHGE3GGP8X9b6idfPMwa77bAzuqXOLfm2GmmWFzECrtvGTO7E7Dr2lvsYrsztShr9gAtBuVcXzwzbXJmEDvPPhGTIGR4y5Ows-UL8sBpxOQP5Bg-yjs3mXyN9mL3jAd7wLGTQ9f2BJ-3jT8kSUguGpo4piLQ2cdKzuDRjeM3mTc89DgaEvIEdg.D93OglP6R4mDEyuvjfWpB3GvyulmrTaAhwWnsliXhT8&dib_tag=se&keywords=lcd%2B1602&qid=1738486369&sr=8-1-spons&sp_csd=d2lkZ2V0TmFtZT1zcF9hdGY&th=1)  

---

## 🔌 LCD 1602 I2C Pin Configuration  

The **LCD 1602 I2C** uses **I2C communication**, requiring only **4 pins** for operation:  

✔ **SDA** – Serial Data Line  
✔ **SCL** – Serial Clock Line  
✔ **VCC** – Power Supply (5V)  
✔ **GND** – Ground  

---

### 📊 LCD 1602 I2C Pins Connected to ESP32  

| **LCD 1602 I2C Pins** | **ESP32 Pins** |
|----------------------|--------------|
| **SDA** | GPIO 21 |
| **SCL** | GPIO 22 |
| **VCC** | Vin |
| **GND** | GND |

---  

# 📟 LCD Display: "Locked" or "Unlocked"  

The **LCD always starts by displaying** `"Welcome Password"`.  
At the end of any unlocking process, the **LCD updates to display** `"Locked"` or `"Unlocked"` depending on the **lock variable** from the `controlDoor(bool lock, String method)` function.  

### **🔄 How the LCD Decides "Locked" or "Unlocked"**  

- No matter which method is used (**RFID, Keypad, Fingerprint, Face ID**), the system **calls**:  
  ```cpp
  controlDoor(doorLocked, "MethodName");
  ```
- The function **updates** `doorLocked`:

  - Example: If `doorLocked` changes from **true (locked) → false (unlocked)**, then `lock = false`.

- The `lock` variable is then **used to update the LCD**:

```cpp
lcd.print(lock ? "Locked" : "Unlocked");
```
- `"Lock= false"`, LCD display "Unlocked".
- `"Lock= true"`, LCD display "Locked".
---
# **📜 Code Section for LCD Update**  

```cpp
void controlDoor(bool lock, String method) {
    // Beep once
    digitalWrite(BUZZER_PIN, HIGH);
    delay(300);
    digitalWrite(BUZZER_PIN, LOW);

    servo.write(lock ? 110 : 50); // 50 = open, 110 = locked

    lcd.clear();
    lcd.setCursor(0, 0);
    lcd.print(lock ? "Locked" : "Unlocked");
}
 ```
# 📷 Two Pathways of LCD Display
![Two pathway of LCD display flowchart](https://github.com/Hotsunlok/ESP32-smart-door-system/blob/d557a7eb30742a3f3f11188b1977dfbb06495a51/assets/LCDFLOWCHART.jpg)  
