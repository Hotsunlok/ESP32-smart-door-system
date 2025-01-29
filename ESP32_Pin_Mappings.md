# 🛠️ ESP32 Component Pin Mappings

This file contains the **complete wiring details** for the **ESP32-based smart door system**.  
It includes:  
- 📷 **Full Breadboard Circuit Image**  
- 📌 **ESP32 Pin Mapping Tables** for all components  

---

## 📸 Full Circuit Diagram (Breadboard Simulation)
![Breadboard Circuit](https://your-image-link-here)

---

## 📌 ESP32 Pin Mapping Tables

### 🔄 Servo Motor
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
| SDA       | GPIO 5   |
| SCK       | GPIO 18  |
| MOSI      | GPIO 23  |
| MISO      | GPIO 19  |
| IRQ       | N/A (PLEASE DO NOT CONNECT)     |
| GND       | GND      |
| RST       | GPIO 4   |

### 🔊 Buzzer
| Buzzer Pin | ESP32 Pin |
|------------|----------|
| VCC        | 5V       |
| GND        | GND      |
| Signal     | GPIO 23  |

### 🛡️ Fingerprint Sensor
| Fingerprint Pin | ESP32 Pin |
|----------------|----------|
| TX            | GPIO 16  |
| RX            | GPIO 17  |
| VCC           | 3.3V     |
| GND           | GND      |

---


