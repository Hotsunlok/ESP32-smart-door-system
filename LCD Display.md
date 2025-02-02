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
---
## 📷 Two Pathways of LCD Display
![Two pathway of LCD display flowchart](https://github.com/Hotsunlok/ESP32-smart-door-system/blob/d557a7eb30742a3f3f11188b1977dfbb06495a51/assets/LCDFLOWCHART.jpg)  

---
# 🔄 LCD Display Behavior Across Unlocking Methods  

Since we have already discussed how **Fingerprint, Keypad, RFID, and Face ID** trigger the LCD updates, you can read the detailed behavior in their respective sections:  

- 🔢 [Keypad](Keypad.md)  
- 📡 [RFID Card System RC522](RFID%20Card.md)  
- 🏷️ [Fingerprint Sensor R307](Fingerprint%20Sensor.md)  
- 🎛️ [Web Server Toggle Button (Manual)](Web%20Server%20Toggle%20Button%20%28Manual%29.md)   
- 🤖 [Face Recognition System](docs/Face_Recognition.md)  

---

### 📌 **Keypad's Unique LCD Behavior**  

Unlike **RFID, Fingerprint, and Face ID**, which **immediately** display `"Thinking..."` after detection:  
✔ **Keypad first updates the LCD** to `"Enter Password"` before allowing users to type their **4-digit password**.  
✔ Once the user enters all **4 digits**, the LCD **then updates** to `"Thinking..."`.  
✔ After that, **all methods follow the same process**, leading to:  
   - `"Unlocked"` if access is granted.  
   - `"Locked"` if access is denied or auto-lock activates.  
✔ The LCD **loops back to "Welcome Password"** in the end.  

---
# 📷 whole map of LCD feedback flow for correct access
![whole map of LCD feedback flow for correct access](https://github.com/Hotsunlok/ESP32-smart-door-system/blob/c5e9da32fbd83962cffe2ea3538faa05df3437dd/assets/LCDCORRECTACCESSFLOW.jpg)  

---
# ⏳ LCD: Auto-Lock Timer  

If the user **does not take any action** to enter a password and lock the door within **10 seconds**, the **LCD automatically updates to** `"Locked"`.

### 📌 **How It Works:**  
✔ The **LCD reads** `lock = true`, determining that it must display `"Locked"` after **10 seconds of inactivity**.  
✔ This **ensures users visually know** that the **door is now locked**.  
✔ The LCD then **loops back** to its initial mode, displaying **"Welcome Password"**, completing the **auto-lock process**.

---

## 📷 LCD Feedback Flow - Auto-Lock Timer  
  
![LCD Auto-Lock Timer Display](https://github.com/Hotsunlok/ESP32-smart-door-system/blob/2b2ccc6899fa8e7f8ccdf2104e38277edc1bc6e5/assets/LCDAUTOLOCKFLOW.jpg)  

---

# ❌ LCD: Incorrect Access Feedback  

When the user enters the **wrong password** using **any of the three methods** (**Keypad, RFID, or Fingerprint**), the system calls the `showError()` function.  
This function **triggers the LCD to display** `"Wrong Access"`, alerting the user of failed authentication.

---

### 📌 **Method-Specific Incorrect Access Handling**  

The table below shows how different methods call the `showError()` function:

| **Method**      | **showError() Function Call**    |
|---------------|--------------------------------|
| **Keypad**     | `showError("keypad")`        |
| **RFID**       | `showError("RFID")`          |
| **Fingerprint**| `showError("fingerprint")`   |

---

## 🛑 **showError() Function - LCD Response**  

When an incorrect password is detected, the `showError()` function:  
✔ **Beeps the buzzer twice** (as an alert).  
✔ **Clears the LCD screen**.  
✔ **Displays** `"Wrong Access"`.  

```cpp
void showError(String method) {
    // Beep twice
    for (int i = 0; i < 2; i++) {
        digitalWrite(BUZZER_PIN, HIGH);
        delay(200);
        digitalWrite(BUZZER_PIN, LOW);
        delay(200);
    }
```
## 📷 LCD Feedback Flow - Incorrect Access 
  
![LCD Auto-Lock Timer Display](https://github.com/Hotsunlok/ESP32-smart-door-system/blob/a9b164685a0ed9fa315a1bdd8ccdd5d6543cbfe3/assets/LCDWRONGACCESSFLOW.jpg)

---
# 🌐 LCD: Web Interface Display  

The LCD reflects the **expected display response** when the **user controls the door via the website toggle switch**.  
On the website, the **toggle button** allows users to **slide left (lock) or right (unlock)**, sending commands to the ESP32.

---

## ✅ **Case (1): User Unlocks via Web Interface**  

When the user **slides the toggle button to the right**, the following occurs:

1. **Toggle button turns green** (unlocked state).  
2. **Message `"unlock"` is sent to ESP32**.  
3. The ESP32 **calls the function**:
   ```cpp
   controlDoor(doorLocked, "website");
   ```
### 4️⃣ This function **updates** the `doorLocked` variable:

- `doorLocked = true → false` (Locked → Unlocked).  
- `lock = false`, storing the updated status.  

### 5️⃣ The LCD **reads** `lock = false` and displays:

```cpp
lcd.print("Unlocked");
```
### 6️⃣ Final LCD Display: ` Unlocked ` 
---
## 📷 Code Flow: Toggle Switch → LCD Displays "Unlocked"
![Toggle Switch → LCD Displays "Unlocked"](https://github.com/Hotsunlok/ESP32-smart-door-system/blob/125f47a0a70d90f17ac7ea504e07ffb8ddfa81d8/assets/LCDtoggleswitchUnlock.jpg)

---
## ✅ **Case (2): User locks via Web Interface**  

When the user **slides the toggle button to the left**, the following occurs:

1. **Toggle button turns red** (locked state).  
2. **Message `"lock"` is sent to ESP32**.  
3. The ESP32 **calls the function**:
   ```cpp
   controlDoor(doorLocked, "website");
   ```
### 4️⃣ This function **updates** the `doorLocked` variable:

- `doorLocked = false → true` (Unlocked → locked).  
- `lock = true`, storing the updated status.  

### 5️⃣ The LCD **reads** `lock = true` and displays:

```cpp
lcd.print("Locked");
```
### 6️⃣ Final LCD Display: ` Locked ` 
---
## 📷 Code Flow: Toggle Switch → LCD Displays "Locked"
![Toggle Switch → LCD Displays "Locked"](https://github.com/Hotsunlok/ESP32-smart-door-system/blob/125f47a0a70d90f17ac7ea504e07ffb8ddfa81d8/assets/LCDtoggleswitchUnlock.jpg)
