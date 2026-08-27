# 🌦️ Weather & Air Monitoring System

## IoT-Based Environmental Monitoring using Arduino UNO R4 WiFi & Blynk

<p align="center">

![Arduino](https://img.shields.io/badge/Arduino-UNO%20R4%20WiFi-00878F?style=for-the-badge&logo=arduino&logoColor=white)
![Blynk](https://img.shields.io/badge/Blynk-IoT-22C55E?style=for-the-badge)
![IoT](https://img.shields.io/badge/IoT-Environmental%20Monitoring-0284C7?style=for-the-badge)
![DHT11](https://img.shields.io/badge/Sensor-DHT11-F59E0B?style=for-the-badge)
![Air Quality](https://img.shields.io/badge/Air%20Quality-MQ%20Sensors-16A34A?style=for-the-badge)

</p>

---

# 📌 Project Overview

The **Weather & Air Monitoring System** is an IoT-based environmental monitoring project developed using the **Arduino UNO R4 WiFi**.

The system monitors:

- 🌡️ Temperature
- 💧 Humidity
- 🌫️ Air-quality-related sensor readings
- 🔥 MQ2 gas sensor reading
- 🏭 MQ9 gas sensor reading

The project uses a **DHT11** sensor for temperature and humidity monitoring and three MQ-series sensors:

```text
MQ135
MQ2
MQ9
```

The Arduino continuously collects sensor readings, processes the values, and sends the data over Wi-Fi to the **Blynk IoT dashboard**.

This allows environmental conditions to be monitored remotely using a smartphone or computer.

---

# 🎯 Aim

To develop an IoT-based environmental monitoring system capable of measuring temperature, humidity and multiple gas-sensor readings and displaying the collected data remotely through Blynk IoT.

---

# ✨ Key Features

| Feature | Description |
|---|---|
| 🌡️ Temperature Monitoring | Measures temperature using DHT11 |
| 💧 Humidity Monitoring | Measures relative humidity |
| 🌫️ Air Monitoring | MQ135 provides air-quality-related readings |
| 🔥 Gas Monitoring | MQ2 provides combustible-gas-related readings |
| 🏭 MQ9 Monitoring | Provides another gas-sensor channel |
| 📡 Built-in Wi-Fi | Uses Arduino UNO R4 WiFi |
| ☁️ Blynk IoT | Sends readings to a cloud dashboard |
| 📱 Remote Monitoring | Values can be viewed remotely |
| ⏱️ Automatic Updates | Sensor values update every 2 seconds |
| 🖥️ Serial Monitoring | Readings are also displayed in Serial Monitor |

---

# 🧰 Hardware Components

| Component | Quantity | Purpose |
|---|---:|---|
| Arduino UNO R4 WiFi | 1 | Main controller |
| DHT11 Sensor | 1 | Temperature and humidity measurement |
| MQ135 Sensor | 1 | Air-quality-related sensing |
| MQ2 Sensor | 1 | Gas/smoke-related sensing |
| MQ9 Sensor | 1 | Gas-related sensing |
| Breadboard | 1 | Prototype circuit assembly |
| Jumper Wires | As required | Electrical connections |
| 5V Power Supply | 1 | Powers the development board |

---

# 🔌 Hardware Connections

## 🌡️ DHT11

| DHT11 Pin | Arduino |
|---|---|
| VCC | 5V |
| GND | GND |
| DATA | D4 |

---

## 🌫️ MQ135

| MQ135 Pin | Arduino |
|---|---|
| VCC | 5V |
| GND | GND |
| AO | A0 |

---

## 🔥 MQ2

| MQ2 Pin | Arduino |
|---|---|
| VCC | 5V |
| GND | GND |
| AO | A2 |

---

## 🏭 MQ9

| MQ9 Pin | Arduino |
|---|---|
| VCC | 5V |
| GND | GND |
| AO | A3 |

---

# 📌 Pin Summary

```text
DHT11 DATA → D4

MQ135 AO   → A0
MQ2 AO     → A2
MQ9 AO     → A3
```

Wi-Fi communication uses the **built-in Wi-Fi interface of the Arduino UNO R4 WiFi**.

---

# 🧠 System Architecture

```text
             DHT11
        Temperature
         + Humidity
              │
              ▼
             D4
              │
              │
MQ135 ──────► A0
              │
MQ2 ────────► A2
              │
MQ9 ────────► A3
              │
              ▼
      Arduino UNO R4 WiFi
              │
              ▼
       Sensor Processing
              │
              ▼
            Wi-Fi
              │
              ▼
          Blynk IoT
              │
              ▼
      Remote Dashboard
```

---

# 🔄 How the System Works

```text
System Starts
      ↓
Initialize DHT11
      ↓
Initialize MQ Sensors
      ↓
Connect to Wi-Fi
      ↓
Connect to Blynk
      ↓
Read Temperature
      ↓
Read Humidity
      ↓
Read MQ135
      ↓
Read MQ2
      ↓
Read MQ9
      ↓
Map Analog Values
      ↓
Send Readings to Blynk
      ↓
Print Data in Serial Monitor
      ↓
Wait 2 Seconds
      ↓
Repeat
```

---

# 🌡️ Temperature Monitoring

Temperature is measured using the DHT11 sensor:

```cpp
float temperature =
    dht.readTemperature();
```

The value is sent to:

```text
Blynk Virtual Pin V0
```

Unit:

```text
°C
```

---

# 💧 Humidity Monitoring

Humidity is measured using:

```cpp
float humidity =
    dht.readHumidity();
```

The value is sent to:

```text
Blynk Virtual Pin V1
```

Unit:

```text
%
```

---

# 🌫️ MQ135 Monitoring

The MQ135 analog output is connected to:

```text
A0
```

The Arduino reads it using:

```cpp
analogRead(mq135);
```

The project maps the raw analog reading into:

```text
0 – 100
```

and sends it to:

```text
Blynk V2
```

---

# 🔥 MQ2 Monitoring

MQ2 is connected to:

```text
A2
```

Its analog reading is mapped to a monitoring range of:

```text
0 – 100
```

and sent to:

```text
Blynk V3
```

---

# 🏭 MQ9 Monitoring

MQ9 is connected to:

```text
A3
```

The processed value is sent to:

```text
Blynk V4
```

---

# ⚠️ Important Note About MQ Sensor Values

In this project, the MQ sensor analog readings are mapped to:

```text
0 – 100
```

for simple dashboard visualization.

For example:

```cpp
mq135Value = map(
    mq135Value,
    0,
    1023,
    0,
    100
);
```

These values should be treated as **relative sensor readings**, not calibrated gas concentrations such as ppm.

Accurate gas concentration measurement requires proper sensor calibration and consideration of the individual sensor characteristics.

---

# 📡 Data Flow

```text
Environmental Sensors
          ↓
Arduino UNO R4 WiFi
          ↓
Analog / Digital Acquisition
          ↓
Data Processing
          ↓
Wi-Fi Connection
          ↓
Blynk Cloud
          ↓
Dashboard Gauges
```

---

# 💻 Software Requirements

| Software / Library | Purpose |
|---|---|
| Arduino IDE | Firmware development |
| Blynk IoT | Remote dashboard |
| WiFiS3 | UNO R4 WiFi communication |
| Blynk Library | Blynk cloud connection |
| DHT Sensor Library | DHT11 interfacing |
| Adafruit Unified Sensor | DHT dependency |
| SPI | Communication support |

---

# 📚 Libraries Used

The firmware uses:

```cpp
#include <SPI.h>
#include <WiFiS3.h>
#include <BlynkSimpleWifi.h>
#include <DHT.h>
```

---

# ☁️ Blynk IoT Setup

## Step 1 — Create Template

Create a template with:

| Setting | Value |
|---|---|
| Template Name | Weather & Air Monitoring |
| Hardware | Arduino UNO R4 WiFi |
| Connection Type | WiFi |

---

# 📊 Step 2 — Create Datastreams

Create the following Blynk Virtual Pins:

| Virtual Pin | Parameter | Type | Range | Unit |
|---|---|---|---|---|
| V0 | Temperature | Double | 0–100 | °C |
| V1 | Humidity | Double | 0–100 | % |
| V2 | MQ135 | Double | 0–100 | % |
| V3 | MQ2 | Double | 0–100 | % |
| V4 | MQ9 | Double | 0–100 | % |

---

# 📱 Step 3 — Configure Dashboard

Add the following widgets:

| Widget | Datastream |
|---|---|
| 🌡️ Temperature Gauge | V0 |
| 💧 Humidity Gauge | V1 |
| 🌫️ MQ135 Gauge | V2 |
| 🔥 MQ2 Gauge | V3 |
| 🏭 MQ9 Gauge | V4 |

The dashboard can then display all five readings simultaneously.

---

# 🔐 Blynk & Wi-Fi Credentials

Use:

```cpp
#define BLYNK_TEMPLATE_ID "YOUR_TEMPLATE_ID"
#define BLYNK_TEMPLATE_NAME "Weather & Air Monitoring"
#define BLYNK_AUTH_TOKEN "YOUR_BLYNK_AUTH_TOKEN"

char ssid[] = "YOUR_WIFI_NAME";
char pass[] = "YOUR_WIFI_PASSWORD";
```

> ⚠️ **Do not upload your real Wi-Fi password or Blynk Auth Token to a public GitHub repository.**

---

# ⏱️ Update Interval

Sensor data is uploaded every:

```text
2 seconds
```

using:

```cpp
timer.setInterval(
    2000L,
    sendSensorData
);
```

This provides regular dashboard updates without continuously flooding the Blynk server.

---

# 🔢 MQ Sensor Processing

Each sensor produces an analog reading.

For example:

```cpp
float mq135Value =
    analogRead(mq135);
```

The program then converts it:

```cpp
mq135Value = map(
    mq135Value,
    0,
    1023,
    0,
    100
);
```

The same process is used for:

```text
MQ135
MQ2
MQ9
```

---

# ✅ DHT Error Checking

The program checks whether the DHT11 returns a valid value:

```cpp
if (
    isnan(temperature) ||
    isnan(humidity)
)
{
    Serial.println(
        "DHT read failed"
    );

    return;
}
```

If the sensor reading fails, invalid temperature and humidity data are not sent to Blynk during that cycle.

---

# 📤 Blynk Data Transmission

The readings are uploaded using:

```cpp
Blynk.virtualWrite(
    V0,
    temperature
);
```

The complete mapping is:

```text
V0 → Temperature
V1 → Humidity
V2 → MQ135
V3 → MQ2
V4 → MQ9
```

---

# 🖥️ Serial Monitor

The system also prints readings to the Arduino Serial Monitor.

Example:

```text
Temperature: 28.00 C | Humidity: 65.00 % | MQ135: 32 | MQ2: 18 | MQ9: 24
```

Set Serial Monitor to:

```text
9600 baud
```

---

# ▶️ How to Run the Project

### Step 1

Complete the hardware connections.

### Step 2

Connect the Arduino UNO R4 WiFi to your computer.

### Step 3

Open Arduino IDE.

### Step 4

Install the required libraries.

### Step 5

Create the Blynk template and datastreams.

### Step 6

Enter your:

```text
Blynk Template ID
Blynk Auth Token
Wi-Fi SSID
Wi-Fi Password
```

### Step 7

Select:

```text
Arduino UNO R4 WiFi
```

as the board.

### Step 8

Select the correct COM port.

### Step 9

Upload the firmware.

### Step 10

Open Serial Monitor at:

```text
9600
```

### Step 11

Open the Blynk dashboard.

### Step 12

Observe the live sensor readings.

---

# 🧪 Expected Result

When the project is working properly:

- ✅ Arduino UNO R4 WiFi starts successfully
- ✅ Wi-Fi connection is established
- ✅ Blynk connection is established
- ✅ DHT11 measures temperature
- ✅ DHT11 measures humidity
- ✅ MQ135 analog value is collected
- ✅ MQ2 analog value is collected
- ✅ MQ9 analog value is collected
- ✅ Values are mapped for dashboard monitoring
- ✅ Sensor readings appear in Serial Monitor
- ✅ Blynk dashboard updates automatically
- ✅ New data is transmitted every two seconds

---

# 🔧 Troubleshooting

| Problem | Possible Cause | Solution |
|---|---|---|
| Arduino not connecting to Wi-Fi | Wrong credentials | Verify SSID and password |
| Blynk shows Offline | Wrong Auth Token | Verify Blynk configuration |
| DHT read failed | DHT wiring/library issue | Check D4 and sensor wiring |
| Temperature incorrect | DHT11 limitation/placement | Check sensor position |
| MQ135 value not changing | Wiring/sensor issue | Check A0 |
| MQ2 value not changing | Wiring/sensor issue | Check A2 |
| MQ9 value not changing | Wiring/sensor issue | Check A3 |
| Values fluctuate heavily | Sensor/environment variation | Allow sensors to stabilize |
| Blynk gauge not updating | Wrong virtual pin | Verify V0–V4 |
| Arduino resets | Power supply issue | Use a suitable stable supply |

---

# 🔥 MQ Sensor Warm-Up

MQ-series sensors use an internal heater.

After switching on the system, the readings may need time to stabilize.

For better repeatability:

```text
Power sensors
      ↓
Allow warm-up
      ↓
Observe baseline
      ↓
Begin monitoring
```

Avoid treating the first readings immediately after power-up as stable measurements.

---

# 💡 Tips for Better Monitoring

For more reliable results:

- Keep sensors in an open-air location.
- Avoid placing MQ sensors directly next to each other if heat affects measurements.
- Use a stable power supply.
- Allow the MQ sensors to warm up.
- Keep the DHT11 away from direct heat sources.
- Avoid touching MQ sensors while operating.
- Keep connections secure.
- Calibrate sensors if meaningful gas concentration values are required.

---

# ⚠️ Project Limitation

The current firmware maps raw MQ analog readings to a simple:

```text
0 – 100
```

dashboard scale.

Therefore, this project should be considered an **environmental monitoring prototype**, not a calibrated safety instrument.

It should not be relied upon as a certified:

- Smoke detector
- Carbon monoxide alarm
- Fire alarm
- Toxic gas detector
- Industrial safety monitor

---

# 🧠 What You Learn

This project provides practical experience with:

- 🔵 Arduino UNO R4 WiFi
- 🌡️ DHT11 interfacing
- 🌫️ MQ135 sensor interfacing
- 🔥 MQ2 sensor interfacing
- 🏭 MQ9 sensor interfacing
- 📊 Analog data acquisition
- 🧮 Sensor-value mapping
- 📡 Wi-Fi communication
- ☁️ Blynk IoT
- 📱 IoT dashboards
- ⏱️ Timed sensor updates
- 🖥️ Serial monitoring
- 🧠 Embedded C/C++
- 🌍 Environmental monitoring

---

# 🌍 Applications

The project can serve as a foundation for:

- 🏠 Indoor environmental monitoring
- 🌦️ Weather stations
- 🏫 Classroom air monitoring
- 🏢 Office monitoring
- 🌱 Greenhouse monitoring
- 🏭 Industrial environmental monitoring prototypes
- 🚗 Garage air monitoring experiments
- 📡 IoT sensor networks
- 🌾 Smart agriculture
- 🎓 Educational IoT projects

---

# 🚀 Future Improvements

The system can be extended with:

- 📊 Historical graphs
- 💾 Cloud data logging
- 🚨 Threshold alerts
- 🔔 Blynk notifications
- 🌡️ Higher-accuracy temperature sensors
- 💨 PM2.5 / particulate matter sensor
- 🧪 Calibrated gas measurements
- 🌬️ CO₂ sensor
- 🌞 Light intensity sensor
- 🌧️ Rain sensor
- 🌪️ Pressure sensor
- 📈 Weather trend analysis
- 📍 GPS/location tagging
- 📱 Mobile notifications
- 🤖 Automatic ventilation control
- 🌀 Exhaust fan control
- ☁️ Additional cloud platforms

---

# 📁 Suggested Repository Structure

```text
Weather-Air-Monitoring/
│
├── README.md
├── index.html
├── LICENSE
│
├── firmware/
│   └── weather_air_monitoring.ino
│
├── images/
│   ├── Weather Air Monitoring.png
│   ├── Arduino r4.png
│   ├── Breadboard.png
│   ├── DHT11.png
│   ├── MQ135 sensor.png
│   ├── MQ2 sensor.png
│   ├── MQ9 sensor.png
│   ├── Jumper Wires.png
│   └── 5V Power Supply.png
│
└── docs/
    └── project-documentation.pdf
```

---

# 🏁 Project Summary

```text
DHT11
 │
 ├── Temperature
 └── Humidity
        │
        │
MQ135 ──┤
MQ2 ────┤
MQ9 ────┤
        ▼
Arduino UNO R4 WiFi
        │
        ▼
Sensor Processing
        │
        ▼
      Wi-Fi
        │
        ▼
    Blynk IoT
        │
        ▼
Environmental Dashboard
```

The **Weather & Air Monitoring System** demonstrates how **Arduino, environmental sensors, analog data acquisition, Wi-Fi and Blynk IoT** can be combined to create a real-time remote monitoring platform.

---

## 👨‍💻 Developed As

Part of a collection of projects exploring:

**Arduino • IoT • Embedded Systems • Sensors • Environmental Monitoring • Automation**

---

## 📜 License

This project is intended for **educational, experimentation and learning purposes**.
