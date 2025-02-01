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
