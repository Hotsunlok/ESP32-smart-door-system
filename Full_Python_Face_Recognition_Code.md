# 📸 Full Python Face Recognition Code

This is the **complete Python script** for face recognition using OpenCV and ESP32.

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
