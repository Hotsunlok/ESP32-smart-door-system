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
