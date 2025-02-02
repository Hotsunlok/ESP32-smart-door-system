# Face Recognition System

## Introduction

- 😊 Face recognition is a technology used to identify or verify a person using their facial features.
- 📸 It captures an image, analyzes facial patterns, and compares them with stored data.
- 🔒 This system is commonly used in security, mobile devices, and smart access control.
- 🚪 Our system integrates face recognition with an ESP32 smart door lock to provide secure and automated access.

## Requirements

To run the face recognition system, the following technologies are required:

- 🐍 **Python**: Used for processing the face recognition logic.
- 🤖 **OpenCV**: A computer vision library that helps detect and recognize faces.
- 🔌 **Arduino C**: Used for programming the ESP32 microcontroller to control the door lock.

These three components work together to capture images, analyze faces, and send unlock signals to the smart door lock.

## Python OpenCV Code

💾 **The full Python OpenCV code can be found here:**  
[📂 Click to Access the Code](Full_Python_Face_Recognition_Code.md)

💾 **The full Arduino C code can be found here:**  
[📂 Click to Access the Code](Full_Arduino_Code.md)

## Important Setup Steps ⚠️

🚨 **Before running the face recognition code, you must complete the following steps!** 🚨

⚡ **PLS BE CAUTION**: These steps **MUST** be done first to ensure everything works smoothly!

🔍 **Please ensure that your computer camera is working fine, or you can connect a USB camera.**

💻 My device is using a **Logitech USB-connected camera** for face recognition.

