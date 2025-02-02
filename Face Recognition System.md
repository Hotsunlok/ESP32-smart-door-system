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

## Step 1️⃣: Access to Webcam 🎥

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
## Step 2️⃣: Face Identification 🧐  

Now that we have access to the webcam, the next step is **face identification**.  
This step will allow us to **detect faces** in the video stream using OpenCV.  

---

### 📥 **Download Required Files**  
Before proceeding, download the following files:  
1️⃣ **Face_identification.py** → The main script for face detection  
2️⃣ **haarcascade_frontalface_default.xml** → A pre-trained model for face detection  

🔗 **Download the required files here:**  
 ![Face_identification.py and haarcascade_frontalface_default.xml Screenshot](https://github.com/Hotsunlok/ESP32-smart-door-system/blob/48f02367200b846e597f075d694fca01663c57b2/assets/Accesstowebcam.jpg)

---

### 📂 **Organizing Your Project Folder**  
1. Create a **new folder** called **`OpenCV_PROJECT`**.  
2. Place the following files inside the folder:  
   - 📄 `Face_identification.py`  
   - 📄 `haarcascade_frontalface_default.xml`  
   - 📄 `AccessTo_webcam.py` (from Step 1)  

---

### ▶️ **Running the Code**  
Once everything is set up, follow these steps to run the face identification script:  
1. **Open Visual Studio Code** and navigate to the `OpenCV_PROJECT` folder.  
2. Run the python `Face_identification.py`:  


