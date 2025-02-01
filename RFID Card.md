# 📡 **RC522 RFID Module**

The **RC522 RFID Module** is one of the **four methods** to access the **Smart Door System**.  
It detects the **user's RFID card** and checks if the **card ID matches an authorized ID** to **unlock or lock** the door.

---

## 📸 **Component Image**  
![Upload Image Here](https://github.com/Hotsunlok/ESP32-smart-door-system/blob/0f91b7d94d00498f622182e936073ebd2f8c9e20/assets/%E5%9C%96%E7%89%874.jpg)

---

## 🛒 **Purchase Link**  
[Upload Amazon Purchase Link Here](https://www.amazon.co.uk/AZDelivery-RFID-RC522-Reader-Parent/dp/B01M28JAAZ?th=1)

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
