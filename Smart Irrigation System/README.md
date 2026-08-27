# 🌱 Smart Irrigation System

## IoT-Based Automatic Irrigation using Arduino UNO R4 WiFi & Blynk

<p align="center">

![Arduino](https://img.shields.io/badge/Arduino-UNO%20R4%20WiFi-00878F?style=for-the-badge&logo=arduino&logoColor=white)
![Blynk](https://img.shields.io/badge/Blynk-IoT-22C55E?style=for-the-badge)
![IoT](https://img.shields.io/badge/IoT-Smart%20Agriculture-16A34A?style=for-the-badge)
![C++](https://img.shields.io/badge/C%2FC++-Arduino-00599C?style=for-the-badge&logo=cplusplus&logoColor=white)
![WiFi](https://img.shields.io/badge/WiFi-Connected-0284C7?style=for-the-badge)

</p>

---

# 📌 Project Overview

The **Smart Irrigation System** is an IoT-based automated irrigation project built using an **Arduino UNO R4 WiFi**.

The system continuously monitors:

- 🌱 Soil Moisture
- 🌧️ Rain Level
- 🌡️ Temperature
- 💧 Humidity

Based on the soil moisture level, the Arduino can automatically control a **DC water pump through a relay module**.

The project also connects to **Blynk IoT**, allowing sensor readings to be monitored remotely and the water pump to be controlled manually.

The system provides two operating modes:

```text
Automatic Mode
      +
Manual Mode
```

---

# 🎯 Aim

To develop an IoT-based smart irrigation system that automatically waters plants according to soil moisture conditions while also providing real-time environmental monitoring and remote pump control through Blynk IoT.

---

# ✨ Key Features

| Feature | Description |
|---|---|
| 🌱 Soil Monitoring | Continuously measures soil moisture |
| 💧 Automatic Irrigation | Automatically controls the water pump |
| 📱 Manual Pump Control | Pump can be controlled from Blynk |
| 🌧️ Rain Monitoring | Monitors rain sensor readings |
| 🌡️ Temperature Monitoring | Measures temperature using DHT11 |
| 💦 Humidity Monitoring | Measures humidity using DHT11 |
| 📡 Wi-Fi Connectivity | Uses Arduino UNO R4 WiFi |
| ☁️ Blynk IoT | Remote monitoring and control |
| 🔄 Auto / Manual Mode | Allows switching between operating modes |
| ⏱️ Live Updates | Sensor data updates every 2 seconds |

---

# 🧰 Hardware Components

| Component | Quantity | Purpose |
|---|---:|---|
| Arduino UNO R4 WiFi | 1 | Main controller |
| Soil Moisture Sensor | 1 | Measures soil moisture |
| Rain Sensor | 1 | Measures rain level |
| DHT11 | 1 | Temperature and humidity sensing |
| 1-Channel Relay Module | 1 | Controls the water pump |
| DC Water Pump | 1 | Supplies irrigation water |
| Jumper Wires | As required | Circuit connections |
| Breadboard | 1 | Prototype wiring |
| 5V Power Supply | 1 | Powers the controller |

---

# 🔌 Hardware Connections

## 🌱 Soil Moisture Sensor

| Sensor Pin | Arduino Connection |
|---|---|
| VCC | 5V |
| GND | GND |
| AO | A0 |

---

## 🌧️ Rain Sensor

| Sensor Pin | Arduino Connection |
|---|---|
| VCC | 5V |
| GND | GND |
| AO | A1 |

---

## 🌡️ DHT11 Sensor

| DHT11 Pin | Arduino Connection |
|---|---|
| VCC | 5V |
| GND | GND |
| DATA | D2 |

---

## ⚡ Relay Module

| Relay Pin | Arduino Connection |
|---|---|
| VCC | 5V |
| GND | GND |
| IN | D7 |

---

# 💧 Water Pump Connection

The water pump is controlled through the relay instead of being connected directly to the Arduino.

```text
External Power Supply (+)
          │
          ▼
       Relay COM
          │
       Relay NO
          │
          ▼
    Water Pump (+)

Water Pump (-)
          │
          ▼
External Power Supply (-)
```

> ⚠️ **Important:** Do not power a high-current water pump directly from an Arduino GPIO pin.

---

# 🧠 System Architecture

```text
          Soil Moisture Sensor
                  │
                  ▼
                 A0
                  │
                  │
Rain Sensor ────► A1
                  │
                  │
DHT11 ──────────► D2
                  │
                  ▼
        Arduino UNO R4 WiFi
                  │
          ┌───────┴────────┐
          │                │
          ▼                ▼
       Blynk IoT         D7
          │                │
          ▼                ▼
   Mobile Dashboard      Relay
                           │
                           ▼
                      Water Pump
```

---

# 🔄 Working Principle

The system follows this sequence:

```text
System Starts
      ↓
Arduino Connects to Wi-Fi
      ↓
Arduino Connects to Blynk
      ↓
Read Soil Moisture
      ↓
Read Rain Sensor
      ↓
Read DHT11
      ↓
Calculate Sensor Values
      ↓
Send Data to Blynk
      ↓
Check Operating Mode
      ↓
 ┌──────────────┐
 │              │
 ▼              ▼
AUTO          MANUAL
 │              │
 ▼              ▼
Check Soil    Read Blynk
Moisture      Pump Switch
 │              │
 ▼              ▼
Control Pump  Control Pump
```

---

# 🤖 Automatic Mode

In **Automatic Mode**, the Arduino automatically controls irrigation based on the soil moisture percentage.

The configured threshold is:

```text
20%
```

### Soil moisture below 20%

```text
Soil Moisture < 20%
        ↓
Soil is considered dry
        ↓
Relay ON
        ↓
Water Pump ON
```

### Soil moisture 20% or above

```text
Soil Moisture ≥ 20%
        ↓
Relay OFF
        ↓
Water Pump OFF
```

The logic used in the program is:

```cpp
if (soilPercent < 20)
{
    digitalWrite(RELAY_PIN, LOW);
}
else
{
    digitalWrite(RELAY_PIN, HIGH);
}
```

---

# 🎮 Manual Mode

The system can also operate in **Manual Mode**.

In this mode, automatic soil-moisture control is disabled and the user controls the water pump through the Blynk dashboard.

```text
Blynk Pump Switch
        ↓
Arduino UNO R4 WiFi
        ↓
Relay Module
        ↓
Water Pump
```

---

# 📱 Blynk IoT Integration

Blynk is used for:

- Monitoring soil moisture
- Monitoring temperature
- Monitoring humidity
- Monitoring rain level
- Controlling the water pump
- Selecting Auto / Manual mode

---

# ☁️ Blynk Template Configuration

Create a new template in Blynk.

Use:

| Setting | Value |
|---|---|
| Template Name | Smart Irrigation |
| Hardware | Arduino UNO R4 WiFi |
| Connection Type | WiFi |

---

# 📊 Blynk Datastreams

Create the following Virtual Pin datastreams:

| Virtual Pin | Parameter | Type | Range | Unit |
|---|---|---|---|---|
| V0 | Soil Moisture | Integer | 0–100 | % |
| V1 | Temperature | Double | 0–100 | °C |
| V2 | Humidity | Double | 0–100 | % |
| V3 | Rain Level | Integer | 0–100 | % |
| V4 | Pump Switch | Integer | 0–1 | — |
| V5 | Mode Switch | Integer | 0–1 | — |

---

# 📲 Blynk Dashboard

Configure the dashboard with:

| Dashboard Widget | Virtual Pin |
|---|---|
| 🌱 Soil Moisture Gauge | V0 |
| 🌡️ Temperature Gauge | V1 |
| 💧 Humidity Gauge | V2 |
| 🌧️ Rain Level Gauge | V3 |
| 🚿 Pump Control Switch | V4 |
| ⚙️ Auto / Manual Switch | V5 |

---

# 🔐 Blynk Credentials

Replace the placeholders with your own credentials:

```cpp
#define BLYNK_TEMPLATE_ID "YOUR_TEMPLATE_ID"
#define BLYNK_TEMPLATE_NAME "Smart Irrigation"
#define BLYNK_AUTH_TOKEN "YOUR_BLYNK_AUTH_TOKEN"

char ssid[] = "YOUR_WIFI_NAME";
char pass[] = "YOUR_WIFI_PASSWORD";
```

> ⚠️ **Never upload your real Wi-Fi password or Blynk Authentication Token to a public GitHub repository.**

---

# 💻 Software Requirements

| Software / Library | Purpose |
|---|---|
| Arduino IDE | Programming and uploading firmware |
| Blynk IoT | Remote monitoring and control |
| WiFiS3 | UNO R4 WiFi connectivity |
| Blynk Library | Blynk cloud communication |
| DHT Sensor Library | DHT11 communication |
| Adafruit Unified Sensor | Sensor library dependency |

---

# 📚 Required Arduino Libraries

Install the required libraries using:

```text
Arduino IDE
      ↓
Sketch
      ↓
Include Library
      ↓
Manage Libraries
```

Install:

```text
Blynk
DHT Sensor Library
Adafruit Unified Sensor
```

The Arduino UNO R4 WiFi uses:

```cpp
#include <WiFiS3.h>
```

for Wi-Fi communication.

---

# 🧾 Important Pin Definitions

The project uses:

```cpp
#define SOIL_PIN A0
#define RAIN_PIN A1
#define RELAY_PIN 7

#define DHTPIN 2
#define DHTTYPE DHT11
```

Therefore:

```text
A0 → Soil Moisture
A1 → Rain Sensor
D2 → DHT11
D7 → Relay
```

---

# ☁️ Blynk Virtual Pins

The firmware defines:

```cpp
#define VPIN_SOIL V0
#define VPIN_TEMP V1
#define VPIN_HUM V2
#define VPIN_RAIN V3
#define VPIN_PUMP V4
#define VPIN_MODE V5
```

---

# 🌱 Soil Moisture Calculation

The analog soil sensor reading is converted into a percentage using:

```cpp
int soilPercent =
    map(
        soilRaw,
        1023,
        0,
        0,
        100
    );
```

The resulting value is displayed as:

```text
0 – 100%
```

---

# 🌧️ Rain Level Calculation

The rain sensor value is also converted to percentage:

```cpp
int rainPercent =
    map(
        rainRaw,
        1023,
        0,
        0,
        100
    );
```

This value is sent to:

```text
Blynk V3
```

---

# 🌡️ Temperature & Humidity

Temperature is read using:

```cpp
float temperature =
    dht.readTemperature();
```

Humidity is read using:

```cpp
float humidity =
    dht.readHumidity();
```

These values are sent to:

```text
Temperature → V1
Humidity    → V2
```

---

# ⏱️ Sensor Update Interval

The system sends updated sensor data every:

```text
2 Seconds
```

using:

```cpp
timer.setInterval(
    2000L,
    sendSensorData
);
```

---

# 📡 Data Flow

```text
Sensors
   ↓
Arduino UNO R4 WiFi
   ↓
Sensor Processing
   ↓
Wi-Fi
   ↓
Blynk Cloud
   ↓
Blynk Dashboard
   ↓
Real-Time Monitoring
```

For manual control:

```text
Blynk Dashboard
      ↓
Pump Switch V4
      ↓
Arduino
      ↓
Relay
      ↓
Water Pump
```

---

# ▶️ Running the Project

### Step 1

Complete all hardware connections.

### Step 2

Connect the Arduino UNO R4 WiFi to the computer.

### Step 3

Open the project in Arduino IDE.

### Step 4

Enter your:

```text
Blynk Template ID
Blynk Authentication Token
Wi-Fi Name
Wi-Fi Password
```

### Step 5

Select the Arduino UNO R4 WiFi board.

### Step 6

Select the correct COM port.

### Step 7

Upload the program.

### Step 8

Open Serial Monitor.

Set baud rate to:

```text
9600
```

### Step 9

Wait for the Arduino to connect to Wi-Fi and Blynk.

### Step 10

Open the Blynk dashboard.

---

# 🖥️ Serial Monitor Output

Example:

```text
Soil: 45% Rain: 10% Temp: 28.5 Hum: 65
```

The values continuously update as the sensors change.

---

# 🧪 Expected Result

When the project is working correctly:

- ✅ Arduino connects to Wi-Fi
- ✅ Arduino connects to Blynk
- ✅ Soil moisture is measured
- ✅ Rain level is measured
- ✅ Temperature is measured
- ✅ Humidity is measured
- ✅ Sensor values appear on Blynk
- ✅ Pump turns ON automatically when soil moisture is below 20%
- ✅ Pump turns OFF when soil moisture reaches 20% or above
- ✅ Manual pump control works from Blynk
- ✅ Auto / Manual mode can be selected remotely

---

# 🔧 Troubleshooting

| Problem | Possible Cause | Solution |
|---|---|---|
| Arduino not connecting to Wi-Fi | Incorrect credentials | Check SSID and password |
| Blynk offline | Wrong Auth Token | Verify Blynk credentials |
| Soil value incorrect | Sensor calibration | Check dry/wet raw readings |
| Rain value incorrect | Sensor calibration | Test sensor manually |
| DHT11 shows invalid data | Wiring/library issue | Check DHT11 wiring and library |
| Pump not running | Relay/power issue | Check relay and pump supply |
| Relay always ON | Relay logic issue | Verify active-LOW operation |
| Blynk values not updating | Virtual pins incorrect | Verify V0–V5 |
| Manual mode not working | V4/V5 configuration | Check Blynk switches |
| Arduino resets when pump starts | Power disturbance | Use appropriate separate pump supply |

---

# ⚠️ Pump Power Safety

The Arduino GPIO must **not directly power the pump**.

Use:

```text
Arduino
   ↓
Relay Module
   ↓
External Pump Power Supply
   ↓
Water Pump
```

The relay acts as the switching interface between the low-power Arduino control signal and the pump circuit.

---

# 🌱 Sensor Calibration

Soil moisture sensors can give different raw values depending on:

- Soil type
- Sensor model
- Water content
- Supply voltage
- Probe condition

For better accuracy, measure the raw sensor reading in:

```text
Completely Dry Soil
```

and:

```text
Wet Soil
```

Then adjust the mapping values in the program accordingly.

---

# 🧠 What You Learn

This project provides practical experience with:

- 🔵 Arduino UNO R4 WiFi
- 🌱 Soil moisture sensing
- 🌧️ Rain sensing
- 🌡️ Temperature sensing
- 💧 Humidity sensing
- ⚡ Relay interfacing
- 🚿 DC pump control
- 📡 Wi-Fi communication
- ☁️ Blynk IoT
- 📱 IoT dashboards
- 🔄 Automatic control
- 🎮 Manual control
- 🧠 Embedded programming
- 🌾 Smart agriculture

---

# 🌍 Applications

The Smart Irrigation concept can be used in:

- 🌱 Home gardens
- 🌾 Agriculture
- 🌿 Greenhouses
- 🪴 Indoor plant monitoring
- 🌳 Nurseries
- 🏡 Smart homes
- 🏫 Educational IoT projects
- 🚜 Precision agriculture
- 🌻 Automated gardening
- 💧 Water conservation systems

---

# 🚀 Future Improvements

The project can be expanded with:

- 📊 Historical sensor graphs
- 💾 Cloud data logging
- 🌦️ Online weather forecast integration
- 💧 Water tank level monitoring
- 🚨 Low-water alerts
- 🔔 Blynk notifications
- ⏰ Irrigation scheduling
- 🌱 Multiple soil moisture sensors
- 🚿 Multiple irrigation zones
- ☀️ Solar-powered operation
- 🔋 Battery monitoring
- 📈 Water consumption monitoring
- 🌡️ More accurate environmental sensors
- 🤖 AI-based irrigation prediction

---

# 📁 Suggested Repository Structure

```text
Smart-Irrigation-System/
│
├── README.md
├── index.html
├── LICENSE
│
├── firmware/
│   └── smart_irrigation.ino
│
├── images/
│   ├── Smart Irrigation System.png
│   ├── Arduino r4.png
│   ├── Soil moisture.png
│   ├── Rain sensor.png
│   ├── DHT11.png
│   ├── Relay.png
│   ├── Motor pump.png
│   ├── Jumper Wires.png
│   ├── Breadboard.png
│   └── 5V Power Supply.png
│
└── docs/
    └── project-documentation.pdf
```

---

# 🏁 Project Summary

```text
Soil Moisture ──┐
Rain Sensor ─────┤
DHT11 ───────────┤
                 ▼
        Arduino UNO R4 WiFi
                 │
        ┌────────┴─────────┐
        │                  │
        ▼                  ▼
    Blynk IoT            Relay
        │                  │
        ▼                  ▼
   Dashboard           Water Pump
        │
        ▼
Monitoring + Manual Control
```

The **Smart Irrigation System** demonstrates how **IoT, sensors, Arduino, Wi-Fi, relay control and cloud monitoring** can be combined to automate irrigation and provide remote monitoring of environmental conditions.

---

## 👨‍💻 Developed As

Part of a collection of projects exploring:

**Arduino • IoT • Embedded Systems • Sensors • Automation • Smart Agriculture**

---

## 📜 License

This project is intended for **educational, experimentation and learning purposes**.
