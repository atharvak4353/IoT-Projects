# 🚪 Smart Door Viewer & Visitor Alert System

## ESP32-CAM Based IoT Doorbell with Telegram Photo Notifications

<p align="center">

![ESP32-CAM](https://img.shields.io/badge/ESP32--CAM-AI%20Thinker-E7352C?style=for-the-badge)
![Camera](https://img.shields.io/badge/Camera-OV2640-2563EB?style=for-the-badge)
![WiFi](https://img.shields.io/badge/Wi--Fi-2.4GHz-0EA5E9?style=for-the-badge)
![Telegram](https://img.shields.io/badge/Telegram-Bot-26A5E4?style=for-the-badge&logo=telegram&logoColor=white)
![Arduino](https://img.shields.io/badge/Arduino-C++-00979D?style=for-the-badge&logo=arduino&logoColor=white)

</p>

---

# 📌 Project Overview

The **Smart Door Viewer & Visitor Alert System** is an IoT-based security project built using an **ESP32-CAM AI Thinker board**.

When a visitor presses the doorbell push button, the ESP32-CAM:

- 🔊 Sounds a buzzer
- 📩 Sends a Telegram alert
- 📸 Captures the visitor's photograph
- 📲 Sends the photograph directly to the user's Telegram account

The Telegram bot can also receive remote commands from the user to:

- Take another photo
- Turn the ESP32-CAM flash LED ON or OFF

This allows the user to monitor visitors remotely without requiring a separate mobile application or local server.

---

# 🎯 Aim

To build an IoT-based smart door security system that can detect a visitor, capture their photograph, and send a real-time notification to the user's smartphone using Telegram.

---

# ✨ Key Features

| Feature | Description |
|---|---|
| 🔘 Doorbell Detection | Push button on GPIO13 acts as visitor input |
| 📸 Visitor Photo Capture | OV2640 camera captures the visitor image |
| 📩 Telegram Alert | Sends a real-time visitor notification |
| 📲 Remote Photo Request | `/photo` captures a new image remotely |
| 🔦 Flash Control | `/flash` toggles the ESP32-CAM flash LED |
| 🔊 Buzzer Alert | Provides local audible feedback |
| 📡 Wi-Fi Connectivity | Uses 2.4 GHz Wi-Fi |
| 🔐 Secure Communication | Uses HTTPS through `WiFiClientSecure` |
| 🧠 Embedded Web/API Control | Telegram Bot API handles commands |

---

# 🧰 Hardware Components

| Component | Quantity | Purpose |
|---|---:|---|
| ESP32-CAM AI Thinker | 1 | Main controller and camera |
| OV2640 Camera | 1 | Captures visitor image |
| USB-to-TTL Programmer | 1 | Uploads firmware |
| Push Button | 1 | Doorbell input |
| Active Buzzer | 1 | Audible visitor alert |
| Jumper Wires | As required | Electrical connections |
| Breadboard | 1 | Prototype assembly |
| 5V Power Supply | 1 | Stable ESP32-CAM power |

---

# 🔌 Hardware Connections

## USB-to-TTL Programmer

| USB-to-TTL | ESP32-CAM | Function |
|---|---|---|
| TX | U0R / GPIO3 | Serial data to ESP32-CAM |
| RX | U0T / GPIO1 | Serial data from ESP32-CAM |
| 5V | 5V | Power |
| GND | GND | Common Ground |
| GPIO0 | GND | Programming mode only |

---

## Doorbell Button & Buzzer

| Component | Pin | ESP32-CAM | Function |
|---|---|---|---|
| Push Button | Terminal 1 | GPIO13 | Doorbell input |
| Push Button | Terminal 2 | GND | Ground |
| Active Buzzer | Positive | GPIO12 | Buzzer control |
| Active Buzzer | Negative | GND | Ground |

The ESP32-CAM onboard flash LED is controlled through:

```text
GPIO 4
```

---

# ⚠️ Power Supply Note

ESP32-CAM requires a stable 5V power supply.

An unstable or weak supply may cause:

- Camera initialization failure
- Brownout reset
- Wi-Fi disconnection
- Repeated restarting
- Failed photo capture

Use a reliable regulated 5V source.

---

# 🧠 System Architecture

```text
Visitor
   ↓
Push Button
   ↓
GPIO13
   ↓
ESP32-CAM
   ↓
Buzzer on GPIO12
   ↓
Capture Visitor Image
   ↓
Connect to Telegram API
   ↓
Send Alert Message
   ↓
Send Visitor Photo
   ↓
User Receives Notification
```

---

# 🔄 Working Principle

## 1️⃣ Wi-Fi Connection

ESP32-CAM connects to the configured 2.4 GHz Wi-Fi network.

---

## 2️⃣ Camera Initialization

The OV2640 camera is initialized using the ESP32 camera library.

---

## 3️⃣ Doorbell Monitoring

The ESP32-CAM continuously reads:

```text
GPIO13
```

The button uses:

```cpp
INPUT_PULLUP
```

Therefore:

```text
Button Released → HIGH
Button Pressed  → LOW
```

---

## 4️⃣ Buzzer Alert

When the button is pressed, the buzzer connected to:

```text
GPIO12
```

sounds twice.

---

## 5️⃣ Telegram Message

The system sends:

```text
Someone is at the door!
```

to the authorized Telegram chat.

---

## 6️⃣ Visitor Photo

ESP32-CAM activates the flash, captures a new image, and sends the JPEG photograph to Telegram.

---

# 🔁 Complete Working Sequence

```text
ESP32-CAM Starts
        ↓
Camera Initializes
        ↓
Connects to Wi-Fi
        ↓
Telegram Bot Starts
        ↓
Wait for Doorbell
        ↓
Visitor Presses Button
        ↓
Buzzer Sounds
        ↓
Telegram Alert Sent
        ↓
Flash LED Turns ON
        ↓
Visitor Photo Captured
        ↓
Flash LED Turns OFF
        ↓
Photo Sent to Telegram
        ↓
System Returns to Monitoring
```

---

# 📚 Software & Libraries

The project uses:

```cpp
#include <Arduino.h>
#include <WiFi.h>
#include <WiFiClientSecure.h>
#include <esp_camera.h>
#include <UniversalTelegramBot.h>
#include <ArduinoJson.h>
```

---

# 📦 Library Purpose

| Library | Purpose |
|---|---|
| `Arduino.h` | Core Arduino functions |
| `WiFi.h` | Connects ESP32-CAM to Wi-Fi |
| `WiFiClientSecure.h` | Secure HTTPS communication |
| `esp_camera.h` | Controls the OV2640 camera |
| `UniversalTelegramBot.h` | Telegram bot messaging |
| `ArduinoJson.h` | Processes Telegram API data |

---

# ⚙️ Arduino IDE Configuration

Use the following settings:

| Setting | Selection |
|---|---|
| Board | AI Thinker ESP32-CAM |
| PSRAM | Enabled |
| Partition Scheme | Huge APP |
| Upload Speed | 115200 |
| Port | USB-to-TTL COM Port |

---

# 🔧 Programming Mode

Before uploading:

```text
GPIO0 → GND
```

Then upload the program.

After uploading:

```text
Disconnect GPIO0 from GND
```

and reset the ESP32-CAM.

---

# 📡 Wi-Fi Configuration

Use placeholders in your GitHub code:

```cpp
const char* ssid =
    "YOUR_WIFI_NAME";

const char* password =
    "YOUR_WIFI_PASSWORD";
```

> ⚠️ Never upload your actual Wi-Fi password to a public repository.

---

# 🤖 Telegram Bot Setup

## Step 1 — Create a Telegram Bot

1. Open Telegram.
2. Search for:

```text
@BotFather
```

3. Send:

```text
/newbot
```

4. Enter a bot name.
5. Create a unique username ending in:

```text
bot
```

6. BotFather will provide a **Bot Token**.

Example placeholder:

```text
YOUR_TELEGRAM_BOT_TOKEN
```

---

# 🔐 Important Bot Token Note

Never publish your real:

```text
Telegram Bot Token
```

in a public GitHub repository.

Treat it like a password.

---

# 🆔 Get Telegram Chat ID

1. Open your newly created bot.
2. Press **START**.
3. Send:

```text
Hello
```

4. Open this API request in a browser:

```text
https://api.telegram.org/botYOUR_TOKEN/getUpdates
```

5. Find the:

```text
chat
```

object.

6. Copy the numeric:

```text
id
```

Use that value as:

```cpp
String CHAT_ID =
    "YOUR_TELEGRAM_CHAT_ID";
```

---

# 🔐 Telegram Credentials

Use placeholders in GitHub:

```cpp
String BOTtoken =
    "YOUR_TELEGRAM_BOT_TOKEN";

String CHAT_ID =
    "YOUR_TELEGRAM_CHAT_ID";
```

Keep the real values only in your local Arduino IDE copy.

---

# 📲 Telegram Commands

The project supports the following bot commands:

| Command | Function |
|---|---|
| `/start` | Displays available commands |
| `/photo` | Captures and sends a new photograph |
| `/flash` | Toggles the ESP32-CAM flash LED |

---

# 📸 `/photo` Command

When the user sends:

```text
/photo
```

the ESP32-CAM captures a new image and sends it to Telegram.

---

# 🔦 `/flash` Command

When the user sends:

```text
/flash
```

the state of:

```text
GPIO4
```

is toggled.

This controls the onboard flash LED.

---

# 📷 Camera Configuration

The project uses the **AI Thinker ESP32-CAM** pin configuration.

Important camera pins include:

```text
PWDN   → GPIO32
XCLK   → GPIO0
SIOD   → GPIO26
SIOC   → GPIO27
VSYNC  → GPIO25
HREF   → GPIO23
PCLK   → GPIO22
```

---

# 🖼 Camera Quality

When PSRAM is available:

```text
Frame Size    = UXGA
JPEG Quality  = 10
Frame Buffers = 2
```

Without PSRAM:

```text
Frame Size    = SVGA
JPEG Quality  = 12
Frame Buffers = 1
```

---

# 🔄 Camera Orientation

The project applies:

```cpp
sensor->set_vflip(
    sensor,
    1
);
```

and:

```cpp
sensor->set_hmirror(
    sensor,
    1
);
```

to correct the camera orientation.

---

# 🔦 Flash-Assisted Photo Capture

Before capturing the visitor photograph:

```text
Flash ON
   ↓
Wait 300 ms
   ↓
Capture Image
   ↓
Flash OFF
```

This helps improve image brightness in low-light conditions.

---

# 🌐 Telegram Photo Upload

The captured JPEG image is uploaded to:

```text
api.telegram.org
```

using a secure HTTPS connection on:

```text
Port 443
```

The ESP32-CAM sends the image using a multipart HTTP POST request.

---

# 🛠 Setup Procedure

## Step 1 — Complete Hardware Wiring

Connect:

- ESP32-CAM
- USB-to-TTL programmer
- Push button
- Active buzzer
- 5V power supply

---

## Step 2 — Install ESP32 Support

Install the ESP32 board package in Arduino IDE.

---

## Step 3 — Install Required Libraries

Install:

```text
UniversalTelegramBot
ArduinoJson
```

The ESP32 camera and Wi-Fi libraries are included with the ESP32 board package.

---

## Step 4 — Enter Credentials

Add your local:

```text
Wi-Fi SSID
Wi-Fi Password
Telegram Bot Token
Telegram Chat ID
```

---

## Step 5 — Enter Programming Mode

Connect:

```text
GPIO0 → GND
```

---

## Step 6 — Upload Program

Select:

```text
AI Thinker ESP32-CAM
```

and upload the firmware.

---

## Step 7 — Exit Programming Mode

Disconnect:

```text
GPIO0 → GND
```

---

## Step 8 — Reset Board

Press reset or power-cycle the ESP32-CAM.

---

## Step 9 — Open Serial Monitor

Use:

```text
115200 baud
```

---

## Step 10 — Wait for Wi-Fi

The terminal should display the ESP32-CAM IP address.

---

## Step 11 — Check Telegram

The bot should send:

```text
Smart Door Viewer is online
```

---

## Step 12 — Test Doorbell

Press the push button.

The system should:

```text
Buzzer Sounds
        ↓
Telegram Alert
        ↓
Photo Capture
        ↓
Visitor Photo Sent
```

---

# 🖥 Expected Serial Monitor Output

A successful startup may show:

```text
Connecting to YOUR_WIFI_NAME
....
ESP32-CAM IP Address: 192.168.x.x
```

When the button is pressed:

```text
Doorbell Pressed!
Preparing photo
```

---

# ✅ Expected Result

When the project is working correctly:

- ✅ ESP32-CAM initializes successfully
- ✅ Wi-Fi connection is established
- ✅ Telegram bot comes online
- ✅ Doorbell button is detected
- ✅ Buzzer sounds
- ✅ Telegram visitor alert is received
- ✅ Visitor image is captured
- ✅ Image is sent to Telegram
- ✅ `/photo` captures another image remotely
- ✅ `/flash` controls the flash LED

---

# 🔧 Troubleshooting

| Problem | Possible Cause | Solution |
|---|---|---|
| Camera initialization failed | Loose camera cable | Reconnect camera |
| Upload failed | GPIO0 not grounded | Connect GPIO0 to GND |
| No serial output | Wrong COM port / baud | Check port and use 115200 |
| Wi-Fi not connecting | Wrong credentials | Verify SSID/password |
| ESP32-CAM restarts | Weak power supply | Use stable regulated 5V |
| Telegram alert missing | Wrong Bot Token | Verify token |
| Photo not received | Network/camera issue | Check camera and internet |
| Button not working | GPIO13 wiring issue | Check button to GND |
| Buzzer not working | GPIO12 wiring issue | Check polarity and buzzer type |
| `/photo` not responding | Bot polling/network issue | Check Telegram connectivity |
| Flash not working | GPIO4 issue | Verify `/flash` command |

---

# 💡 Tips for Reliable Operation

For better performance:

- Use stable 5V power.
- Use a strong 2.4 GHz Wi-Fi connection.
- Keep the ESP32-CAM antenna area clear.
- Secure the camera ribbon cable.
- Avoid publishing credentials.
- Use a well-lit installation area.
- Test photo capture before mounting the system permanently.

---

# 🧠 What You Learn

This project provides practical experience with:

- 📷 ESP32-CAM
- 🧠 Embedded programming
- 📡 Wi-Fi communication
- 🔐 HTTPS communication
- 🤖 Telegram Bot API
- 📸 Camera image capture
- 🔦 Flash control
- 🔘 Digital input
- 🔊 Digital output
- 🧾 ArduinoJson
- 🌐 IoT notifications
- 🔒 Basic IoT security practices

---

# 🌍 Applications

This project can be used as a base for:

- 🏠 Smart home security
- 🚪 Smart door viewers
- 📸 Visitor monitoring
- 🏢 Office entrance monitoring
- 🏬 Shop security
- 🏭 Factory access monitoring
- 🏫 School entrance monitoring
- 📦 Delivery monitoring
- 🛎 Smart doorbells
- 📲 Remote visitor alerts

---

# 🚀 Future Improvements

The project can be extended with:

- 🚶 PIR sensor based automatic detection
- 🧠 Face recognition
- 🔐 Face-based access control
- 🔓 Electronic door lock control
- ☁️ Cloud image storage
- 📷 Multiple image capture
- 🎥 Short video recording
- 🔊 Two-way communication
- 🌐 Web dashboard
- 📱 Dedicated mobile application
- 🔔 Multiple user notifications
- 🕒 Date and time logging
- 💾 Visitor database
- 🌙 Night vision
- 📊 Visitor history

---

# 📁 Suggested Repository Structure

```text
Smart-Door-Viewer/
│
├── README.md
├── index.html
├── LICENSE
│
├── firmware/
│   └── smart_door_viewer.ino
│
├── images/
│   ├── ESP32 CAM.png
│   ├── USB TTL.png
│   ├── Push Button.png
│   ├── Buzzer.png
│   ├── Jumper Wires.png
│   ├── Breadboard.png
│   ├── 5V Power Supply.png
│   └── ESP32 CAM Circuit Diagram 1.png
│
└── docs/
    └── project-documentation.pdf
```

---

# ⚠️ Security Notes

Do not commit real values for:

```text
Wi-Fi SSID
Wi-Fi Password
Telegram Bot Token
Telegram Chat ID
```

Use placeholders in public repositories.

For example:

```cpp
const char* ssid =
    "YOUR_WIFI_NAME";

const char* password =
    "YOUR_WIFI_PASSWORD";

String BOTtoken =
    "YOUR_TELEGRAM_BOT_TOKEN";

String CHAT_ID =
    "YOUR_TELEGRAM_CHAT_ID";
```

---

# 🏁 Project Summary

```text
Visitor Presses Button
        ↓
ESP32-CAM
        ↓
Buzzer Alert
        ↓
Camera Capture
        ↓
Wi-Fi
        ↓
Telegram Bot API
        ↓
Visitor Alert Message
        ↓
Visitor Photograph
        ↓
User Smartphone
```

The **Smart Door Viewer & Visitor Alert System** demonstrates how **ESP32-CAM, IoT, camera interfacing, Telegram API, Wi-Fi and embedded hardware** can be combined to build a practical remote visitor monitoring system.

---

## 👨‍💻 Developed As

Part of a collection of projects exploring:

**IoT • ESP32 • Embedded Systems • Smart Security • Automation**

---

## 📜 License

This project is intended for **educational, experimentation and learning purposes**.
