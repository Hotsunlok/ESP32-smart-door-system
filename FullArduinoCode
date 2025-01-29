#define SS_PIN 4
#define RST_PIN 34
#define BUZZER_PIN 23

#include <WiFi.h>
#include <ESPAsyncWebServer.h>
#include <AsyncTCP.h>
#include <ESP32Servo.h>
#include <LiquidCrystal_I2C.h>
#include <Adafruit_Fingerprint.h>
#include <Keypad.h>
#include <Password.h>
#include <SPI.h>
#include <MFRC522.h>
#include "HardwareSerial.h"

Servo servo;
AsyncWebServer server(80);
AsyncWebSocket ws("/ws");

LiquidCrystal_I2C lcd(0x27, 16, 2);

// Fingerprint sensor on hardware Serial1 (TX=17, RX=16)
HardwareSerial mySerial(1);
Adafruit_Fingerprint finger = Adafruit_Fingerprint(&mySerial);

// Keypad
Password password = Password("1234"); 
MFRC522 rfid(SS_PIN, RST_PIN);

const byte ROWS = 4;
const byte COLS = 4;
char keys[ROWS][COLS] = {
  {'1', '2', '3', 'A'},
  {'4', '5', '6', 'B'},
  {'7', '8', '9', 'C'},
  {'*', '0', '#', 'D'},
};
byte rowPins[ROWS] = {14, 27, 26, 25}; 
byte colPins[COLS] = {33, 32, 18, 19};
Keypad keypad = Keypad(makeKeymap(keys), rowPins, colPins, ROWS, COLS);

// Track door state
bool doorLocked = true;  
String accessLog = "<p>1. The door is locked.</p>";
int eventCounter = 1;
bool darkMode = false;  

// Auto-lock countdown
unsigned long unlockTime = 0; 
const unsigned long countdownDuration = 20000; // 20s
bool timerActive = false;

void setup() {
  Serial.begin(115200);

  // AP mode
  const char* ssid = "ESP32_AP";
  const char* password = "12345678";
  WiFi.softAP(ssid, password);

  Serial.print("AP IP address: ");
  Serial.println(WiFi.softAPIP());

  // Servo
  servo.attach(13);
  servo.write(110); // locked

  // LCD
  lcd.init();
  lcd.backlight();
  displayWelcomeMessage();

  // Buzzer
  pinMode(BUZZER_PIN, OUTPUT);
  digitalWrite(BUZZER_PIN, LOW);

  // Fingerprint
  mySerial.begin(57600, SERIAL_8N1, 16, 17);
  if (finger.verifyPassword()) {
    Serial.println("Found fingerprint sensor!");
  } else {
    Serial.println("Did not find fingerprint sensor :(");
    while(1) { delay(1); }
  }

  // RFID
  SPI.begin(15, 35, 2, 4);
  rfid.PCD_Init();

  // Website root
  server.on("/", HTTP_GET, handleRoot);

  // ---- Remove old /face_unlock & /face_lock if it exists ----
  /*
  server.on("/face_unlock", HTTP_POST, [](AsyncWebServerRequest *request) { ... });
  server.on("/face_lock", HTTP_POST, [](AsyncWebServerRequest *request) { ... });
  */

  // ---- Add single /face_toggle endpoint ----
  server.on("/face_toggle", HTTP_POST, [](AsyncWebServerRequest *request) {
    doorLocked = !doorLocked; 
    controlDoor(doorLocked, "FACE ID");
    String responseText = doorLocked ? "Face Lock Successful" : "Face Unlock Successful";
    request->send(200, "text/plain", responseText);
  });

  // WebSocket
  ws.onEvent(onWebSocketEvent);
  server.addHandler(&ws);

  // Start server
  server.begin();
}

void loop() {
  // Keypad
  char key = keypad.getKey();
  if (key != NO_KEY) {
    delay(60);
    if (key == 'C') {
      resetPassword();
    } else {
      processKey(key);
    }
  }

  // Fingerprint
  getFingerprintID();

  // RFID
  checkRFID();

  // Auto-lock countdown
  if (timerActive) {
    unsigned long currentTime = millis();
    if (currentTime - unlockTime >= countdownDuration) {
      doorLocked = true;
      controlDoor(doorLocked, "auto-lock");
    }
  }

  delay(50);
}

