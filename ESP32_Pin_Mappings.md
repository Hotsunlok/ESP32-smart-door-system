# 🛠️ ESP32 Component Pin Mappings

This file contains the **complete wiring details** for the **ESP32-based smart door system**.  
It includes:  
- 📷 **Full Breadboard Circuit Image**  
- 📌 **ESP32 Pin Mapping Tables** for all components  

---

## 📸 Full Circuit Diagram (Breadboard Simulation)
![Breadboard Circuit](https://github.com/Hotsunlok/ESP32-smart-door-system/blob/5980bb01afa6fdc7061bd8b202ed9d2c758aaa80/Final%20Project%20-2%20(2).jpg)

---

## 📌 ESP32 Pin Mapping Tables

### 🔄 Servo Motor SG90
| Servo Pin | ESP32 Pin |
|-----------|----------|
| VCC       | 5V       |
| GND       | GND      |
| Signal    | GPIO 13  |

### 🖥️ LCD 1602 I2C
| LCD Pin | ESP32 Pin |
|---------|----------|
| VCC     | 5V       |
| GND     | GND      |
| SDA     | GPIO 21  |
| SCL     | GPIO 22  |

### 🔢 Keypad
| Keypad Row/Col | ESP32 Pin |
|---------------|----------|
| R1           | GPIO 14  |
| R2           | GPIO 27  |
| R3           | GPIO 26  |
| R4           | GPIO 25  |
| C1           | GPIO 33  |
| C2           | GPIO 32  |
| C3           | GPIO 18  |
| C4           | GPIO 19  |

### 📡 RFID/NFC Module (RC522)
| RC522 Pin | ESP32 Pin |
|-----------|----------|
| SDA       | GPIO 4   |
| SCK       | GPIO 15 (in series with 10K pull-up resistors) |
| MOSI      | GPIO 2  (in series with 10K pull-downn resistors)|
| MISO      | GPIO 35  |
| IRQ       | N/A (PLEASE DO NOT CONNECT)     |
| 3.3V      | 3.3V      |
| GND       | GND      |
| RST       | GPIO 34   |

### 🔊 Buzzer
| Buzzer Pin | ESP32 Pin |
|------------|----------|
| GND        | GND      |
| Signal     | GPIO 23  |

### 🛡️ Fingerprint Sensor
| Fingerprint Pin | ESP32 Pin |
|----------------|----------|
| TX            | GPIO 16 (RX2) |
| RX            | GPIO 17 (TX2) |
| VCC           | 5V     |
| GND           | GND      |

---


