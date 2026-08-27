# 🎨 AI Based Color Sorting System

## Raspberry Pi + Computer Vision + Conveyor + Robot Magician

<p align="center">

![Raspberry Pi](https://img.shields.io/badge/Raspberry%20Pi-4-C51A4A?style=for-the-badge&logo=raspberrypi&logoColor=white)
![Python](https://img.shields.io/badge/Python-3-3776AB?style=for-the-badge&logo=python&logoColor=white)
![OpenCV](https://img.shields.io/badge/OpenCV-Computer%20Vision-5C3EE8?style=for-the-badge&logo=opencv&logoColor=white)
![Robot](https://img.shields.io/badge/Robot-Magician-2563EB?style=for-the-badge)
![Automation](https://img.shields.io/badge/Industrial-Automation-16A34A?style=for-the-badge)

</p>

---

# 📌 Project Overview

The **AI Based Color Sorting System** is an automated machine vision and robotic sorting project developed using a **Raspberry Pi 4, USB Camera, OpenCV, conveyor system, E18-D80NK proxy sensor and Robot Magician**.

The system automatically detects the color of an object and sorts it into the appropriate bin.

The project currently identifies:

- 🔴 Red
- 🔵 Blue
- 🟡 Yellow

The USB camera detects the object's color using **OpenCV HSV image processing**.

Once a valid color is detected:

1. The conveyor starts.
2. The object moves toward the robot pickup position.
3. The proxy sensor detects the object.
4. The conveyor stops.
5. Robot Magician picks the object.
6. The robot moves to the corresponding color bin.
7. Vacuum suction releases the object.
8. The system returns to its IDLE state.

---

# 🎯 Aim

To develop an automated color sorting system that combines:

**Computer Vision + Sensors + Conveyor Automation + Robotics**

for automatically identifying and sorting colored objects.

---

# ✨ Key Features

| Feature | Description |
|---|---|
| 🎨 Color Detection | Detects Red, Blue and Yellow objects |
| 📷 Machine Vision | USB camera continuously monitors objects |
| 👁️ OpenCV | Performs real-time image processing |
| 🌈 HSV Processing | Uses HSV color ranges for classification |
| 🧹 Noise Filtering | Morphological operations reduce image noise |
| 🏭 Conveyor Control | Automatically starts and stops conveyor |
| 📡 Proxy Detection | Detects object at robot pickup position |
| 🤖 Robotic Sorting | Robot Magician performs pick-and-place |
| 💨 Vacuum Pickup | Uses suction for handling objects |
| 📊 Live Dashboard | Displays counts, detected color and state |
| ⚠️ Safety Timeout | Stops conveyor if proxy detection fails |
| 💾 Calibration Storage | Robot coordinates are stored for reuse |

---

# 🧰 Hardware Components

| Component | Qty | Purpose | Specification |
|---|---:|---|---|
| Raspberry Pi 4 | 1 | Main controller | 4 GB RAM |
| Robot Magician | 1 | Pick-and-place robot | Vacuum suction |
| USB Camera | 1 | Capture object images | 640 × 480 or higher |
| E18-D80NK Proxy Sensor | 1 | Detect object at pickup position | NPN, 5–24 V |
| Relay Module | 1 | Start/Stop conveyor | 1 Channel |
| PWM DC Motor Controller | 1 | Conveyor speed control | 12 V DC |
| DC Conveyor Motor | 1 | Move objects | 12 V |
| 12 V Power Supply | 1 | Power conveyor | 12 V DC |
| Sorting Bins | 3 | Collect sorted objects | Red, Blue, Yellow |

---

# 🔌 Hardware Connections

| Connection | Details |
|---|---|
| Relay VCC | Raspberry Pi 3.3 V |
| Relay GND | Raspberry Pi GND |
| Relay Signal | GPIO 17 |
| Proxy Sensor Signal | GPIO 23 |
| Proxy Sensor VCC | Raspberry Pi 5 V |
| Proxy Sensor GND | Raspberry Pi GND |
| USB Camera | Raspberry Pi USB Port |
| Robot Magician USB | Raspberry Pi USB Port |
| Relay COM | PWM Controller Power (+) |
| Relay NC | 12 V Positive Supply |
| PWM Output | DC Conveyor Motor |

---

# ⚠️ Important Wiring Notes

Complete all wiring before powering ON the system.

Make sure that:

- Raspberry Pi GPIO connections are correct.
- USB connections are secure.
- 12 V supply polarity is correct.
- Relay wiring is checked.
- Proxy sensor signal is connected to GPIO 23.
- Conveyor motor is connected through the PWM controller.
- Robot Magician is connected through USB.

---

# 🧠 System Architecture

```text
                   USB Camera
                       │
                       ▼
                Raspberry Pi 4
                       │
              OpenCV Color Detection
                       │
              Red / Blue / Yellow
                       │
                       ▼
                 Relay GPIO 17
                       │
                       ▼
                 Conveyor Motor
                       │
                       ▼
                 Moving Object
                       │
                       ▼
              E18-D80NK Sensor
                       │
                   GPIO 23
                       │
                       ▼
                Conveyor Stops
                       │
                       ▼
                Robot Magician
                       │
                 Vacuum Pickup
                       │
                       ▼
              Correct Color Bin
```

---

# 🎨 Color Detection

The project uses **HSV color space** instead of directly detecting colors from the normal BGR image.

HSV stands for:

```text
H = Hue
S = Saturation
V = Value
```

HSV makes it easier to separate different colors under changing lighting conditions.

---

# 🔴 Red Detection Range

Red requires two HSV ranges because red appears at both ends of the HSV Hue scale.

```python
LOWER_RED1 = np.array([0, 120, 80])
UPPER_RED1 = np.array([10, 255, 255])

LOWER_RED2 = np.array([170, 120, 80])
UPPER_RED2 = np.array([180, 255, 255])
```

---

# 🔵 Blue Detection Range

```python
LOWER_BLUE = np.array([90, 90, 70])
UPPER_BLUE = np.array([135, 255, 255])
```

---

# 🟡 Yellow Detection Range

```python
LOWER_YELLOW = np.array([22, 110, 100])
UPPER_YELLOW = np.array([40, 255, 255])
```

The yellow range is tightened to reduce incorrect detection caused by hands and light reflections.

---

# 📷 Image Processing Sequence

The color detection process follows:

```text
USB Camera
    ↓
Capture Image
    ↓
Select Region of Interest (ROI)
    ↓
Convert BGR → HSV
    ↓
Create Color Masks
    ↓
Remove Bright Reflections
    ↓
Morphological Opening
    ↓
Morphological Closing
    ↓
Find Contours
    ↓
Check Contour Area
    ↓
Check Detection Threshold
    ↓
Determine Best Color
```

---

# 🧹 Noise Reduction

The program performs morphological opening and closing:

```python
kernel = np.ones((5, 5), np.uint8)

mask = cv2.morphologyEx(
    mask,
    cv2.MORPH_OPEN,
    kernel
)

mask = cv2.morphologyEx(
    mask,
    cv2.MORPH_CLOSE,
    kernel
)
```

This helps remove small unwanted pixels and improves the stability of color detection.

---

# 🎯 Detection Threshold

The project uses:

```python
DETECTION_THRESHOLD = 1500
MIN_CONTOUR_AREA = 1200
STABLE_FRAMES_REQUIRED = 5
```

This means the system does not immediately accept every detected color.

The color must satisfy the required mask/contour conditions and remain stable for multiple frames before the conveyor starts.

---

# 🔄 Complete Working Sequence

```text
SYSTEM START
     ↓
Camera Starts
     ↓
Waiting for Object
     ↓
Color Detected
     ↓
Stable for Required Frames?
     ↓
    YES
     ↓
Identify Color
     ↓
Update Object Counter
     ↓
Conveyor ON
     ↓
Object Moves Forward
     ↓
Proxy Sensor Detects Object
     ↓
Conveyor OFF
     ↓
Robot Moves to PICK Position
     ↓
Vacuum Suction ON
     ↓
Robot Picks Object
     ↓
Color Bin Selected
     ↓
Robot Moves to Bin
     ↓
Vacuum Suction OFF
     ↓
Object Released
     ↓
Robot Returns
     ↓
Wait for Previous Object to Leave
     ↓
System Returns to IDLE
```

---

# 🚦 System States

The program operates using three main states.

## 🟢 IDLE

The system waits for a stable color detection.

```text
Camera Monitoring
Conveyor OFF
Waiting for Red / Blue / Yellow
```

---

## 🔵 RUN

Once a valid object is detected:

```text
Conveyor ON
Object Moving
Waiting for Proxy Sensor
```

---

## 🟡 WAIT

After the robot completes sorting:

```text
Robot Operation Complete
Conveyor OFF
Waiting for Previous Object to Leave Camera
```

After the object disappears from the camera for the required number of frames, the system returns to:

```text
IDLE
```

---

# 📡 E18-D80NK Proxy Sensor

The project uses an:

**E18-D80NK NPN Proximity Sensor**

The default program configuration is:

```python
PROXY_PIN = 23
PROXY_ACTIVE_LOW = True
```

Therefore:

```text
Object Not Detected → HIGH
Object Detected     → LOW
```

The sensor is used to confirm that the object has reached the robot pickup position.

---

# 📡 Stable Proxy Detection

Instead of accepting only one sensor reading, the program checks the sensor multiple times.

```python
PROXY_STABLE_READS = 3
PROXY_STABLE_DELAY = 0.02
```

This reduces false triggers caused by sensor noise.

---

# 🏭 Conveyor Control

The conveyor relay is connected to:

```text
GPIO 17
```

Program configuration:

```python
RELAY_PIN = 17
```

The conveyor starts using:

```python
GPIO.output(RELAY_PIN, GPIO.HIGH)
```

and stops using:

```python
GPIO.output(RELAY_PIN, GPIO.LOW)
```

---

# ⚠️ Conveyor Safety Timeout

The program contains a safety cutoff:

```python
CONVEYOR_MAX = 10.0
```

If the proxy sensor fails to detect the object within approximately:

```text
10 seconds
```

the conveyor automatically stops.

This prevents the conveyor from running continuously if there is a sensor or wiring problem.

---

# 🤖 Robot Magician

Robot Magician performs the pick-and-place operation.

The robot connects to the Raspberry Pi through USB.

The program searches:

```text
/dev/ttyUSB*
/dev/ttyACM*
```

to locate the robot connection.

---

# 📍 Robot Calibration

The robot must know four positions:

```text
PICK
BIN_RED
BIN_BLUE
BIN_YELLOW
```

These coordinates are stored in:

```text
dobot_coords.json
```

---

# 🎯 Calibration Positions

| Position | Purpose |
|---|---|
| PICK | Conveyor pickup location |
| BIN_RED | Red object placement |
| BIN_BLUE | Blue object placement |
| BIN_YELLOW | Yellow object placement |

---

# ⚙️ Robot Calibration Procedure

### Step 1

Open the project folder.

### Step 2

Activate the virtual environment:

```bash
source myenv/bin/activate
```

### Step 3

Run calibration mode:

```bash
python3 color_sor.py --calibrate
```

### Step 4

When the program asks for:

```text
PICK
```

manually move Robot Magician to the pickup position.

Press:

```text
ENTER
```

### Step 5

Repeat for:

```text
BIN_RED
```

### Step 6

Repeat for:

```text
BIN_BLUE
```

### Step 7

Repeat for:

```text
BIN_YELLOW
```

### Step 8

The coordinates are automatically saved in:

```text
dobot_coords.json
```

---

# 💾 Calibration File

Example:

```json
{
    "PICK": {
        "x": 200.0,
        "y": 0.0,
        "z": -30.0,
        "r": 0.0
    },

    "BIN_RED": {
        "x": 180.0,
        "y": 100.0,
        "z": -30.0,
        "r": 0.0
    },

    "BIN_BLUE": {
        "x": 180.0,
        "y": 0.0,
        "z": -30.0,
        "r": 0.0
    },

    "BIN_YELLOW": {
        "x": 180.0,
        "y": -100.0,
        "z": -30.0,
        "r": 0.0
    }
}
```

> The values above are only an example. Use the coordinates generated from your own physical setup.

---

# 🔄 When Should You Recalibrate?

Recalibrate the robot if:

- 🤖 Robot Magician is moved
- 🏭 Conveyor position changes
- 📦 Sorting bins are moved
- 🎯 Pickup position changes
- 🦾 Robot is not picking accurately
- 📍 Robot is placing objects incorrectly

---

# 🔁 Force Recalibration

Run:

```bash
source myenv/bin/activate
python3 color_sor.py --calibrate
```

You can also remove the old:

```text
dobot_coords.json
```

and run the program again to teach new coordinates.

---

# 💻 Software Requirements

| Software / Library | Purpose |
|---|---|
| Raspberry Pi OS | Operating system |
| Python 3 | Main programming language |
| OpenCV | Image processing |
| NumPy | Numerical and array processing |
| RPi.GPIO | Raspberry Pi GPIO control |
| pydobot | Robot Magician communication |
| pyserial | Serial communication |

---

# 🛠 Software Installation

Navigate to your project folder:

```bash
cd ~/project_folder
```

Activate the virtual environment:

```bash
source myenv/bin/activate
```

Install the required libraries:

```bash
pip install opencv-python numpy pydobot pyserial RPi.GPIO
```

---

# ▶️ Running the Project

Activate the environment:

```bash
source myenv/bin/activate
```

Run:

```bash
python3 color_sor.py
```

---

# 📊 Live Dashboard

The OpenCV window displays useful system information such as:

```text
No. of Blocks

🔴 Red       0
🔵 Blue      0
🟡 Yellow    0

State: IDLE
Color: None
```

As objects are detected and sorted, the counters increase automatically.

---

# 🔢 Object Counting

The system maintains separate counters:

```python
red_count = 0
blue_count = 0
yellow_count = 0
```

For example:

```text
Red    = 5
Blue   = 3
Yellow = 4
```

This allows the operator to monitor the total number of sorted objects.

---

# 🤖 Pick-and-Place Sequence

When an object reaches the pickup location:

```text
Proxy Sensor Triggered
        ↓
Conveyor OFF
        ↓
Robot Moves Above PICK
        ↓
Robot Moves Down
        ↓
Vacuum ON
        ↓
Object Picked
        ↓
Robot Moves Up
        ↓
Correct Bin Selected
        ↓
Robot Moves to Bin
        ↓
Robot Moves Down
        ↓
Vacuum OFF
        ↓
Object Released
        ↓
Robot Returns
```

---

# 🎨 Color-to-Bin Mapping

```python
bin_map = {
    "Red": "BIN_RED",
    "Blue": "BIN_BLUE",
    "Yellow": "BIN_YELLOW"
}
```

Therefore:

```text
🔴 Red    → BIN_RED
🔵 Blue   → BIN_BLUE
🟡 Yellow → BIN_YELLOW
```

---

# 🛑 Stopping the Program

There are two ways to stop the project.

### Method 1

Click the OpenCV camera window and press:

```text
Q
```

### Method 2

From the terminal press:

```text
Ctrl + C
```

The program performs cleanup before exiting.

---

# 🔧 Important Program Settings

| Parameter | Value |
|---|---|
| Relay GPIO | GPIO 17 |
| Proxy GPIO | GPIO 23 |
| Proxy Logic | Active LOW |
| Conveyor Safety Timeout | 10 seconds |
| Detection Threshold | 1500 |
| Minimum Contour Area | 1200 |
| Stable Detection Frames | 5 |
| Gone Frames | 8 |
| Camera Resolution | 640 × 480 |
| Colors | Red, Blue, Yellow |
| Calibration File | `dobot_coords.json` |

---

# ⚠️ Troubleshooting

| Problem | Possible Cause | Solution |
|---|---|---|
| Camera not opening | Wrong camera index | Check USB camera and camera index |
| Color not detected | Incorrect lighting | Improve lighting |
| Wrong color detected | HSV range mismatch | Adjust HSV limits |
| Yellow false detection | Reflection or hand detected | Adjust lighting / HSV threshold |
| Conveyor not starting | Relay wiring issue | Check GPIO 17 and relay |
| Conveyor doesn't stop | Proxy sensor issue | Check GPIO 23 and sensor |
| Proxy always detected | Wrong active logic | Check Active LOW configuration |
| Robot not detected | USB connection problem | Check `/dev/ttyUSB*` or `/dev/ttyACM*` |
| Robot misses object | Calibration changed | Recalibrate PICK position |
| Wrong bin placement | Bin moved | Recalibrate bin coordinates |
| Conveyor stops after 10 sec | Proxy did not detect object | Check sensor position and wiring |

---

# 💡 Tips for Reliable Color Detection

For better results:

- Use consistent lighting.
- Avoid strong reflections.
- Keep the camera fixed.
- Keep the conveyor position fixed.
- Use clearly colored objects.
- Avoid backgrounds with similar colors.
- Keep the Region of Interest clear.
- Adjust HSV ranges if lighting changes significantly.

---

# 🧪 Expected Result

When the system is operating correctly:

- ✅ USB camera detects the object
- ✅ OpenCV identifies its color
- ✅ Dashboard updates object count
- ✅ Conveyor starts automatically
- ✅ Object moves toward pickup point
- ✅ Proxy sensor detects the object
- ✅ Conveyor stops
- ✅ Robot Magician moves to PICK
- ✅ Vacuum suction picks the object
- ✅ Correct sorting bin is selected
- ✅ Robot places the object
- ✅ System waits for the object to clear
- ✅ System returns to IDLE
- ✅ Next object can be processed

---

# 🧠 What You Learn

This project provides practical experience with:

- 🍓 Raspberry Pi programming
- 🐍 Python
- 👁️ OpenCV
- 📷 Machine vision
- 🌈 HSV color detection
- 🎯 Contour detection
- 🧹 Image filtering
- 🔌 GPIO control
- 📡 Proximity sensors
- ⚡ Relay control
- 🏭 Conveyor automation
- 🤖 Robot programming
- 📍 Robot coordinate teaching
- 💨 Vacuum pick-and-place
- 💾 JSON configuration files
- 🚦 State-machine logic
- 🏗️ Industrial automation integration

---

# 🌍 Applications

The same concept can be used for:

- 🏭 Manufacturing
- 📦 Packaging
- 🚚 Warehouse automation
- ♻️ Automated material sorting
- 🔍 Product classification
- 🧪 Quality inspection
- 🥫 Food and beverage sorting
- 🤖 Robotic pick-and-place
- 🎓 Robotics laboratories
- 🏗️ Industrial automation training

---

# 🚀 Future Improvements

The system can be expanded with:

- 🧠 AI object classification
- 📷 Industrial machine vision camera
- 💡 Controlled industrial lighting
- 🏷️ Shape detection
- 📏 Size detection
- 🔢 QR/Barcode scanning
- 📊 Production dashboard
- 💾 Database logging
- 🌐 Web-based monitoring
- 📱 Mobile dashboard
- 📧 Fault notifications
- 🚨 Emergency stop integration
- 🏭 PLC integration
- 📡 Industrial sensors
- 🔄 Continuous production mode
- 📈 Production statistics
- ❌ Rejection system
- 🔍 Quality inspection

---

# 📁 Suggested Repository Structure

```text
AI-Color-Sorting-System/
│
├── README.md
├── index.html
├── color_sor.py
├── dobot_coords.json
│
├── images/
│   ├── Colour sorting system.jpg
│   ├── system-overview.jpg
│   ├── calibration.jpg
│   └── dashboard.jpg
│
└── docs/
    └── color_sorting_sop.pdf
```

---

# 🔐 Important Safety Notes

- Complete all wiring before switching ON the system.
- Verify the 12 V supply polarity.
- Keep hands away from the robot during automatic operation.
- Keep hands away from the moving conveyor.
- Check the robot workspace before running the program.
- Secure the camera so its position does not change.
- Check the proxy sensor position before operation.
- Do not move the robot or bins after calibration without recalibrating.
- Use the conveyor safety timeout.
- Stop the system immediately if the robot moves unexpectedly.

---

# 🏁 Project Summary

```text
USB Camera
     ↓
Raspberry Pi 4
     ↓
OpenCV
     ↓
HSV Color Detection
     ↓
Red / Blue / Yellow
     ↓
GPIO 17 Relay
     ↓
Conveyor Starts
     ↓
E18-D80NK Proxy Sensor
     ↓
GPIO 23 Detection
     ↓
Conveyor Stops
     ↓
Robot Magician
     ↓
Vacuum Pick
     ↓
Color-Based Bin Selection
     ↓
Object Placement
     ↓
System Returns to IDLE
```

The **AI Based Color Sorting System** demonstrates the integration of **Computer Vision, Raspberry Pi, Sensors, Conveyor Automation and Industrial Robotics** into a complete automated sorting application.

---

## 👨‍💻 Developed As

Part of a collection of projects exploring:

**Artificial Intelligence • Computer Vision • Raspberry Pi • Robotics • IoT • Industrial Automation**

---

## 📜 License

This project is intended for **educational, experimentation and learning purposes**.