// ------------------- Keypad -------------------
void processKey(char key) {
  static int currentPasswordLength = 0;

  if (currentPasswordLength == 0) {
    lcd.clear();
    lcd.setCursor(0, 0);
    lcd.print("Enter Password:");
    lcd.setCursor(0, 1);
  }

  lcd.print(key);  
  password.append(key);
  currentPasswordLength++;

  if (currentPasswordLength == 4) {
    lcd.clear();
    lcd.setCursor(0, 0);
    lcd.print("Thinking...");
    delay(1000);

    if (password.evaluate()) {
      doorLocked = !doorLocked; 
      controlDoor(doorLocked, "keypad");
    } else {
      showError("keypad");
    }
    resetPassword();
    currentPasswordLength = 0;
  }
}

// ------------------- Fingerprint -------------------
void getFingerprintID() {
  uint8_t p = finger.getImage();
  if (p != FINGERPRINT_OK) return;

  lcd.clear();
  lcd.setCursor(0, 0);
  lcd.print("Thinking...");
  delay(1000);

  p = finger.image2Tz();
  if (p != FINGERPRINT_OK) {
    showError("fingerprint");
    return;
  }

  p = finger.fingerSearch();
  if (p == FINGERPRINT_OK) {
    if (finger.fingerID == 1) {
      doorLocked = !doorLocked;
      controlDoor(doorLocked, "fingerprint");
    } else {
      showError("fingerprint");
    }
  } else {
    showError("fingerprint");
  }
}

// ------------------- RFID -------------------
void checkRFID() {
  if (!rfid.PICC_IsNewCardPresent()) return;
  if (!rfid.PICC_ReadCardSerial())   return;

  lcd.clear();
  lcd.setCursor(0, 0);
  lcd.print("Thinking...");
  delay(1000);

  // Example: check for 0x33, 0x82, 0xDA, 0x11
  if (rfid.uid.uidByte[0] == 0x33 && 
      rfid.uid.uidByte[1] == 0x82 && 
      rfid.uid.uidByte[2] == 0xDA && 
      rfid.uid.uidByte[3] == 0x11) {
    doorLocked = !doorLocked;
    controlDoor(doorLocked, "RFID");
  } else {
    showError("RFID");
  }
  rfid.PICC_HaltA();
  rfid.PCD_StopCrypto1();
}

// ------------------- controlDoor() -------------------
void controlDoor(bool lock, String method) {
  // beep once
  digitalWrite(BUZZER_PIN, HIGH);
  delay(300);
  digitalWrite(BUZZER_PIN, LOW);

  // servo
  servo.write(lock ? 110 : 50);

  // LCD & logs
  lcd.clear();
  lcd.setCursor(0, 0);
  lcd.print(lock ? "Locked" : "Unlocked");

  eventCounter++;
  accessLog += "<p>" + String(eventCounter) + ". The door is "
            + (lock ? "locked" : "unlocked")
            + " (by " + method + ").</p>";

  if (!lock) {
    unlockTime = millis();
    timerActive = true;
  } else {
    timerActive = false;
  }

  // push update
  sendDoorStatus();
  delay(1000);
  displayWelcomeMessage();
}

void showError(String method) {
  // beep twice
  for (int i = 0; i < 2; i++) {
    digitalWrite(BUZZER_PIN, HIGH);
    delay(200);
    digitalWrite(BUZZER_PIN, LOW);
    delay(200);
  }
  lcd.clear();
  lcd.setCursor(0, 0);
  lcd.print("Wrong Access");

  eventCounter++;
  accessLog += "<p>" + String(eventCounter) + ". Wrong access (by "
            + method + ").</p>";
  sendDoorStatus();

  delay(1000);
  displayWelcomeMessage();
}

void resetPassword() {
  password.reset();
}

void displayWelcomeMessage() {
  lcd.clear();
  lcd.setCursor(0, 0);
  lcd.print("Welcome Password");
}

// ------------------- handleRoot() & generateWebPage() -------------------
void handleRoot(AsyncWebServerRequest *request) {
  String htmlContent = generateWebPage();
  request->send(200, "text/html", htmlContent);
}

