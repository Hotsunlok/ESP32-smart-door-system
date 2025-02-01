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

![Full LCD display process](https://github.com/Hotsunlok/ESP32-smart-door-system/blob/56790733250a6e5f43e3dc5d8b46a60331fbae0f/assets/keypad.jpg)  

