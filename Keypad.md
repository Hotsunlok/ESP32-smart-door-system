# ⌨ Keypad Access  

The **keypad** is one of the four methods to access the **Smart Door System**.  
It allows users to **enter a 4-digit password** to verify **locking** or **unlocking** the door.  

## 🖼️ Keypad Component  

*Upload the image of the keypad here:*  

![Keypad Component](https://github.com/Hotsunlok/ESP32-smart-door-system/blob/457c78315f53fe405cb2db72a8e64f07c065ac6c/assets/keypad.jpg)  

## 🛒 Buy the Keypad  

🔗 [Purchase the Keypad on Amazon](https://www.amazon.co.uk/AZDelivery-Matrix-Membrane-Switch-Keypad/dp/B08B3JR8W9?th=1)  

---
## 🔌 Keypad Pin Configuration  

The **keypad used in this project is a 4x4 keypad**, which consists of **4 row pins** (R1, R2, R3, R4) and **4 column pins** (C1, C2, C3, C4).  
The table below shows how the **8 pins of the keypad are connected to the ESP32**.  

| **Keypad 4x4 Pins** | **ESP32 Pins** |
|--------------------|---------------|
| R1               | GPIO 14       |
| R2               | GPIO 27       |
| R3               | GPIO 26       |
| R4               | GPIO 25       |
| C1               | GPIO 33       |
| C2               | GPIO 32       |
| C3               | GPIO 18       |
| C4               | GPIO 19       |

---

### 📌 Keypad Matrix Setup in Code  

The **ESP32 scans the keypad matrix** using **row and column definitions**:

```cpp
const byte ROWS = 4;  
const byte COLS = 4;  

char keys[ROWS][COLS] = {  
  {'1', '2', '3', 'A'},  
  {'4', '5', '6', 'B'},  
  {'7', '8', '9', 'C'},  
  {'*', '0', '#', 'D'}  
};  

byte rowPins[ROWS] = {14, 27, 26, 25};  // GPIO pins for row connections  
byte colPins[COLS] = {33, 32, 18, 19};  // GPIO pins for column connections  

Keypad keypad = Keypad(makeKeymap(keys), rowPins, colPins, ROWS, COLS);
```
## 🔍 Explanation of the Code:

- Defines **4 rows** and **4 columns** for the **4x4 keypad matrix**.
- Each key **(1, 2, 3, A, etc.)** is mapped to a specific **row-column intersection**.
- The **ESP32 detects key presses** by scanning the intersection of **active row and column**.
- The **`Keypad`** library is used to manage this matrix scanning automatically.
---
# ⌨ Keypad Input Processing  

This section explains **how the keypad captures user input**, **displays the password on the LCD**, and **compares the entered password with the pre-set code (`1234`)** to determine if the door should unlock or remain locked.  

---

## 🔑 Setting the Pre-Set Password  

Before the system starts verifying user input, it first **sets the pre-set password** to **"1234"** using the `Password` library.  

The following code initializes the **default password**:  

```cpp
Password password = Password("1234");  // Set your password
```
### 📌 Explanation:

- The pre-set password is `"1234"`.
- This stored password will be used for **comparison** whenever a user enters input via the keypad.
---
## 🔄 Detecting Keypad Input  

Once the system has a stored password, it **continuously checks for user input** via the keypad.(see the code below)

```cpp
char key = keypad.getKey();  
if (key != NO_KEY) {  
    delay(60);  
    if (key == 'C') {  
        resetPassword();  
    } else {  
        processKey(key);  
    }  
}
```
## 📌 Explanation:

- The system **continuously listens** for a key press using `keypad.getKey()`.
- If **no key is pressed**, the function **skips the next steps**.
- If the user **presses any key**, it moves to the next process.
---
## 🔄 Handling Keypad Input  

- If the user **presses** `C`, the system **clears** the current input and **resets the password**.  
- If the user **presses any other key**, it jumps to the `processKey(key)` function for further steps, including **displaying the password on the LCD** and **verifying it**.  

---

## 📌 Important Notes:  

- The **4x4 keypad does not have the letter 'C'**, but you can assign it as a special function key.  
- Normal key inputs **proceed to password verification**, where the system checks if the entered password **matches** `"1234"`.  
---
## 🔢 Processing Keypad Input  

Once the system **calls** the `processKey(key)` function, it jumps to the following code:  

```cpp
void processKey(char key) {
    static int currentPasswordLength = 0; // Track the current password length

    if (currentPasswordLength == 0) {
        lcd.clear();
        lcd.setCursor(0, 0);
        lcd.print("Enter Password:");
        lcd.setCursor(0, 1); // Move to the second line
    }

    lcd.print(key);  // Display the actual key instead of '*'
    password.append(key);
    currentPasswordLength++; // Increment password length
}
```
## 🔐 Password Verification  

When `currentPasswordLength` reaches **4 digits**, the system **stops accepting more input**.  
The **LCD updates** to `"Thinking..."`, and the system **evaluates the entered password** using the `password.evaluate()` function.  

If the entered password **matches** the pre-set password (`"1234"`), it **calls** the `controlDoor()` function, which:  
- **Updates the LCD** to display **"Locked" / "Unlocked"**.  
- **Changes the servo motor angle** to lock/unlock the door.  

If the password **does not match**, the system **calls** `showError("keypad")`, triggering:  
- **LCD display update** to `"Wrong Access"`.  
- **Buzzer beeps twice** to indicate incorrect input.  

---

## 📟 Code for Password Evaluation  

```cpp
if (currentPasswordLength == 4) {
    lcd.clear();
    lcd.setCursor(0, 0);
    lcd.print("Thinking...");
    delay(1000);

    if (password.evaluate()) {
        doorLocked = !doorLocked; // Toggle door status
        controlDoor(doorLocked, "keypad");
    } else {
        // Incorrect password
        showError("keypad");
    }

    resetPassword();
    currentPasswordLength = 0;
}
```
## 📷 LCD Display Process Flow

for easier explanation of password Evaluation and Verification, it will use the picture below to show the whooloe LCD updates during the keypad authentication process:

![Full LCD display process](https://github.com/Hotsunlok/ESP32-smart-door-system/blob/7a83b11a52cd67dd14948c10687e9617a8d39927/assets/keypadlcd1234.jpg)  

---

# 🔢 FIRST CASE: Correct Keypad Access - Unlocking the Door  

Once the **correct 4-digit password** is entered via the keypad, the system **verifies** the input.  
If the password **matches** the pre-set code (`"1234"`), the following actions occur:  

## 📟 LCD & Physical Response  

- The **LCD updates** to `"Unlocked"`, confirming successful access.  
- The **servo motor rotates to 50°**, pulling the **sliding bolt into the unlocked position**.  
- The **buzzer beeps once** as feedback.  

## 🌐 Web Interface Update  

- The **toggle switch** automatically moves from **red (locked) to green (unlocked)**.  
- The **access log updates** from `"1. The door is locked."` to `"2. The door is unlocked (by keypad)."`  

---

## 📷 Physical Response to Correct Keypad Password (UNLOCKING)  

*Upload the image showing the physical unlocking process via keypad here:*  

![Keypad Unlocking Process](https://github.com/Hotsunlok/ESP32-smart-door-system/blob/9bb0c271f22709f6961a770f23153990ad20814c/assets/keypadunlockingprocess.jpg)  

## 📸 **Web Interface Once Door Is Unlocked By keypad**
![web interface once door is unlocked by keypad](https://github.com/Hotsunlok/ESP32-smart-door-system/blob/3751f2d17c6c4c0995f8a499e9911d61b202aeff/assets/webinterfacekeypad.jpg)

---

# 🔢 SECOND CASE: Correct Keypad Access - Locking the Door  

After the user successfully unlocks the door, they can use the **keypad** to **lock the door again**.  

## 📟 LCD & Physical Response  

- The **LCD updates** to `"Locked"`, confirming that the door is secured.  
- The **servo motor rotates back to 110°**, pushing the **sliding bolt into the locked position**.  
- The **buzzer beeps once** as confirmation.  

## 🌐 Web Interface Update  

- The **toggle switch** automatically moves from **green (unlocked) to red (locked)**.  
- The **access log updates** from `"2. The door is unlocked (by keypad)."` to `"3. The door is locked (by keypad)."`   

---

## 📷 Physical Response to Correct Keypad Password (Locking)  

*Upload the image showing the physical locking process via keypad here:*  

![Keypad Locking Process](https://github.com/Hotsunlok/ESP32-smart-door-system/blob/2eaa9485b30900dd44a8d8b3b8105d2d3c7c1a5e/assets/keypadlockingprocess.jpg)  

## 📸 **Web Interface Once Door Is locked By keypad**
![web interface once door is unlocked by keypad](https://github.com/Hotsunlok/ESP32-smart-door-system/blob/0d164d36a0aa056e9b34758aaa44af6972245cf0/assets/%E5%9C%96%E7%89%8714.jpg)
---
# ⏳ THIRD CASE: Auto-Lock Timer with Keypad  

If the user **unlocks the door with the correct keypad password** but **does not enter any further input** within **10 seconds**, the **auto-lock timer** activates.  

---

## **🟢 Step 1: Unlocking (Keypad)**  

The user enters the **correct 4-digit password** via the keypad, unlocking the door.  
- **LCD displays** `"Unlocked"`.  
- **Toggle switch slides** from **red (left) to green (right)**.  
- **Access log updates** to `"2. The door is unlocked (by keypad)."`.  

📸 **Web Interface - Timer Countdown (Keypad)**  
![Upload Image Here](https://github.com/Hotsunlok/ESP32-smart-door-system/blob/b4a50721a8eee801bf4310a0a3366808f181e9d3/assets/keypadautotimelockflow.jpg)  

---

## **🔴 Step 2: Auto-Lock Activated (No Action for 10s)**  

If the user **does not interact** within **10 seconds**:  
- **LCD updates** to `"Locked"`.  
- **Toggle switch slides back** from **green (right) to red (left)**.  
- **Timer resets to `00:10` on the web interface**.  
- **Access log updates** to `"3. The door is locked (by auto-lock)."`  
- **Buzzer beeps once**.  
- **Servo motor rotates to `110°`**, pushing the **sliding bolt lock into the locked position**.  

📸 **Web Interface - Auto-Lock Timer Triggered (Keypad)**  
![Upload Image Here](https://github.com/Hotsunlok/ESP32-smart-door-system/blob/e5b3f5fb96ef568fd7f852f7fa8ea53158b1e2b8/assets/%E5%9C%96%E7%89%8715.jpg)  

---

## **🔄 Step 3: System Resets for Next User**  

- **LCD loops back** to `"Welcome Password"`, allowing new user input.  
- The system is **ready for unlocking again**.  

---

📸 **Overall Physical System - Auto-Lock Timer (Keypad)**  
![Upload Image Here](https://github.com/Hotsunlok/ESP32-smart-door-system/blob/b0b3f0bdf816540ba046bf7ca5affeb42d4b5286/assets/webkeypadautotimelock.jpg)  

---
# ❌ **CASE 4: Incorrect Password Access**  

If the user **enters an incorrect password via the keypad**, the system **denies access** and the door remains **locked**.  

---

## **🛑 Step 1: Attempting Access with Wrong Password**  

For testing, the **incorrect password entered is `6789`**.  

📸 **Wrong Password Entered (Keypad: 6789)**  
![Upload Image Here](https://github.com/Hotsunlok/ESP32-smart-door-system/blob/66bc3459f7a915d81d3a0deba3d0e0c6d26e1d9a/assets/keypad6789.jpg)  

- **LCD displays** `"Thinking..."` while verifying.  
- **LCD updates** to `"Wrong Access"`.  
- **Buzzer beeps twice** to indicate failed access.  
- **Servo motor remains unchanged** (door stays locked).  
- **Toggle switch remains in red (left)**.  
- **Access log updates** to `"4. Wrong access (by keypad)."`  

---

## **🌐 Step 2: Web Interface Response**  

- **Toggle switch does not move** (remains **red/locked**).  
- **Access log displays** `"4. Wrong access (by keypad)."`  

📸 **Web Interface - Wrong Access (Keypad)**  
![Upload Image Here](UPLOAD_IMAGE_LINK_HERE)  

---

## **🔄 Step 3: System Resets for Next User**  

- **LCD loops back** to `"Welcome Password"`, allowing the next attempt.  
- The system **remains in a locked state** until the correct password is entered.  

---

📸 **Overall Physical System - Wrong Password Entry**  
![Upload Image Here](UPLOAD_IMAGE_LINK_HERE)  