// *** EXACT old website code for the HTML/JS block ***
String generateWebPage() {
  String htmlContent = R"rawliteral(
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<style>
body {
  font-family: Arial, 'Apple Color Emoji', sans-serif;
  background-color: #8a2be2;
  color: #000;
  display: flex;
  align-items: center;
  justify-content: center;
  height: 100vh;
  margin: 0;
  flex-direction: column;
}
#accessLog {
  width: 90%;
  height: 250px;
  border: 2px solid #636363;
  border-radius: 10px;
  padding: 20px;
  background-color: #f9f9f9;
  overflow-y: auto;
  margin-top: 20px;
  font-size: 18px;
  line-height: 1.5;
  color: #000;
}
.animated-title {
  font-size: 48px;
  font-weight: bold;
  color: #ff007f;
  animation: float 3s ease-in-out infinite, colorchange 6s linear infinite;
}
@keyframes float {
  0%, 100% { transform: translateY(0); }
  50% { transform: translateY(-20px); }
}
@keyframes colorchange {
  0% { color: #ff007f; }
  25% { color: #ffbf00; }
  50% { color: #00ff7f; }
  75% { color: #00bfff; }
  100% { color: #ff007f; }
}
.timer {
  font-size: 48px;
  font-weight: bold;
  margin-bottom: 20px;
}
.switch {
  width: 60px;
  height: 35px;
  display: inline-block;
  cursor: pointer;
  position: relative;
  margin-top: 20px;
}
.switch .slider {
  position: absolute;
  inset: 0;
  background: rgb(145, 145, 145);
  border-radius: 25px;
  transition: 0.5s background;
}
.switch input {
  display: none;
}
.switch .slider::before {
  content: "";
  position: absolute;
  height: 27px;
  width: 27px;
  left: 5px;
  top: 4px;
  background: white;
  transition: 0.5s all;
  border-radius: 50%;
}
.switch input:checked + .slider {
  background: rgb(12, 235, 12); /* Green for unlocked */
}
.switch input:checked + .slider::before {
  transform: translateX(23px);
}
.switch input:not(:checked) + .slider {
  background: rgb(235, 12, 12); /* Red for locked */
}
.theme-toggle {
  position: absolute;
  top: 10px;
  right: 10px;
  width: 150px;
  height: 35px;
  display: flex;
  align-items: center;
  justify-content: space-between;
}
.theme-toggle .slider {
  position: relative;
  width: 60px;
  height: 35px;
  border-radius: 25px;
  transition: 0.5s background;
  display: inline-block;
}
.theme-toggle input {
  display: none;
}
.theme-toggle .slider::before {
  content: "";
  position: absolute;
  height: 27px;
  width: 27px;
  left: 5px;
  top: 4px;
  transition: 0.5s all;
  border-radius: 50%;
}
.theme-toggle input:checked + .slider {
  background: #ffffff; /* White for dark mode */
}
.theme-toggle input:checked + .slider::before {
  background: black; /* Black knob in dark mode */
  transform: translateX(23px);
}
.theme-toggle input:not(:checked) + .slider {
  background: #ffbf00; /* Yellow for light mode */
}
.theme-toggle input:not(:checked) + .slider::before {
  background: white; /* White knob in light mode */
}
.theme-toggle .sun-icon, .theme-toggle .moon-icon {
  font-size: 25px;
}
input[type="password"] {
  padding: 10px;
  font-size: 16px;
  border-radius: 5px;
  border: 1px solid #ccc;
  width: 200px;
}
button {
  padding: 10px;
  font-size: 16px;
  border-radius: 5px;
  border: none;
  background-color: #4CAF50;
  color: white;
  cursor: pointer;
  margin-left: 10px;
}
button:hover {
  background-color: #45a049;
}
</style>
<script>
let doorLocked = true;
let passwordVerified = false;
let darkMode = false;
let remainingTime = 20000; // Initial remaining time

// WebSocket initialization
var gateway = `ws://${window.location.hostname}/ws`;  // <-- FIXED with backticks!
var websocket;

window.addEventListener('load', onLoad);

function onLoad(event) {
  initWebSocket();
  setInterval(function() {
    if (!doorLocked && remainingTime > 0) {
      remainingTime -= 1000;
      if (remainingTime < 0) remainingTime = 0;
      updateTimerDisplay();
    }
  }, 1000);
}

function initWebSocket() {
  websocket = new WebSocket(gateway);
  websocket.onopen    = function(event){ console.log('WebSocket opened'); };
  websocket.onclose   = function(event){ console.log('WebSocket closed'); setTimeout(initWebSocket, 2000); };
  websocket.onmessage = function(event){ onMessage(event); };
}

function onMessage(event) {
  var data = JSON.parse(event.data);
  doorLocked = data.doorLocked;
  document.getElementById('switch-1').checked = !doorLocked;
  document.getElementById('accessLog').innerHTML = data.accessLog;
  remainingTime = data.remainingTime;
  updateTimerDisplay();
}

function verifyPassword() {
  const password = document.getElementById('password').value;
  if (password === '1234') {
    passwordVerified = true;
    document.getElementById('passwordBox').style.display = 'none';
    alert('Password correct! You can now use the switch to unlock or lock the door.');
  } else {
    alert('Incorrect password. Please try again.');
  }
}

function handleChange() {
  if (!passwordVerified) {
    alert('Please enter the correct password first.');
    document.getElementById('switch-1').checked = doorLocked;
    return;
  }
  var message = document.getElementById('switch-1').checked ? 'unlock' : 'lock';
  websocket.send(message);
}

function toggleTheme() {
  darkMode = !darkMode;
  document.body.style.backgroundColor = darkMode ? '#000' : '#8a2be2';
  document.body.style.color = darkMode ? '#f0f0f0' : '#000';
  document.getElementById('accessLog').style.backgroundColor = darkMode ? '#333' : '#f9f9f9';
  document.getElementById('accessLog').style.color = darkMode ? '#f0f0f0' : '#000';
  document.getElementById('timer').style.color = darkMode ? '#f0f0f0' : '#000';
}

function updateTimerDisplay() {
  var timerElement = document.getElementById('timer');
  if (doorLocked) {
    timerElement.innerHTML = '00:20';
  } else {
    var seconds = Math.ceil(remainingTime / 1000);
    var displaySeconds = seconds < 10 ? '0' + seconds : seconds;
    timerElement.innerHTML = '00:' + displaySeconds;
  }
}
</script>
</head>
<body>
<label class="theme-toggle">
  <span class="sun-icon">☀️</span>
  <input type="checkbox" id="theme-switch" onclick="toggleTheme()" />
  <span class="slider"></span>
  <span class="moon-icon">🌙</span>
</label>
<div id="timer" class="timer">00:10</div>
<h1 class="animated-title">Smart Door</h1>
<div id="passwordBox" class="password-box">
  <input type="password" id="password" placeholder="Enter password" />
  <button onclick="verifyPassword()">Verify</button>
</div>
<label for="switch-1" class="switch">
  <input type="checkbox" id="switch-1" onclick="handleChange()" />
  <span class="slider"></span>
</label>
<div id="accessLog"><p>1. The door is locked.</p></div>
</body>
</html>
)rawliteral";

  // For dynamic updates
  htmlContent.replace("doorLocked = true;",  "doorLocked = " + String(doorLocked ? "true" : "false") + ";");
  htmlContent.replace("passwordVerified = false;", "passwordVerified = " + String("false") + ";");
  htmlContent.replace("darkMode = false;",  "darkMode = " + String(darkMode ? "true" : "false") + ";");
  htmlContent.replace(
    "<div id=\"accessLog\"><p>1. The door is locked.</p></div>", 
    "<div id=\"accessLog\">" + accessLog + "</div>"
  );

  return htmlContent;
}

// ------------------- WebSocket Events -------------------
void onWebSocketEvent(AsyncWebSocket *server, AsyncWebSocketClient *client,
                      AwsEventType type, void *arg, uint8_t *data, size_t len) {
  if (type == WS_EVT_CONNECT) {
    // Send the current status to newly connected client
    sendDoorStatusToClient(client);
  } 
  else if (type == WS_EVT_DATA) {
    AwsFrameInfo *info = (AwsFrameInfo *)arg;
    if (info->opcode == WS_TEXT) {
      data[len] = 0;
      String message = (char *)data;
      if (message == "unlock") {
        if (doorLocked) {
          doorLocked = false;
          controlDoor(doorLocked, "website");
        }
      } else if (message == "lock") {
        if (!doorLocked) {
          doorLocked = true;
          controlDoor(doorLocked, "website");
        }
      }
    }
  }
}

void sendDoorStatus() {
  unsigned long remainingTimeVal = 0;
  if (timerActive) {
    unsigned long elapsed = millis() - unlockTime;
    if (elapsed < countdownDuration) {
      remainingTimeVal = countdownDuration - elapsed;
    }
  }

  String escapedLog = accessLog;
  escapedLog.replace("\"", "\\\"");
  escapedLog.replace("\n", "\\n");
  escapedLog.replace("\r", "\\r");

  String json = "{\"doorLocked\":" 
              + String(doorLocked ? "true" : "false")
              + ",\"accessLog\":\"" + escapedLog + "\""
              + ",\"remainingTime\":" + String(remainingTimeVal)
              + "}";
  ws.textAll(json);
}

void sendDoorStatusToClient(AsyncWebSocketClient *client) {
  unsigned long remainingTimeVal = 0;
  if (timerActive) {
    unsigned long elapsed = millis() - unlockTime;
    if (elapsed < countdownDuration) {
      remainingTimeVal = countdownDuration - elapsed;
    }
  }

  String escapedLog = accessLog;
  escapedLog.replace("\"", "\\\"");
  escapedLog.replace("\n", "\\n");
  escapedLog.replace("\r", "\\r");

  String json = "{\"doorLocked\":" 
              + String(doorLocked ? "true" : "false")
              + ",\"accessLog\":\"" + escapedLog + "\""
              + ",\"remainingTime\":" + String(remainingTimeVal)
              + "}";
  client->text(json);
}

