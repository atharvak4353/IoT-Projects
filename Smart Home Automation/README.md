# 🏠 Gesture Controlled Smart Home Automation

## Raspberry Pi Based Touch-Free Appliance Control using Computer Vision

<p align="center">

![Raspberry Pi](https://img.shields.io/badge/Raspberry%20Pi-Smart%20Home-C51A4A?style=for-the-badge&logo=raspberrypi&logoColor=white)
![Python](https://img.shields.io/badge/Python-3.10-3776AB?style=for-the-badge&logo=python&logoColor=white)
![OpenCV](https://img.shields.io/badge/OpenCV-Computer%20Vision-5C3EE8?style=for-the-badge&logo=opencv&logoColor=white)
![GPIO](https://img.shields.io/badge/GPIO-Relay%20Control-2563EB?style=for-the-badge)
![Automation](https://img.shields.io/badge/Home-Automation-16A34A?style=for-the-badge)

</p>

---

# 📌 Project Overview

The **Gesture Controlled Smart Home Automation** project uses a **Raspberry Pi and computer vision** to control multiple electrical devices using hand gestures.

A USB camera continuously captures the user's hand.

The software detects the number of raised fingers and maps each gesture to a specific appliance.

The Raspberry Pi then controls a **4-channel relay module** through GPIO pins.

The current system can control:

- 💡 LED
- 🌀 Fan
- 💧 Water Pump
- 🔊 Buzzer

The system is completely touch-free and demonstrates how **computer vision, embedded systems and home automation** can be integrated into one practical project.

---

# 🎯 Aim

To build a smart home automation system that uses hand gesture recognition to control multiple appliances through a Raspberry Pi and relay module without using physical switches.

---

# ✨ Key Features

| Feature | Description |
|---|---|
| ✋ Gesture Control | Appliances are controlled using finger gestures |
| 📷 Real-Time Camera | USB camera continuously captures the hand |
| 👁️ Computer Vision | OpenCV processes live camera frames |
| 🧠 Hand Tracking | Raised fingers are detected in real time |
| ⚡ Relay Control | Raspberry Pi controls a 4-channel relay |
| 🏠 Multiple Appliances | LED, fan, pump and buzzer can be controlled |
| 🛑 Emergency OFF | Five fingers turn all devices OFF |
| ⏱ Gesture Delay | Prevents accidental repeated switching |
| 🧹 Safe Cleanup | GPIO and camera are released properly on exit |

---

# 🧰 Hardware Components

| Component | Quantity | Purpose |
|---|---:|---|
| Raspberry Pi | 1 | Main controller |
| USB Camera | 1 | Captures hand gestures |
| 4-Channel Relay Module | 1 | Controls connected appliances |
| Jumper Wires | As required | GPIO connections |
| Raspberry Pi Power Supply | 1 | Powers the Raspberry Pi |
| 5V Fan | 1 | Demonstration appliance |
| LEDs | As required | Demonstration load |
| Water Pump | 1 | Demonstration appliance |
| Buzzer | 1 | Demonstration appliance |
| Breadboard | 1 | Prototype connections |

---

# 🔌 GPIO & Relay Connections

| Relay Channel | Raspberry Pi GPIO | Connected Device |
|---|---|---|
| Relay 1 | GPIO 17 | LED |
| Relay 2 | GPIO 27 | Fan |
| Relay 3 | GPIO 22 | Water Pump |
| Relay 4 | GPIO 23 | Buzzer |

Relay module power:

```text
Relay VCC → Raspberry Pi 5V
Relay GND → Raspberry Pi GND
```

USB camera:

```text
USB Camera → Raspberry Pi USB Port
```

---

# ⚠️ Relay Logic

The project assumes an:

```text
Active-LOW Relay Module
```

This means:

```text
GPIO LOW  → Relay ON
GPIO HIGH → Relay OFF
```

In the program:

```python
RELAY_ON = GPIO.LOW
RELAY_OFF = GPIO.HIGH
```

---

# 🧠 System Architecture

```text
               USB Camera
                   │
                   ▼
             Raspberry Pi
                   │
             OpenCV Frames
                   │
                   ▼
             Hand Detector
                   │
             Finger Count
                   │
                   ▼
         Gesture Decision Logic
                   │
       ┌───────────┼───────────┐
       ▼           ▼           ▼
     GPIO17      GPIO27      GPIO22
       │           │           │
      LED         Fan        Pump
                   │
                 GPIO23
                   │
                 Buzzer
```

---

# ✋ Gesture Mapping

| Gesture | Action |
|---|---|
| ☝️ 1 Finger | Toggle LED |
| ✌️ 2 Fingers | Toggle Fan |
| 🤟 3 Fingers | Toggle Water Pump |
| 🖐️ 4 Fingers | Toggle Buzzer |
| ✋ 5 Fingers | Turn ALL devices OFF |

---

# 🔄 Complete Working Sequence

```text
System Starts
      ↓
USB Camera Opens
      ↓
Hand is Detected
      ↓
Raised Fingers are Counted
      ↓
Gesture is Identified
      ↓
1 Finger → Toggle LED
2 Fingers → Toggle Fan
3 Fingers → Toggle Pump
4 Fingers → Toggle Buzzer
5 Fingers → All Devices OFF
      ↓
Relay State Changes
      ↓
Appliance State Changes
      ↓
System Continues Monitoring
```

---

# 🧠 Gesture Debouncing

The program uses a delay to avoid repeated toggling when the same gesture remains visible.

```python
GESTURE_DELAY = 1.0
```

This means the system waits approximately:

```text
1 second
```

before allowing another gesture action.

---

# 📷 Camera Configuration

The USB camera is configured to:

```text
Resolution = 640 × 480
```

using:

```python
camera.set(
    cv2.CAP_PROP_FRAME_WIDTH,
    640
)

camera.set(
    cv2.CAP_PROP_FRAME_HEIGHT,
    480
)
```

The image is also horizontally flipped:

```python
frame = cv2.flip(
    frame,
    1
)
```

This creates a mirror-like interaction for the user.

---

# 🧠 Hand Detection

The project uses:

```python
from cvzone.HandTrackingModule import HandDetector
```

The detector is configured as:

```python
detector = HandDetector(
    detectionCon=0.8,
    maxHands=1
)
```

This means:

```text
Detection Confidence = 80%
Maximum Hands = 1
```

---

# 💻 Software Requirements

| Software / Library | Purpose |
|---|---|
| Python 3.10 | Main programming language |
| OpenCV | Camera capture and image processing |
| Hand Tracking | Detects hand and finger state |
| RPi.GPIO | Controls Raspberry Pi GPIO |
| GPIO Zero | GPIO support |
| Virtual Environment | Isolates project dependencies |

---

# 🛠 Installation & Setup

## Step 1 — Update Raspberry Pi

```bash
sudo apt update
```

---

## Step 2 — Install Build Dependencies

```bash
sudo apt install -y build-essential libssl-dev zlib1g-dev \
libncurses5-dev libncursesw5-dev libreadline-dev libsqlite3-dev \
libgdbm-dev libdb5.3-dev libbz2-dev libexpat1-dev liblzma-dev \
tk-dev libffi-dev uuid-dev wget
```

---

## Step 3 — Download Python 3.10.13

```bash
cd /usr/src
```

```bash
sudo wget https://www.python.org/ftp/python/3.10.13/Python-3.10.13.tgz
```

```bash
sudo tar -xf Python-3.10.13.tgz
```

```bash
cd Python-3.10.13
```

---

## Step 4 — Compile Python

```bash
sudo ./configure --enable-optimizations
```

```bash
sudo make -j4
```

```bash
sudo make altinstall
```

---

## Step 5 — Verify Python

```bash
python3.10 --version
```

Expected:

```text
Python 3.10.x
```

---

# 📦 Virtual Environment

Create the project environment:

```bash
cd
```

```bash
python3.10 -m venv mp-env
```

Activate it:

```bash
source mp-env/bin/activate
```

---

# 👁️ Install Computer Vision Packages

Install one at a time:

```bash
pip install mediapipe
```

```bash
pip install opencv-python==4.7.0.72
```

```bash
pip install tflite-runtime
```

---

# 🔌 Install GPIO Libraries

```bash
pip install gpiozero
```

```bash
pip install RPi.GPIO
```

---

# 🔊 Additional Audio Packages

Your project setup also includes audio-related packages:

```bash
sudo apt install python3-pyaudio
```

```bash
pip3 install SpeechRecognition
```

```bash
sudo apt install portaudio19-dev python3-pyaudio -y
```

```bash
pip3 install SpeechRecognition pyaudio
```

```bash
sudo apt install flac -y
```

---

# 🎤 Audio Card Configuration

Open:

```bash
sudo nano ~/.asoundrc
```

Add:

```text
defaults.pcm.card 1
defaults.ctl.card 1
```

Then reboot:

```bash
reboot
```

After reboot:

```bash
source mp-env/bin/activate
```

---

# ▶️ Running the Project

Activate the virtual environment:

```bash
source mp-env/bin/activate
```

Navigate to your project directory and run:

```bash
python3 smart_home.py
```

---

# 🖥️ Camera Dashboard

The OpenCV window displays:

```text
SMART HOME AUTOMATION

Fingers: 2
FAN ON

Press Q to exit
```

The status changes depending on the detected gesture.

---

# ⚙️ Appliance Control Logic

## 1 Finger

```text
LED Toggle
```

Example:

```text
LED ON
```

or:

```text
LED OFF
```

---

## 2 Fingers

```text
Fan Toggle
```

---

## 3 Fingers

```text
Water Pump Toggle
```

---

## 4 Fingers

```text
Buzzer Toggle
```

---

## 5 Fingers

```text
ALL DEVICES OFF
```

This is useful as a simple emergency or reset gesture.

---

# 🧹 Safe Exit

Press:

```text
Q
```

to exit the program.

The program then:

```text
Turns all devices OFF
        ↓
Releases camera
        ↓
Closes OpenCV windows
        ↓
Cleans GPIO
```

The cleanup code ensures the relays are left in a safe state.

---

# ⚡ Relay Functions

The program includes a reusable relay function:

```python
def set_relay(
    relay_pin,
    state
):
```

and a toggle function:

```python
def toggle_relay(
    relay_pin
):
```

This makes the program easier to expand with more devices or gestures.

---

# 🧪 Expected Result

When the system is working correctly:

- ✅ USB camera starts
- ✅ Hand is detected
- ✅ Raised fingers are counted
- ✅ One finger controls LED
- ✅ Two fingers control fan
- ✅ Three fingers control water pump
- ✅ Four fingers control buzzer
- ✅ Five fingers switch everything OFF
- ✅ Device status appears on screen
- ✅ GPIO controls the relay module
- ✅ Appliances respond in real time

---

# 🔧 Troubleshooting

| Problem | Possible Cause | Solution |
|---|---|---|
| Camera not opening | Wrong camera index | Check USB camera and `VideoCapture(0)` |
| Hand not detected | Poor lighting | Improve lighting |
| Finger count incorrect | Hand not clearly visible | Keep hand open and facing camera |
| Relay not switching | GPIO wiring issue | Check GPIO connections |
| Device remains ON | Active relay logic incorrect | Verify active-LOW relay |
| OpenCV error | Missing package | Reinstall OpenCV |
| Hand tracking error | Missing package | Check required hand-tracking library |
| GPIO permission error | GPIO setup issue | Verify Raspberry Pi GPIO environment |
| Rapid repeated switching | Gesture delay too low | Increase `GESTURE_DELAY` |
| Wrong device toggles | Incorrect relay wiring | Check GPIO-to-relay mapping |

---

# 💡 Tips for Better Gesture Detection

For reliable operation:

- Use good lighting.
- Avoid very dark backgrounds.
- Keep your hand clearly visible.
- Use only one hand.
- Keep the camera fixed.
- Keep fingers separated.
- Avoid moving your hand too quickly.
- Keep the hand within the center of the camera frame.

---

# ⚠️ Electrical Safety

Test the project first using:

```text
Low-voltage DC loads
```

such as:

- LEDs
- Small 5V fans
- Low-voltage pumps
- Buzzers

Do not connect mains-voltage appliances unless the entire system has been designed with proper:

- Relay ratings
- Enclosure
- Insulation
- Fusing
- Electrical protection
- Qualified supervision

---

# 🧠 What You Learn

This project provides practical experience with:

- 🍓 Raspberry Pi
- 🐍 Python
- 👁️ OpenCV
- ✋ Hand tracking
- 🧠 Computer vision
- 🔌 GPIO programming
- ⚡ Relay interfacing
- 🏠 Home automation
- 🔄 Toggle logic
- ⏱ Gesture debouncing
- 📷 Real-time camera processing
- 🤖 Human-machine interaction

---

# 🌍 Applications

This concept can be expanded for:

- 🏠 Smart home systems
- 🏥 Touch-free hospital controls
- 🏢 Office automation
- 🏭 Industrial operator interfaces
- ♿ Accessibility systems
- 🧪 Laboratory automation
- 🤖 Robot control
- 🖥️ Human-machine interfaces
- 🏫 Educational automation systems
- 🚪 Touchless room control

---

# 🚀 Future Improvements

The project can be expanded with:

- 🎤 Voice control
- 📱 Mobile control
- 🌐 Web dashboard
- 📡 Wi-Fi remote control
- ☁️ IoT cloud integration
- 🧠 Custom ML gesture recognition
- 🖐️ More gesture combinations
- 💾 Device state logging
- 🔐 User authentication
- 📊 Appliance usage monitoring
- ⚡ Energy monitoring
- 🗣 Voice feedback
- 🏠 Home Assistant integration
- 🔔 Smart notifications

---

# 📁 Suggested Repository Structure

```text
Gesture-Smart-Home-Automation/
│
├── README.md
├── index.html
├── LICENSE
│
├── src/
│   └── smart_home.py
│
├── images/
│   ├── Raspberry PI.png
│   ├── USB camera.png
│   ├── 4 channel relay.png
│   ├── Jumper Wires.png
│   ├── Power supply.png
│   ├── 5V fan.png
│   ├── 2 Led.png
│   ├── Motor pump.png
│   ├── Breadboard.png
│   └── SHA.png
│
└── docs/
    └── project-documentation.pdf
```

---

# 🏁 Project Summary

```text
Hand Gesture
      ↓
USB Camera
      ↓
Raspberry Pi
      ↓
OpenCV + Hand Tracking
      ↓
Finger Count
      ↓
Gesture Logic
      ↓
GPIO
      ↓
4-Channel Relay
      ↓
LED / Fan / Pump / Buzzer
```

The **Gesture Controlled Smart Home Automation** project demonstrates how **Computer Vision, Raspberry Pi GPIO, Python and relay-based control** can be combined to create a touch-free smart home interface.

---

## 👨‍💻 Developed As

Part of a collection of projects exploring:

**Raspberry Pi • Computer Vision • IoT • Embedded Systems • Smart Home Automation**

---

## 📜 License

This project is intended for **educational, experimentation and learning purposes**.
