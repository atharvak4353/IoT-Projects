# 🐾 AI Animal Detection System

## Raspberry Pi Based Real-Time Animal Detection & Alert System

<p align="center">

![Raspberry Pi](https://img.shields.io/badge/Raspberry%20Pi-AI%20Vision-C51A4A?style=for-the-badge&logo=raspberrypi&logoColor=white)
![Python](https://img.shields.io/badge/Python-3-3776AB?style=for-the-badge&logo=python&logoColor=white)
![OpenCV](https://img.shields.io/badge/OpenCV-Computer%20Vision-5C3EE8?style=for-the-badge&logo=opencv&logoColor=white)
![AI](https://img.shields.io/badge/AI-MobileNet%20SSD-16A34A?style=for-the-badge)
![Camera](https://img.shields.io/badge/Camera-Picamera2-F59E0B?style=for-the-badge)

</p>

---

# 📌 Project Overview

**AI Animal Detection** is a Raspberry Pi based computer vision project designed to detect animals in real time using a camera.

The system uses:

- 🍓 Raspberry Pi
- 📷 Raspberry Pi Camera
- 🐍 Python
- 👁️ OpenCV
- 🧠 MobileNet SSD
- 🔊 GPIO-controlled buzzer

The camera continuously captures live video.

Each frame is processed by the **MobileNet SSD deep-learning model** using OpenCV's DNN module.

When a supported animal is detected with more than **50% confidence**, the system:

- Identifies the animal
- Draws a bounding box
- Displays the animal class
- Prints the detected animal in the terminal
- Activates a buzzer connected to GPIO 18

---

# 🎯 Aim

To develop an **AI-based real-time animal detection system** using Raspberry Pi, OpenCV and MobileNet SSD that can detect selected animals from a live camera feed and automatically generate an audible alert.

---

# ✨ Key Features

| Feature | Description |
|---|---|
| 📹 Real-Time Detection | Continuously processes the live camera feed |
| 🧠 AI Recognition | Uses pretrained MobileNet SSD |
| 🐾 Multiple Animals | Detects six supported animal classes |
| 🎯 Confidence Filtering | Accepts detections above 50% confidence |
| 🟩 Bounding Boxes | Marks detected animals visually |
| 🏷️ Class Labels | Displays the detected animal name |
| 🔊 Buzzer Alert | Activates GPIO buzzer after detection |
| 💻 Terminal Output | Prints detected animals in the terminal |
| 🍓 Edge AI | Detection runs directly on Raspberry Pi |

---

# 🧰 Hardware Components

| Component | Quantity | Purpose |
|---|---:|---|
| Raspberry Pi | 1 | Main processing unit |
| Raspberry Pi Camera | 1 | Captures live video |
| Buzzer | 1 | Generates animal detection alert |
| Jumper Wires | As required | GPIO connections |
| Raspberry Pi Power Supply | 1 | Powers the Raspberry Pi |

---

# 🔌 Hardware Connections

| Component | Pin | Raspberry Pi Connection | Function |
|---|---|---|---|
| Buzzer | Positive (+) | GPIO 18 | Detection alert |
| Buzzer | Negative (-) | GND | Ground |
| Pi Camera | Ribbon Cable | Camera Connector | Live image capture |

> ⚠️ **Important:** Switch OFF the Raspberry Pi before connecting or disconnecting the camera ribbon cable.

The Python program uses **BCM GPIO numbering**, so:

```text
Buzzer Pin = BCM GPIO 18
```

---

# 🧠 AI Model

The project uses the pretrained:

## MobileNet SSD

MobileNet SSD provides a lightweight object detection model suitable for embedded computer vision applications.

The project requires two model files:

```text
deploy.prototxt
mobilenet_iter_73000.caffemodel
```

They are loaded using:

```python
net = cv2.dnn.readNetFromCaffe(
    "deploy.prototxt",
    "mobilenet_iter_73000.caffemodel"
)
```

---

# 🐕 Supported Animal Classes

The application filters MobileNet SSD detections and responds to these animals:

| Animal | MobileNet Class |
|---|---|
| 🐕 Dog | `dog` |
| 🐈 Cat | `cat` |
| 🐎 Horse | `horse` |
| 🐑 Sheep | `sheep` |
| 🐄 Cow | `cow` |
| 🐦 Bird | `bird` |

The detection threshold is:

```python
confidence > 0.5
```

Therefore, only detections with more than **50% confidence** are accepted.

---

# 🔄 How the System Works

```text
Raspberry Pi Starts
        ↓
Camera Initializes
        ↓
MobileNet SSD Model Loads
        ↓
Camera Captures Live Frame
        ↓
OpenCV Processes Frame
        ↓
Frame Converted to 300 × 300 Blob
        ↓
MobileNet SSD Performs AI Inference
        ↓
Objects are Detected
        ↓
Animal Classes are Filtered
        ↓
Confidence > 50% ?
        ↓
      YES
        ↓
Bounding Box is Drawn
        ↓
Animal Name is Displayed
        ↓
Detection Printed in Terminal
        ↓
Buzzer Activated
        ↓
System Continues Monitoring
```

---

# ⚙️ Detection Process

### 1️⃣ Camera Capture

Picamera2 continuously captures frames at:

```text
640 × 480
```

### 2️⃣ Frame Processing

OpenCV receives the camera image and prepares it for object detection.

### 3️⃣ DNN Input

The image is converted into a:

```text
300 × 300
```

blob using:

```python
cv2.dnn.blobFromImage()
```

### 4️⃣ AI Inference

MobileNet SSD analyzes the image:

```python
detections = net.forward()
```

### 5️⃣ Confidence Check

Only detections above:

```text
50%
```

are processed.

### 6️⃣ Animal Filtering

The detected object must belong to:

```python
animal_classes = [
    "dog",
    "cat",
    "horse",
    "sheep",
    "cow",
    "bird"
]
```

### 7️⃣ Visual Output

The system draws a bounding box around the animal and displays its class name.

### 8️⃣ Buzzer Alert

The buzzer connected to GPIO 18 activates for approximately:

```text
0.3 seconds
```

### 9️⃣ Continuous Monitoring

The process continues until the user presses:

```text
Q
```

---

# 💻 Software Requirements

The project uses:

| Software / Library | Purpose |
|---|---|
| Python 3 | Main programming language |
| OpenCV | Image processing and AI inference |
| Picamera2 | Raspberry Pi camera interface |
| GPIO Zero | Buzzer GPIO control |
| MobileNet SSD | Object detection model |
| FFmpeg | Multimedia support |

---

# 🛠 Raspberry Pi Setup

## Step 1 — Update Raspberry Pi

```bash
sudo apt update
```

---

## Step 2 — Install OpenCV

```bash
sudo apt install python3-opencv
```

---

## Step 3 — Download MobileNet SSD Configuration

```bash
wget https://raw.githubusercontent.com/chuanqi305/MobileNet-SSD/master/deploy.prototxt
```

---

## Step 4 — Download MobileNet SSD Model

```bash
wget https://raw.githubusercontent.com/chuanqi305/MobileNet-SSD/master/mobilenet_iter_73000.caffemodel
```

---

## Step 5 — Install Camera Libraries

```bash
sudo apt update
```

```bash
sudo apt install -y libcamera-apps python3-libcamera python3-picamera2
```

---

## Step 6 — Install FFmpeg

```bash
sudo apt install ffmpeg
```

---

## Step 7 — Install GPIO Zero

```bash
sudo apt install python3-gpiozero
```

---

## Step 8 — Reboot Raspberry Pi

```bash
reboot
```

---

# 📂 Required Project Files

Before running the program, make sure these three files are in the same project directory:

```text
animal_detection.py
deploy.prototxt
mobilenet_iter_73000.caffemodel
```

---

# ▶️ Running the Project

Open the terminal inside your project directory.

Run:

```bash
python3 animal_detection.py
```

The terminal should display:

```text
Starting Animal Detection... Press 'q' to quit
```

The camera window will open and the system will begin detecting animals.

---

# 🖥️ Detection Output

For example, if a dog enters the camera view, the application can display:

```text
DOG
```

on the camera window and print:

```text
Animal Detected: dog
```

in the terminal.

The buzzer will also activate.

---

# 🚨 Alert System

The buzzer is initialized using:

```python
from gpiozero import Buzzer

buzzer = Buzzer(18)
```

When an animal is detected:

```python
buzzer.on()

time.sleep(0.3)

buzzer.off()
```

This generates a short audible warning.

---

# 🟩 Bounding Box Detection

When a supported animal is detected, OpenCV draws a bounding box:

```python
cv2.rectangle(
    frame,
    (x1, y1),
    (x2, y2),
    (0, 255, 0),
    2
)
```

The animal class is also displayed:

```python
cv2.putText(
    frame,
    label.upper(),
    (x1, y1 - 10),
    cv2.FONT_HERSHEY_SIMPLEX,
    1,
    (0, 0, 255),
    3
)
```

---

# 🧪 Expected Result

When the project runs successfully:

- ✅ Raspberry Pi camera starts
- ✅ Live camera feed is displayed
- ✅ MobileNet SSD analyzes each frame
- ✅ Supported animals are identified
- ✅ Bounding boxes appear around detected animals
- ✅ Animal names appear on screen
- ✅ Detection information appears in the terminal
- ✅ Buzzer generates an alert
- ✅ Detection continues in real time

---

# 🔧 Troubleshooting

| Problem | Possible Cause | Solution |
|---|---|---|
| Camera not opening | Camera connection issue | Power OFF Pi and check ribbon cable |
| `No module named picamera2` | Picamera2 missing | Install `python3-picamera2` |
| OpenCV not found | OpenCV missing | Install `python3-opencv` |
| `deploy.prototxt` not found | Model configuration missing | Place file in project directory |
| Caffe model not found | Model file missing | Place `.caffemodel` in project directory |
| Animal not detected | Poor lighting / low confidence | Improve lighting and camera position |
| Buzzer not working | Incorrect GPIO wiring | Check GPIO 18 and GND |
| Q key not working | Camera window not active | Click camera window and press Q |

---

# 🧠 What You Learn

By completing this project, you gain practical experience with:

- 🍓 Raspberry Pi
- 🐍 Python programming
- 📷 Raspberry Pi camera interfacing
- 👁️ Computer vision
- 🧠 Artificial intelligence
- 🤖 Deep neural networks
- 🎯 Object detection
- 📊 Confidence-based detection
- 🟩 Bounding boxes
- 🔌 GPIO programming
- 🔊 Buzzer control
- ⚡ Edge AI

---

# 🌍 Applications

This project can serve as the foundation for:

- 🌾 Smart farm monitoring
- 🐄 Livestock monitoring
- 🌲 Wildlife detection
- 🚨 Animal intrusion alerts
- 🏡 Property monitoring
- 🌱 Crop protection systems
- 📷 Intelligent surveillance
- 🌳 Forest monitoring
- 🤖 Autonomous monitoring systems

---

# 🚀 Future Improvements

The system can be extended with:

- 📱 Telegram / WhatsApp alerts
- 📧 Email notifications
- 📸 Automatic image capture
- 🎥 Detection video recording
- ☁️ Cloud storage
- 🌐 Web dashboard
- 📊 Detection history
- 🕒 Date and time logging
- 🔔 Different alerts for different animals
- 📍 GPS location tracking
- 🌙 Night vision camera
- 🎯 Custom YOLO animal detection model
- 📲 Mobile monitoring
- 💾 Detection database

---

# 📁 Suggested Repository Structure

```text
AI-Animal-Detection/
│
├── README.md
├── index.html
├── LICENSE
│
├── src/
│   └── animal_detection.py
│
├── model/
│   ├── deploy.prototxt
│   └── mobilenet_iter_73000.caffemodel
│
├── images/
│   ├── raspberry-pi.png
│   ├── camera.png
│   ├── buzzer.png
│   ├── jumper-wires.png
│   ├── power-supply.png
│   └── circuit-diagram.png
│
└── docs/
    └── project-documentation.pdf
```

---

# ⚠️ Safety Notes

- Switch OFF the Raspberry Pi before connecting the camera ribbon cable.
- Verify GPIO connections before powering the system.
- Use a suitable Raspberry Pi power supply.
- Avoid short circuits between GPIO pins.
- Ensure the buzzer is connected to the correct GPIO and ground.
- Handle the camera ribbon cable carefully.

---

# 🏁 Project Summary

```text
Camera
   ↓
Raspberry Pi
   ↓
OpenCV
   ↓
MobileNet SSD
   ↓
AI Object Detection
   ↓
Animal Classification
   ↓
Bounding Box + Label
   ↓
GPIO 18
   ↓
Buzzer Alert
```

The project demonstrates how **Artificial Intelligence, Computer Vision and Embedded Systems** can be combined to build a practical real-time animal monitoring system.

---

## 👨‍💻 Developed As

Part of a collection of projects exploring:

**Artificial Intelligence • IoT • Computer Vision • Raspberry Pi • Embedded Systems**

---

## 📜 License

This project is intended for **educational, experimentation and learning purposes**.
