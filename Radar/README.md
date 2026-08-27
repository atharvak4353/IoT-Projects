# 📡 Arduino UNO Radar System

## Ultrasonic Object Detection with Servo Scanning & Processing Visualization

<p align="center">

![Arduino](https://img.shields.io/badge/Arduino-UNO-00979D?style=for-the-badge&logo=arduino&logoColor=white)
![Ultrasonic](https://img.shields.io/badge/Sensor-HC--SR04-2563EB?style=for-the-badge)
![Servo](https://img.shields.io/badge/Servo-MG995-F59E0B?style=for-the-badge)
![Processing](https://img.shields.io/badge/Processing-Visualization-16A34A?style=for-the-badge)
![Serial](https://img.shields.io/badge/Communication-Serial-7C3AED?style=for-the-badge)

</p>

---

# 📌 Project Overview

The **Arduino UNO Radar System** is a real-time ultrasonic object detection and visualization project.

It uses:

- 🔵 Arduino UNO
- 📡 HC-SR04 Ultrasonic Sensor
- ⚙️ MG995 Servo Motor
- 💡 LED
- 🔊 Buzzer
- 💻 Processing IDE

The HC-SR04 ultrasonic sensor is mounted on the servo motor.

The servo continuously rotates the sensor between:

```text
15° and 165°
```

At each angle, Arduino measures the distance of nearby objects.

The measured:

```text
Angle + Distance
```

data is transmitted to the computer using serial communication.

Processing IDE receives this data and creates a real-time **radar-style graphical display**.

---

# 🎯 Aim

To develop a radar-style object detection system using Arduino UNO, an ultrasonic sensor and a servo motor, and display the detected object position using a graphical interface in Processing IDE.

---

# ✨ Key Features

| Feature | Description |
|---|---|
| 📡 Ultrasonic Detection | Measures object distance using HC-SR04 |
| ⚙️ Servo Scanning | Sensor scans between 15° and 165° |
| 📊 Real-Time Radar | Processing displays live radar visualization |
| 🔄 Continuous Scanning | Servo repeatedly scans forward and backward |
| 💻 Serial Communication | Arduino sends angle and distance values |
| 💡 LED Alert | LED turns ON for nearby objects |
| 🔊 Buzzer Alert | Audible warning for nearby objects |
| 🎯 Distance Measurement | Displays approximate object distance |
| 📐 Angle Detection | Shows approximate object direction |

---

# 🧰 Hardware Components

| Component | Quantity | Purpose |
|---|---:|---|
| Arduino UNO | 1 | Main controller |
| HC-SR04 Ultrasonic Sensor | 1 | Measures object distance |
| MG995 Servo Motor | 1 | Rotates the ultrasonic sensor |
| LED | 1 | Visual warning |
| 220Ω Resistor | 1 | LED current limiting |
| Buzzer | 1 | Audible warning |
| Jumper Wires | As required | Electrical connections |
| Breadboard | 1 | Prototype wiring |
| USB Cable | 1 | Programming and serial communication |

---

# 🔌 Hardware Connections

| Component | Component Pin | Arduino UNO | Function |
|---|---|---|---|
| HC-SR04 | VCC | 5V | Sensor power |
| HC-SR04 | GND | GND | Ground |
| HC-SR04 | Trigger | D10 | Ultrasonic trigger |
| HC-SR04 | Echo | D11 | Echo input |
| MG995 Servo | Signal | D9 | Servo control |
| LED | Anode (+) | D8 | Visual alert |
| LED | Cathode (-) | GND through resistor | Ground |
| Buzzer | Positive (+) | D7 | Audible alert |
| Buzzer | Negative (-) | GND | Ground |

---

# ⚠️ Servo Power Note

The **MG995 servo motor** can draw significantly more current than a small hobby servo.

For reliable operation, use a suitable external regulated power supply for the servo.

Important:

```text
External Servo GND
        ↓
Connect to
        ↓
Arduino GND
```

All components must share a **common ground**.

---

# 🧠 System Architecture

```text
            HC-SR04 Sensor
                  │
                  ▼
             Servo Motor
                  │
        Sweeps 15° to 165°
                  │
                  ▼
             Arduino UNO
                  │
       Measures Object Distance
                  │
                  ▼
       Sends Angle + Distance
                  │
            USB Serial
                  │
                  ▼
            Processing IDE
                  │
                  ▼
        Radar Visualization
                  │
        ┌─────────┴─────────┐
        ▼                   ▼
      LED Alert         Buzzer Alert
```

---

# 🔄 Working Principle

## 1️⃣ Servo Scanning

Arduino rotates the servo motor between:

```text
15° → 165°
```

and then:

```text
165° → 15°
```

The process repeats continuously.

---

## 2️⃣ Ultrasonic Trigger

Arduino sends a short pulse to the HC-SR04 Trigger pin:

```cpp
digitalWrite(trigPin, HIGH);
delayMicroseconds(10);
digitalWrite(trigPin, LOW);
```

---

## 3️⃣ Echo Reception

The ultrasonic pulse travels through the air and reflects from an object.

The HC-SR04 Echo pin remains HIGH for a duration proportional to the total travel time.

Arduino measures this using:

```cpp
pulseIn()
```

---

# 📏 Distance Calculation

The program calculates distance using:

```cpp
distance = duration * 0.034 / 2;
```

Why divide by 2?

```text
Sensor → Object → Sensor
```

The measured time represents the complete round trip.

Therefore:

```text
One-Way Distance
=
Total Travel Distance / 2
```

---

# 📐 Servo Scanning Range

The system scans between:

```cpp
const int minimumAngle = 15;
const int maximumAngle = 165;
```

This creates a scanning range of approximately:

```text
150°
```

---

# 🚨 Alert Distance

The alert threshold is:

```cpp
const int alertDistance = 20;
```

So:

```text
Distance > 20 cm
→ No alert

Distance ≤ 20 cm
→ LED ON
→ Buzzer ON
```

---

# 💡 LED Alert

The LED is connected to:

```text
D8
```

When an object is detected within 20 cm:

```cpp
digitalWrite(
    ledPin,
    HIGH
);
```

Otherwise:

```cpp
digitalWrite(
    ledPin,
    LOW
);
```

---

# 🔊 Buzzer Alert

The buzzer is connected to:

```text
D7
```

When an object is nearby:

```cpp
tone(
    buzzerPin,
    1000
);
```

The buzzer frequency is:

```text
1000 Hz
```

When no nearby object is detected:

```cpp
noTone(
    buzzerPin
);
```

---

# 🔁 Complete Detection Sequence

```text
Arduino Starts
      ↓
Servo Moves to Angle
      ↓
HC-SR04 Sends Ultrasonic Pulse
      ↓
Pulse Reflects from Object
      ↓
Echo Returns
      ↓
Arduino Measures Echo Duration
      ↓
Distance Calculated
      ↓
Distance ≤ 20 cm?
      ↓
YES ─────────────── NO
 ↓                    ↓
LED ON              LED OFF
Buzzer ON           Buzzer OFF
      ↓
Send Angle + Distance
      ↓
Processing Receives Data
      ↓
Radar Display Updates
      ↓
Move Servo to Next Angle
```

---

# 💻 Software Requirements

| Software / Library | Purpose |
|---|---|
| Arduino IDE | Write and upload Arduino code |
| Servo Library | Controls MG995 servo |
| Processing IDE | Radar visualization |
| Processing Serial Library | Receives serial data |
| USB Driver | Arduino serial communication |

---

# 📚 Arduino Library

The project uses:

```cpp
#include <Servo.h>
```

The Servo library is included with the Arduino IDE.

---

# 📡 Serial Communication

Arduino communicates with Processing at:

```text
9600 baud
```

Arduino starts serial communication using:

```cpp
Serial.begin(9600);
```

---

# 📤 Data Format

Arduino sends the radar data using:

```text
angle,distance.
```

Example:

```text
45,18.
```

This means:

```text
Angle    = 45°
Distance = 18 cm
```

Another example:

```text
120,35.
```

means:

```text
Angle    = 120°
Distance = 35 cm
```

---

# 🖥 Processing IDE

Processing receives the angle and distance values from Arduino.

It then creates the radar visualization using:

- Arcs
- Radial angle lines
- Green scanning line
- Red detected-object point
- Angle text
- Distance text
- Object detection warning

---

# 🟢 Radar Display

The radar visualization contains:

```text
Green Arcs
    +
Angle Lines
    +
Green Scanning Line
    +
Red Detected Object
    +
Angle Information
    +
Distance Information
```

---

# 🔴 Object Visualization

If an object is detected within the display range:

```text
0 cm to 100 cm
```

Processing displays the object as a red point.

The point position is calculated based on:

```text
Servo Angle
+
Measured Distance
```

---

# 🛠 Arduino Setup

## Step 1 — Connect Hardware

Make all circuit connections.

---

## Step 2 — Connect Arduino

Connect Arduino UNO to your computer using USB.

---

## Step 3 — Open Arduino IDE

Select:

```text
Tools
→ Board
→ Arduino UNO
```

---

## Step 4 — Select COM Port

Go to:

```text
Tools
→ Port
→ Select Arduino COM Port
```

---

## Step 5 — Upload Arduino Code

Upload:

```text
arduino_radar.ino
```

---

# 🖥 Processing Setup

## Step 1 — Install Processing IDE

Install and open Processing IDE.

---

## Step 2 — Load Processing Program

Open:

```text
arduino_radar_display.pde
```

---

## Step 3 — Check Serial Ports

The Processing program runs:

```java
println(
    Serial.list()
);
```

This displays the available serial ports.

---

## Step 4 — Select Arduino Serial Port

The program initially uses:

```java
Serial.list()[0]
```

If Arduino is not at index `0`, change it.

For example:

```java
Serial.list()[1]
```

or:

```java
Serial.list()[2]
```

---

# ⚠️ Important Serial Note

Before starting Processing:

**Close Arduino Serial Monitor.**

Only one program can normally access the Arduino serial port at a time.

If Serial Monitor is open, Processing may show a:

```text
Serial Port Busy
```

error.

---

# ▶️ How to Run the Complete Project

1. Complete the circuit connections.
2. Connect Arduino UNO to the computer.
3. Open Arduino IDE.
4. Select Arduino UNO.
5. Select the correct COM port.
6. Upload the radar firmware.
7. Confirm the Arduino is working.
8. Close Arduino Serial Monitor.
9. Open Processing IDE.
10. Load the Processing radar program.
11. Run the Processing program.
12. Check the displayed serial-port list.
13. Select the correct Arduino serial port.
14. Run Processing again if needed.
15. Place an object in front of the sensor.
16. Observe the radar display.
17. Move the object around the scanning area.
18. Observe the change in angle and distance.
19. Move the object within 20 cm.
20. Observe the LED and buzzer alert.

---

# 📊 Expected Processing Output

The display shows:

```text
Angle: 90°
Distance: 35 cm
```

If an object enters the alert distance:

```text
Angle: 90°
Distance: 15 cm
Object Detected!
```

---

# 🧪 Expected Result

When the system works correctly:

- ✅ Servo continuously scans
- ✅ HC-SR04 measures distance
- ✅ Arduino sends angle and distance data
- ✅ Processing receives the data
- ✅ Radar visualization updates in real time
- ✅ Object position appears as a red point
- ✅ Angle is displayed
- ✅ Distance is displayed
- ✅ LED turns ON for objects within 20 cm
- ✅ Buzzer sounds for objects within 20 cm

---

# 🔧 Troubleshooting

| Problem | Possible Cause | Solution |
|---|---|---|
| Servo not rotating | Wiring or power issue | Check D9 and servo power |
| Servo shaking | Insufficient current | Use external regulated supply |
| Distance always 400 cm | No echo received | Check Trigger/Echo wiring |
| Wrong distance | Sensor alignment issue | Check sensor position |
| Radar not updating | Serial data not received | Check Arduino USB connection |
| Processing shows no data | Wrong serial port | Check `Serial.list()` |
| Serial port busy | Serial Monitor open | Close Arduino Serial Monitor |
| LED not working | Wrong polarity | Check LED and resistor |
| Buzzer not sounding | Wrong wiring | Check D7 and polarity |
| Unstable distance | Poor object reflection | Use a flat solid object |
| Radar object appears wrong | Incorrect angle/distance data | Verify serial data format |

---

# 💡 Tips for Better Results

For more stable operation:

- Secure the HC-SR04 firmly to the servo.
- Avoid loose jumper wires.
- Use a stable servo power supply.
- Keep all grounds common.
- Use large flat objects for testing.
- Avoid very soft materials that absorb ultrasound.
- Avoid moving the sensor assembly manually while running.
- Keep Arduino Serial Monitor closed when Processing is running.

---

# 🧠 What You Learn

This project provides practical experience with:

- 🔵 Arduino UNO
- 📡 Ultrasonic sensors
- 📏 Distance measurement
- ⚙️ Servo motor control
- 📐 Angle scanning
- 💡 LED control
- 🔊 Buzzer control
- 💻 Serial communication
- 🎨 Processing IDE
- 📊 Real-time visualization
- 🔁 Embedded control loops
- 🔌 Sensor interfacing
- 🧮 Distance calculation

---

# 🌍 Applications

This project can be extended toward:

- 🚗 Obstacle detection
- 🤖 Robot navigation
- 🚘 Parking assistance
- 🛡 Perimeter monitoring
- 📡 Proximity detection
- 🚪 Automatic door sensing
- 🦾 Robotic scanning
- 📚 Educational sensor demonstrations
- 🏭 Industrial object detection
- 🚤 Autonomous vehicle experiments

---

# 🚀 Future Improvements

Possible upgrades include:

- 📡 Multiple ultrasonic sensors
- 📶 Bluetooth communication
- 🌐 Wi-Fi communication
- 📱 Mobile radar display
- ☁️ IoT dashboard
- 💾 Data logging
- 📊 Object tracking
- 🔴 Multiple object visualization
- 📷 Camera integration
- 🤖 Mobile robot integration
- 🧭 360° scanning system
- 📍 Position mapping
- 🧠 AI-based object classification
- ⚡ Faster scanning algorithms

---

# 📁 Suggested Repository Structure

```text
Arduino-Radar-System/
│
├── README.md
├── index.html
├── LICENSE
│
├── firmware/
│   └── arduino_radar.ino
│
├── processing/
│   └── arduino_radar_display.pde
│
├── images/
│   ├── Arduino UNO.png
│   ├── HC-SR04 Sensor.png
│   ├── MG995 Servo Motor.png
│   ├── LED.png
│   ├── Buzzer.png
│   ├── Jumper Wires.png
│   └── Radar.png
│
└── docs/
    └── project-documentation.pdf
```

---

# ⚠️ Safety Notes

- Check all connections before powering the system.
- Do not reverse power connections.
- Use a current-limiting resistor with the LED.
- Use a suitable external supply for the MG995 servo.
- Connect external supply ground to Arduino GND.
- Keep fingers away from the rotating servo assembly.
- Secure the ultrasonic sensor before running the scan.
- Disconnect power before changing circuit wiring.

---

# 🏁 Project Summary

```text
Arduino UNO
     ↓
Servo Motor
     ↓
HC-SR04 Scanning
     ↓
Ultrasonic Pulse
     ↓
Echo Detection
     ↓
Distance Calculation
     ↓
Angle + Distance
     ↓
Serial Communication
     ↓
Processing IDE
     ↓
Real-Time Radar Display
     ↓
Object Detected Within 20 cm
     ↓
LED + Buzzer Alert
```

The **Arduino UNO Radar System** demonstrates how embedded hardware, sensors, servo control, serial communication and graphical visualization can be combined to build a simple real-time object detection system.

---

## 👨‍💻 Developed As

Part of a collection of projects exploring:

**Arduino • Embedded Systems • Sensors • Robotics • IoT • Visualization**

---

## 📜 License

This project is intended for **educational, experimentation and learning purposes**.