🖼️ **Camera Picture:**  
![Logitech USB Camera](https://github.com/Hotsunlok/ESP32-smart-door-system/blob/8038f1cc167ce2bb21b5b2b31a5d0b44430b55e7/assets/IMG_0255.jpg)

🛒 **Buy the Camera Here:**  
[🔗 Click to Buy Logitech USB Camera](https://www.logitech.com/en-gb/shop/p/brio-100-webcam.960-001585)


---

# Step-by-Step Guide 📖

👉 **The following steps are based on the linked guide**.  
If you want to fully understand the process, **please watch the link carefully**! 🎥  

🚨 **This guide is very important!** 🚨  
It covers all the necessary steps to implement Face Recognition and Identification using OpenCV and Arduino.  

### 🔗 [Face Recognition and Identification | Arduino Face ID Using OpenCV Python and Arduino](https://www.instructables.com/Face-Recognition-and-Identification-Arduino-Face-I/)

### 📌 Steps Covered in the Guide:
1️⃣ **Access to Webcam** 🎥 – Capturing real-time face data  
2️⃣ **Face Identification** 🧐 – Detecting and recognizing faces  
3️⃣ **Data Collection** 📊 – Storing recognized faces for training  
4️⃣ **DATA Training** 📈 – Training the system to recognize faces  
5️⃣ **Programming Arduino** 💻 – Integrating with ESP32 for smart door control  

⚡ **Make sure to follow all steps in the video carefully to ensure proper setup!**

---

# Step 1️⃣: Access to Webcam 🎥

In the original tutorial, the author provides a **Python script (AccessTo_webcam.py)** to access the webcam using OpenCV.  
However, if you haven't installed **OpenCV**, you'll need to set it up first.  

### 🖥️ Using Visual Studio Code  
I assume you are using **Visual Studio Code** to run the Python script.  
Before proceeding, make sure you have **Visual Studio Code installed**.  

🔗 **Download Visual Studio Code here:**  
[📥 Click to Download](https://code.visualstudio.com/)

---

### ⚠️ Important: Installing OpenCV  
The tutorial on the original website **may not be the best** guide for installing OpenCV.  
I encountered **many mistakes and issues** while following it.  

👉 **Instead, I highly recommend watching this YouTube video** before installing OpenCV:  

📺 **[How To Install OpenCV Python in Visual Studio Code (Windows 11)](https://www.youtube.com/watch?v=fclTFQQvQFQ)**  

Make sure to **follow the video carefully** to avoid common installation errors.  

---

After **installing OpenCV properly**, you can download and run the **AccessTo_webcam.py** script from the original tutorial.
---

### 📂 AccessTo_webcam.py Code  

![AccessTo_webcam.py Screenshot](https://github.com/Hotsunlok/ESP32-smart-door-system/blob/48f02367200b846e597f075d694fca01663c57b2/assets/Accesstowebcam.jpg)

If you **could not download the script**, don't worry!  
The **Python code is shared below** so you can **copy and paste** it into your own script.  

### 🎥 What to Expect After Running the Code
- Once you run the script, your **camera will activate** automatically.  
- You will see a **real-time video feed** from your camera in a new OpenCV window.  
- This window will display **whatever the camera captures** in real-time.  

💡 **Tip:** If your camera does not activate, check if it's properly connected or try using an external **USB webcam**.  

```python
# import OpenCV library
import cv2

# Create a VideoCapture() object as "video"
video = cv2.VideoCapture(0)

# Enter into an infinite while loop
while True:
    # Data of 1 frame is read every time when this while loop is repeated
    # into the variable frame
    # check contains the boolean value i.e True or False if its True then
    # webcam is active
    check, frame = video.read()

    # imshow is a method of cv2 library which will basically show the image
    # or frame on a new window
    cv2.imshow("Video", frame)

    # Creates a delay of 1 millisecond and stores the value to variable key
    # if any key is pressed on keyboard
    key = cv2.waitKey(1)

    # check if key is equal to 'q' if it is then break out of the loop
    if key == ord('q'):
        break

# Release the webcam. In other words, turn it off
video.release()

# Destroys all the windows which were created
cv2.destroyAllWindows()
```
---
# Step 2️⃣: Face Identification 🧐  

Now that we have access to the webcam, the next step is **face identification**.  
This step will allow us to **detect faces** in the video stream using OpenCV.  

---

### 📥 **Download Required Files**  
Before proceeding, download the following files:  
1️⃣ **Face_identification.py** → The main script for face detection  
2️⃣ **haarcascade_frontalface_default.xml** → A pre-trained model for face detection  

🔗 **Download the required files here:**  
 ![Face_identification.py and haarcascade_frontalface_default.xml Screenshot](https://github.com/Hotsunlok/ESP32-smart-door-system/blob/07d7bd9439632b3abc20918c4ba41ef9eef1b7ab/assets/%E8%9E%A2%E5%B9%95%E6%93%B7%E5%8F%96%E7%95%AB%E9%9D%A2%202025-02-02%20125109.jpg)

### `Face_identification.py` code can also found in here
```python
import cv2

video = cv2.VideoCapture(0)

# load "haarcascade_frontalface_default.xml" by creating a CascadeClassifier
# object as cascade
cascade = cv2.CascadeClassifier("haarcascade_frontalface_default.xml")

while True:
    check,frame = video.read()
    
    # Image from webacm is in the format of BGR i.e combination of 3 colours
    # which will basicall require more amount of computation.
    # so we convert it into a gray scale image which is only single colour
    # and requires less computation.
    gray = cv2.cvtColor(frame, cv2.COLOR_BGR2GRAY)
    
    # Now we use detectMultiScale method to detect the faces in the video
    # stream. Which will return x,y,w,h which are basically the positions
    # with which we create a rectangle box.
    face = cascade.detectMultiScale(gray, scaleFactor = 1.1, minNeighbors = 6)

    # using for loop to go through the locations x,y,w,h and drow a rectangle
    for x,y,w,h in face:
        frame = cv2.rectangle(frame, (x,y), (x+w,y+h), (0,255,255), 3)
        
    cv2.imshow("Video",frame)
    key = cv2.waitKey(1)
    if(key == ord('q')):
        break

video.release()
cv2.destroyAllWindows()
```
---

### 📂 **Organizing Your Project Folder**  
1. Create a **new folder** called **`OpenCV_PROJECT`**.  
2. Place the following files inside the folder:  
   - 📄 `Face_identification.py`  
   - 📄 `haarcascade_frontalface_default.xml`  
   - 📄 `AccessTo_webcam.py` (from Step 1)  

### 📷 screenshot of the OPENCV_PROJECT
![file Screenshot](https://github.com/Hotsunlok/ESP32-smart-door-system/blob/75a0f978c9bce3cf58ff1426f887714b177eb579/assets/%E8%9E%A2%E5%B9%95%E6%93%B7%E5%8F%96%E7%95%AB%E9%9D%A2%202025-02-02%20125702.jpg)

---

### ▶️ **Running the Code**  
Once everything is set up, follow these steps to run the face identification script:  
1. **Open Visual Studio Code** and navigate to the `OpenCV_PROJECT` folder.  
2. Run the python `Face_identification.py`:  

---
### 🎥 **What Happens Next?**  
- 🖥️ **The camera will activate**, and a **real-time video feed** will appear.  
- 😊 **You will see your face displayed on the monitor**.  
- 🟡 **A yellow square box** will appear around your face.  
- 🎯 **The box follows your face** as you move, tracking your position dynamically.  

---
### 📸 **Successful Face Detection Screenshot**  
Once the system is working, you should see something like this:  
![face detection screenshot](https://github.com/Hotsunlok/ESP32-smart-door-system/blob/74ce4fddbb6f39f0465d4bfc10b80732c3b32ef6/assets/IMG_9750.jpg)

---
# Step 3️⃣: Data Collection 📸  

To train the AI model for face recognition, you need to **capture multiple images** of yourself.  
These images will be used later to **train the system** to recognize your face accurately.  

---

### 🎯 **How to Capture Images Properly**  
1️⃣ **Use the same camera** you used in Step 1 & 2.  
2️⃣ **Take at least 20+ pictures** of yourself.  
3️⃣ **Keep the background consistent** and **ensure good lighting** (avoid shadows).  
4️⃣ Capture **different angles and expressions**:  
   - 🏞️ **Front face, left face, right face, up, and down**  
   - 😀 **Smile, sad, angry expressions**  
   - 👓 **With and without glasses**  

💡 **For example, I took 60 images and named them from `000.jpg` to `060.jpg`**.

---

### 📂 **Organizing Your Data Folder**  
1. Create a folder named **`image_data`** inside your `OpenCV_PROJECT` directory.  
2. Inside `image_data`, create another folder **with your name** (e.g., `Jacky`).  
3. Move all the **60 captured images** into this folder.  
4. Ensure your images are named **sequentially** (`000.jpg`, `001.jpg`, ..., `060.jpg`).  

---

### 📷 **Example Screenshot of Image Collection**
![Image Data Collection](https://github.com/Hotsunlok/ESP32-smart-door-system/blob/1b584379699e1a2defc191aa62e0aeabc46b0449/assets/%E8%9E%A2%E5%B9%95%E6%93%B7%E5%8F%96%E7%95%AB%E9%9D%A2%202025-02-02%20131635.jpg)

---

### 🔍 **Check Your Library Manager**
Once you have stored your images correctly, you should see the folder order in your **file explorer or library manager**.

![Library Manager View](https://github.com/Hotsunlok/ESP32-smart-door-system/blob/e69a0a4e4a5d14a8761abc9c6425ca928a8c9974/assets/%E8%9E%A2%E5%B9%95%E6%93%B7%E5%8F%96%E7%95%AB%E9%9D%A2%202025-02-02%20132021.jpg)

---
# Step 4️⃣: Training the Face Recognition Model 🤖  

Now that we have collected the image data, it's time to **train the AI model**.  
This step will go through all the images, detect faces, and generate the necessary files for recognition.  

---

### 📥 **Download Required File**  
To train the model, download and place the following file in your **OpenCV_PROJECT** folder:  

🔗 **Download face_trainer.py:**  
![📂 face_trainer.py](https://github.com/Hotsunlok/ESP32-smart-door-system/blob/1b6d68efed53896f8232144bc95bd3b8d1db3f2f/assets/%E8%9E%A2%E5%B9%95%E6%93%B7%E5%8F%96%E7%95%AB%E9%9D%A2%202025-02-02%20133129.jpg)  

---

### ▶️ **Running the Training Script**  
1. Open **Visual Studio Code** and navigate to the **OpenCV_PROJECT** folder.  
2. Run the `python face_trainer.py` 

---
### ⏳ **Wait for the Training Process**
3. **Wait patiently** as the script processes all the images and generates model files.

---

### 📂 **Output Files**  
After training, the script will create **two new files**:  
- 🗂 **`labels.pickle`** → Stores face labels and names.  
- 🗂 **`trainner.yml`** → Contains trained face recognition data.  

---

### 🔍 **Confirm the Output in Your Library Manager**  
1. Open your **file explorer** or **library manager**.  
2. Ensure that **`labels.pickle`** and **`trainner.yml`** are inside the **OpenCV_PROJECT** folder.  
3. If these files are missing, **move them manually** to the correct folder.  
![two file structure.py](https://github.com/Hotsunlok/ESP32-smart-door-system/blob/466e8febf71d334ec85b77746a48120797010942/assets/%E8%9E%A2%E5%B9%95%E6%93%B7%E5%8F%96%E7%95%AB%E9%9D%A2%202025-02-02%20133404.jpg)  
   
### `Face trainer.py` code can also found in here
```python
# import the required libraries
import cv2
import os
import numpy as np
from PIL import Image
import pickle


cascade = cv2.CascadeClassifier("haarcascade_frontalface_default.xml")

recognise = cv2.face.LBPHFaceRecognizer_create()

# Created a function 
def getdata():

    current_id = 0
    label_id = {} #dictionanary
    face_train = [] # list
    face_label = [] # list
    
    # Finding the path of the base directory i.e path were this file is placed
    BASE_DIR = os.path.dirname(os.path.abspath(__file__))

    # We have created "image_data" folder that contains the data so basically
    # we are appending its path to the base path
    my_face_dir = os.path.join(BASE_DIR,'image_data')

    # Finding all the folders and files inside the "image_data" folder
    for root, dirs, files in os.walk(my_face_dir):
        for file in files:

            # Checking if the file has extention ".png" or ".jpg"
            if file.endswith("png") or file.endswith("jpg"):

                # Adding the path of the file with the base path
                # so you basically have the path of the image 
                path = os.path.join(root, file)

                # Taking the name of the folder as label i.e his/her name
                label = os.path.basename(root).lower()

                # providing label ID as 1 or 2 and so on for different persons
                if not label in label_id:
                    label_id[label] = current_id
                    current_id += 1
                ID = label_id[label]

                # converting the image into gray scale image
                # you can also use cv2 library for this action
                pil_image = Image.open(path).convert("L")

                # converting the image data into numpy array
                image_array = np.array(pil_image, "uint8")
        
                # identifying the faces
                face = cascade.detectMultiScale(image_array)

                # finding the Region of Interest and appending the data
                for x,y,w,h in face:
                    img = image_array[y:y+h, x:x+w]
                #image_array = cv2.rectangle(image_array,(x,y),(x+w,y+h),(255,255,255),3)
                    cv2.imshow("Test",img)
                    cv2.waitKey(1)
                    face_train.append(img)
                    face_label.append(ID)

    # string the labels data into a file
    with open("labels.pickle", 'wb') as f:
        pickle.dump(label_id, f)
   

    return face_train,face_label

# creating ".yml" file
face,ids = getdata()
recognise.train(face, np.array(ids))
recognise.save("trainner.yml")
```
---
# Step 5️⃣: Running the Face Recognition Code 🧠  

Now that we have trained the model, it's time to **run the face recognition code**.  
This script will detect and recognize faces using the trained model.  

---

### 📥 **Download the Face Recognition Code**  
1. **Download the `face_recognise.py` file** from the website.  
2. Ensure that it is placed inside the **OpenCV_PROJECT** folder.  

🔗 **Download face_recognise.py:**  
![📂 face_recognise.py](https://github.com/Hotsunlok/ESP32-smart-door-system/blob/3f0d6ff6229d157fe109c9ef261f2e6d130dabed/assets/%E8%9E%A2%E5%B9%95%E6%93%B7%E5%8F%96%E7%95%AB%E9%9D%A2%202025-02-02%20134651.jpg)  

---

### ▶️ **Running the Code**  
1. Open **Visual Studio Code** and navigate to the **OpenCV_PROJECT** folder.  
2. Run the `face_recognise.py`
---
## 🧠 **What Happens Next?**  
- 🖥️ **The camera will activate**, and a **real-time video feed** will appear.  
- 🟡 **A yellow square box** will **detect and track your face**.  
- 🏷️ **Your name (e.g., "Jacky")** will be displayed in the **top-left corner** of the yellow box.  
- 🎯 As you move, the **box will follow your face dynamically**.  

---

### 📸 **Screenshot of Face Recognition in Action**  
Once the system is running, you should see something like this:  
![ face_recognise screenshot](https://github.com/Hotsunlok/ESP32-smart-door-system/blob/c4ce9dd9f7e52e1f10b333bb7c0ec0d99581778f/assets/IMG_0166.jpg) 

---

### ⚠️ **This is Not Yet the Final Version!**  
- 🗂️ The **`face_recognise.py`** script is a **pre-built version** of the recognition system.  
- ❌ **This is not the final completed version!**  
- 🔜 Step **6️⃣** will provide the **full Python recognition code** that integrates with the **ESP32**.  
---

# Step 6️⃣: Full Face Recognition Code with ESP32 🌐  

Now that you have completed **Step 5**, it's time to upgrade to the **full face recognition code** that integrates with the **ESP32 smart lock system**!  

---

### ⚠️ **Before Using This Code**  
- 🛑 **Make sure you have successfully completed Step 5** before proceeding.  
- ✅ Your **face recognition system must be working properly** before using this updated version.  
- 🚀 This version **adds new features**, including **sending a signal to the ESP32** when a recognized face is detected.  

---

### 📝 **What’s New in This Version?**  
1. 🌍 **ESP32 Web Communication** – Sends a signal to the ESP32 when your face is recognized.  
2. ⏳ **Cooldown Mechanism** – Prevents multiple unlock signals from being sent in quick succession.  
3. 🔄 **Real-time Face Tracking** – Ensures smooth face detection with improved tracking.  

---

### 📜 **Updated Face Recognition Code**  
If you **could not download the script**, don't worry!  
The **Python code is shared below** so you can **copy and paste** it into your own script.  

```python
import cv2
import pickle
import time
import requests

# Global variables
cooldown_active = False   # Track if the cooldown period is active
cooldown_duration = 5     # Cooldown period in seconds

# Single toggle endpoint (update if your ESP32 IP or endpoint name is different)
ESP32_URL_TOGGLE = "http://192.168.4.1/face_toggle"


def send_to_esp32(url):
    """
    Send an HTTP POST request to the ESP32.
    """
    try:
        response = requests.post(url, timeout=3)  # Add timeout for reliability
        if response.status_code == 200:
            print(f"ESP32 Response: {response.text}")
        else:
            print(f"ESP32 Error: HTTP {response.status_code}")
    except Exception as e:
        print(f"Failed to send request to ESP32: {e}")


def face_recognition():
    """
    Perform face recognition and send toggle commands to ESP32.
    """
    global cooldown_active

    # Initialize camera and face cascade
    video = cv2.VideoCapture(0)
    cascade = cv2.CascadeClassifier("haarcascade_frontalface_default.xml")

    # Load the face recognizer and the trained data
    recognise = cv2.face.LBPHFaceRecognizer_create()
    recognise.read("trainner.yml")

    # Load label mappings
    labels = {}
    with open("labels.pickle", 'rb') as f:
        og_label = pickle.load(f)
        labels = {v: k for k, v in og_label.items()}
        print("Labels:", labels)

    while True:
        check, frame = video.read()
        gray = cv2.cvtColor(frame, cv2.COLOR_BGR2GRAY)
        faces = cascade.detectMultiScale(gray, scaleFactor=1.2, minNeighbors=5)

        for x, y, w, h in faces:
            face_roi = gray[y:y+h, x:x+w]  # Region of interest

            # Predict the face
            ID, conf = recognise.predict(face_roi)
            if 20 <= conf <= 115:
                print(f"ID: {ID}, Name: {labels[ID]}, Confidence: {conf}")
                
                # Draw a rectangle and label around the face
                cv2.putText(frame, labels[ID], (x-10, y-10),
                            cv2.FONT_HERSHEY_COMPLEX, 1, (18, 5, 255), 2, cv2.LINE_AA)
                cv2.rectangle(frame, (x, y), (x+w, y+h), (0, 255, 255), 4)

                # Check cooldown before sending another toggle
                if cooldown_active:
                    print("Cooldown active. Skipping detection...")
                    continue  # Skip if cooldown hasn't finished

                # Toggle the door on the ESP32
                print("Face recognized. Sending toggle command to ESP32.")
                send_to_esp32(ESP32_URL_TOGGLE)

                # Start cooldown
                cooldown_active = True
                start_cooldown()

        cv2.imshow("Face Recognition", frame)
        key = cv2.waitKey(1)
        if key == ord('q'):
            break

    video.release()
    cv2.destroyAllWindows()


def start_cooldown():
    """
    Start a cooldown timer to prevent repeated commands within a short time.
    """
    global cooldown_active
    print("Starting cooldown...")
    time.sleep(cooldown_duration)
    cooldown_active = False
    print("Cooldown ended. Ready to detect again.")


if __name__ == "__main__":
    face_recognition()
```
## 💡 **How Does the ESP32 Communication Work?**  

The **face recognition system** is integrated with an **ESP32 smart door lock**. When your face is detected, a signal is sent to the ESP32 to **toggle the door lock**.

---

### 🌍 **Sending a Signal to the ESP32**  
When a recognized face is detected, the system **sends an HTTP POST request** to the ESP32 using the following command:

```python
ESP32_URL_TOGGLE = "http://192.168.4.1/face_toggle"

def send_to_esp32(url):
    """
    Send an HTTP POST request to the ESP32.
    """
    try:
        response = requests.post(url, timeout=3)  # Timeout for reliability
        if response.status_code == 200:
            print(f"ESP32 Response: {response.text}")
        else:
            print(f"ESP32 Error: HTTP {response.status_code}")
    except Exception as e:
        print(f"Failed to send request to ESP32: {e}")
```
### 📌 **Key Explanation:**  
- 🌍 The **ESP32 runs a web server** at `http://192.168.4.1`, which listens for toggle commands.  
- 📡 The **`requests.post(url)`** function **sends the unlock signal** when a face is recognized.  
- ✅ If the **ESP32 responds with HTTP 200**, the command was successful.  
- ⏳ A **timeout of 3 seconds** ensures the system doesn’t hang if the ESP32 is unreachable.  

---

### ⏳ **Cooldown Mechanism: Preventing Multiple Unlocks**  
To **prevent excessive requests**, a **cooldown period** is implemented.  
This stops the system from **spamming the ESP32** with multiple unlock signals in a short time.  

```python
# Global variable to track cooldown status
cooldown_active = False   
cooldown_duration = 5  # Cooldown period in seconds

def start_cooldown():
    """
    Start a cooldown timer to prevent repeated commands within a short time.
    """
    global cooldown_active
    print("Starting cooldown...")
    time.sleep(cooldown_duration)  # Wait for the cooldown duration
    cooldown_active = False
    print("Cooldown ended. Ready to detect again.")
```
### 📌 **Key Explanation:**  
- The variable **`cooldown_active`** is used to **track if a cooldown is in progress**.  
- Once a face is detected, the system **waits for `cooldown_duration` (5 seconds)** before allowing another unlock attempt.  
- This **prevents unnecessary duplicate unlock signals** from being sent repeatedly.  

---

### 🟡 **Integrating Face Recognition with ESP32 Communication**  
The **ESP32 unlock command** is only sent **if the cooldown is not active**:  
```python
if 20 <= conf <= 115:  # Confidence level check
    print(f"ID: {ID}, Name: {labels[ID]}, Confidence: {conf}")

    # Only send unlock command if cooldown is NOT active
    if cooldown_active:
        print("Cooldown active. Skipping detection...")
    else:
        print("Face recognized. Sending toggle command to ESP32.")
        send_to_esp32(ESP32_URL_TOGGLE)

        # Activate cooldown
        cooldown_active = True
        start_cooldown()
```
### 📌 **Key Explanation:**  
- The system **checks if the detected face is within the confidence range** (`20 ≤ conf ≤ 115`).  
- If a valid face is recognized and **cooldown is NOT active**, it **sends the unlock signal** to the ESP32.  
- After sending the unlock command, the **cooldown is activated** to prevent immediate re-triggering.  

---

### 🎥 **Final Behavior**  
- 🖥️ **Your camera activates** and detects your face.  
- 🟡 **A yellow box tracks your face**, and **your name appears in the corner**.  
- 🔓 When recognized, a **signal is sent to ESP32** to unlock the door.  
- ⏳ **A cooldown prevents repeated unlocks** for **5 seconds**.  

---
# Step 7️⃣: ESP32 Integration with Face Recognition 🏠🔓  

After integrating **Face Recognition** into the ESP32 smart lock system, we will look back to the C code. 
several key **changes were made to the system behavior**, **web interface**, and **auto-lock timer**.  

---

### 🔍 **Key Changes Due to Face Recognition Integration**  

### 1️⃣ **Face Recognition as a New Unlock Method**  
- When **Face Recognition** is used to unlock the door, the **web interface logs the event as "by FACE ID"**.  
- The ESP32 processes the request via the **`/face_toggle`** endpoint to toggle the door state.  

#### 📜 **Code Extract (Handling Face Recognition Unlock Event)**
```cpp
server.on("/face_toggle", HTTP_POST, [](AsyncWebServerRequest *request) {
    doorLocked = !doorLocked; 
    controlDoor(doorLocked, "FACE ID");  // Log event as "by FACE ID"
    String responseText = doorLocked ? "Face Lock Successful" : "Face Unlock Successful";
    request->send(200, "text/plain", responseText);
});
```
---
### 🔹 **Explanation:**  
- The **`/face_toggle`** endpoint toggles the door lock when triggered by **Face Recognition**.  
- The **access log** in the web UI will display the unlock event as **"by FACE ID"**.  

---

### 2️⃣ **Auto-Lock Timer Extended (From 10s → 20s)**  
- Since **Face Recognition takes longer to process**, the **auto-lock timer has been increased from 10s to 20s**.  
- This ensures **users have enough time to close the door** before it auto-locks.  

#### 📜 **Code Extract (Updating Auto-Lock Timer)**
```cpp
const unsigned long countdownDuration = 20000; // Auto-lock now set to 20s
```
### 🔹 **Explanation:**  
- Previously, **`countdownDuration`** was **10000 (10s)**.  
- It is now **increased to 20000 (20s)** to allow **more time for Face ID processing and door closure**.  

---

### 3️⃣ **Web Interface Timer Display Adjustments**  
- The **web UI timer dynamically updates** to reflect the **new 20-second countdown** when Face Recognition is used.  
- This provides a **visual confirmation** that users have **more time before auto-lock activates**.  

#### 📜 **Code Extract (Timer Display in Web Interface)**
```js
function updateTimerDisplay() {
    var timerElement = document.getElementById('timer');
    if (doorLocked) {
        timerElement.innerHTML = '00:20';  // Auto-lock timer updated
    } else {
        var seconds = Math.ceil(remainingTime / 1000);
        var displaySeconds = seconds < 10 ? '0' + seconds : seconds;
        timerElement.innerHTML = '00:' + displaySeconds;
    }
}
```
### 🔹 **Explanation:**  
- When the **door locks**, the **timer resets to 20 seconds**.  
- If the **door unlocks**, the **remaining time updates dynamically**.  
- This ensures **users know exactly how long they have** before auto-lock activates.  

---

### 🎯 **Summary of Face Recognition Integration in ESP32 Code**  

| Feature | Description |
|---------|-------------|
| **Face ID Unlock** | **`/face_toggle`** toggles door state and logs **"by FACE ID"** in the web UI. |
| **Extended Auto-Lock Timer** | Increased from **10s → 20s** to allow enough time for door closure. |
| **Web UI Timer Updates** | Timer dynamically adjusts to show a **20s countdown**. |

---

### 🔥 **Final Behavior After Face Recognition Unlock**  

1️⃣ ✅ **Web UI logs the unlock event as "by FACE ID".**  
2️⃣ 🚪 **Door unlocks, and the auto-lock timer starts at 20 seconds.**  
3️⃣ ⏳ **User sees the timer counting down from 20s instead of 10s.**  
4️⃣ 🔒 **After 20 seconds, the door locks automatically if no other action is taken.**  
