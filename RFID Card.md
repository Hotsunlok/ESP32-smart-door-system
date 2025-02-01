# 📡 **RC522 RFID Module**

The **RC522 RFID Module** is one of the **four methods** to access the **Smart Door System**.  
It detects the **user's RFID card** and checks if the **card ID matches an authorized ID** to **unlock or lock** the door.

---

## 📸 **Component Image**  
![Upload Image Here](https://github.com/Hotsunlok/ESP32-smart-door-system/blob/0f91b7d94d00498f622182e936073ebd2f8c9e20/assets/%E5%9C%96%E7%89%874.jpg)

---

## 🛒 **Purchase Link**  
[ Amazon Purchase Link Here](https://www.amazon.co.uk/AZDelivery-RFID-RC522-Reader-Parent/dp/B01M28JAAZ?th=1)

---
## 🔌 **Pin Configuration Table**
| **RC522 Pin** | **ESP32 Pin** |
|--------------|--------------|
| **SDA**      | GPIO 4       |
| **SCK**      | GPIO 15 (in series with 10K pull-up resistors) |
| **MOSI**     | GPIO 2 (in series with 10K pull-down resistors) |
| **MISO**     | GPIO 35      |
| **IRQ**      | N/A (**DO NOT CONNECT**) |
| **3.3V**     | 3.3V         |
| **GND**      | GND          |
| **RST**      | GPIO 34      |

---
# 🆔 **Pre-Authorized ID Enrollment (RFID)**

Before using the **RC522 RFID Module**, you must **enroll a pre-authorized card**.  
This process ensures the system can **verify the correct card ID** for unlocking or locking the door.

---

## 📖 **How to Get the Pre-Authorized ID**
If you **haven't enrolled a card yet**, follow this guide to retrieve your **RFID card ID** from the **Serial Monitor**  
and copy the ID into the code.

📌 **Reference Guide:**
[Enrollment Guide](https://srituhobby.com/how-to-make-an-rfid-door-lock-system-using-an-arduino-nano-board/)

---

## ⚠️ **IMPORTANT: Enrollment is REQUIRED!**
🚨 **WARNING: The system will NOT work unless you enroll a pre-authorized ID first!**  
Before testing, **ensure that your RFID card is successfully registered**.  

✅ **If enrollment is not completed**, the system **CANNOT verify any card**, and access will be denied.  

⚠️ SO PLEASE WATCH THE ABOVE LINK to finish enrollment step first !!!
---

# 🆔 **Card Enrollment - Pre-Authorized ID (RFID)**

The **pre-authorized ID** is set to **4 bytes**, with each byte containing a specific number.  
This **unique ID** ensures that **only the enrolled card** can unlock or lock the door.

⚠️ THIS IS MY PRE-AUTHORIZED ID , PLEASE DO NOT COPY!!!

---

## 📖 **Pre-Authorized ID Table**
| **Byte**  | **Value (Hex)** |
|-----------|----------------|
| **Byte 0** | `0x33` |
| **Byte 1** | `0x82` |
| **Byte 2** | `0xDA` |
| **Byte 3** | `0x11` |

---

📸 **Correct RFID Card for Unlocking/Locking**

![Upload Image Here](https://github.com/Hotsunlok/ESP32-smart-door-system/blob/e7a01c0bf9843400c4d1428af1b82f09cd09f059/assets/whitecard.jpg)
---

# 📡 **RFID Initialization & Card Detection Process**

The **ESP32 initializes the RC522 RFID Module** by specifying the **SPI communication pins** and  
ensuring the **RFID reader is ready** to detect and process cards.

---

## 🔄 **Step 1: RFID Initialization**  
The following code **sets up the RFID module** by defining the correct **ESP32 pins**:  

```cpp
// Set up RFID
SPI.begin(15, 35, 2, 4); // SCK = GPIO15, MISO = GPIO35, MOSI = GPIO2, SS = GPIO4
rfid.PCD_Init(); // Initialize the RC522 module
```
## ✅ Explanation:

- ✔️ **`SPI.begin(...)`** → Defines **SCK, MISO, MOSI, and SS** pins for the ESP32 to communicate with the RFID module.
- ✔️ **`rfid.PCD_Init();`** → Ensures the RFID reader is **ready to detect and process cards**.
# 🆔 Step 2: Detecting RFID Card Presence

After initialization, the system **checks for a new card** before reading its data.

```cpp
void checkRFID() {
    if (!rfid.PICC_IsNewCardPresent()) {
        return; // No card detected, exit function
    }

    if (!rfid.PICC_ReadCardSerial()) {
        return; // Card detected but not read, exit function
    }
}
```
## 📌 How It Works?

### 🔷 `rfid.PICC_IsNewCardPresent()` → **Checks if a new card is present.**
- ✅ **If `false`** (card detected): The function proceeds.
- ❌ **If `true`** (no card detected): The function returns and does nothing.

### 🔷 `rfid.PICC_ReadCardSerial()` → **Reads the card’s unique ID.**
- ✅ **If `false`** (card read successfully): The function moves to the next step.
- ❌ **If `true`** (card not read): The function returns without further processing.
---
## 🔐 RFID Card Verification

After detecting a new RFID card, the system **compares** the scanned card ID with the **pre-authorized** ID stored in memory.

### 📌 Card Verification Process
The RFID reader **analyzes the card's unique ID** by checking its **4-byte values**:

```cpp
// Check if the card is the correct one
if (rfid.uid.uidByte[0] == 0x33 && rfid.uid.uidByte[1] == 0x82 && 
    rfid.uid.uidByte[2] == 0xDA && rfid.uid.uidByte[3] == 0x11) {
    
    // Correct card
    doorLocked = !doorLocked; // Toggle door status
    controlDoor(doorLocked, "RFID");
} else {
    // Incorrect card
    showError("RFID");
}
```
### ✅ Successful Match (Authorized Card)
- The **card ID matches** the pre-authorized ID.
- Calls `controlDoor(doorLocked, "RFID")`, which:
  - Updates the LCD to display **"Locked"** / **"Unlocked"**.
  - **Servo motor** rotates to adjust the lock position.
  - **Buzzer beeps once** to confirm access.

### ❌ Unsuccessful Match (Unauthorized Card)
- The **card ID does not match** the stored ID.
- Calls `showError("RFID")`, which:
  - Updates the LCD to display **"Wrong Access"**.
  - **Buzzer beeps twice** to indicate access denial.
---
## 📸 **RFID Sensor Process Flow**  
![Upload Image Here](https://github.com/Hotsunlok/ESP32-smart-door-system/blob/319e93fcd72172a0480d0ba39cbf7445e095b930/assets/RFIDFLOW.jpg)

---
## 🔐 FIRST CASE: Correct RFID Access - Unlocking the Door  

Once the code is updated, the **LCD** will display `"Welcome Password"`, ensuring the system starts in a locked state.  
At this point, the **servo motor** immediately rotates to **110°**, pushing the **sliding bolt lock into the locked position**.  

When the user **taps the correct RFID card** (e.g., pre-authorized white card) on the **RFID sensor**, the **RFID reader detects and verifies it**.  
During verification, the **LCD updates** to `"Thinking..."` while processing.  

If the **RFID card matches** the pre-authorized ID, the **LCD updates** to `"Unlocked"`, and the **buzzer beeps once** as feedback.  
Simultaneously, the **servo motor rotates to 50°**, pulling the **sliding bolt into the unlocked position**.  

On the **web interface**, the **toggle switch** automatically moves from **red (locked) to green (unlocked)**.  
The **access log** updates from `"1. The door is locked."` to **"2. The door is unlocked (by RFID)."**

---


## 📸 **Physical Response to Correct RFID CARD (Unlocking)**
![Correct Card Unlocking Process](https://github.com/Hotsunlok/ESP32-smart-door-system/blob/a824ed96a32631e6cd3bd1095002b6f6af437301/rfidunlock.jpg)

## 📸 **Web Interface Once Door Is Unlocked By Correct RFID CARD**
![web interface once door is unlocked by Correct RFID Card](https://github.com/Hotsunlok/ESP32-smart-door-system/blob/401d8d9a35c0648f07a33bc2fd6a1c4908694f3f/assets/webRFIDUNLOCK.jpg)

---

## 🔐 SECOND CASE: Correct RFID Access - Locking the Door  

After the user successfully unlocks the door, they can use their **RFID card** to **lock the door again**.  

When the **correct RFID card** is tapped on the **RFID sensor**, the **LCD updates** to `"Locked"`, as shown below.  
Immediately, the **servo motor rotates back to 110°**, pushing the **sliding bolt into the locked position**.  

The **buzzer beeps once** as confirmation.  

On the **web interface**, the **toggle switch** automatically moves from **green (unlocked) to red (locked)**.  
The **access log** updates from `"2. The door is unlocked (by RFID)."` to `"3. The door is locked (by RFID)."`  

Finally, the **LCD loops back** to `"Welcome Password"`, allowing the user to **enter their access method again**.  

---

## 📸 **Physical Response to Correct RFID CARD (Locking)**
![Correct Card Locking Process](https://github.com/Hotsunlok/ESP32-smart-door-system/blob/1ba0efb921396740fb1ff246696d271c78624111/assets/rfidlock.jpg)

## 📸 **Web Interface Once Door Is locked By Correct RFID CARD**
![web interface once door is locked by Correct RFID Card](https://github.com/Hotsunlok/ESP32-smart-door-system/blob/9d8b723472cbda1b3dcb99c4d1badbcdff9d4754/assets/webrfidlock.jpg)
