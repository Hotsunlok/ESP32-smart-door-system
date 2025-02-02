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
## Step 3️⃣: Data Collection 📸  

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

![Library Manager View](INSERT_IMAGE_LINK_HERE)

---
