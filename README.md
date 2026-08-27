# 📷 ESP32-CAM Experiment No. 4

## Live Camera Streaming with SD Card Photo Capture

A simple **ESP32-CAM IoT project** that provides live video streaming over Wi-Fi and allows the user to capture photos directly from a web browser and save them to a Micro SD card.

---

<p align="center">

![ESP32](https://img.shields.io/badge/ESP32-CAM-E7352C?style=for-the-badge&logo=espressif&logoColor=white)
![Arduino](https://img.shields.io/badge/Arduino-IDE-00979D?style=for-the-badge&logo=arduino&logoColor=white)
![WiFi](https://img.shields.io/badge/WiFi-Enabled-2563EB?style=for-the-badge)
![Camera](https://img.shields.io/badge/Camera-Live%20Streaming-16A34A?style=for-the-badge)
![SD Card](https://img.shields.io/badge/Storage-Micro%20SD-F59E0B?style=for-the-badge)

</p>

---

# 📌 Project Overview

This experiment demonstrates how the **ESP32-CAM** can be used as a small wireless camera system.

The ESP32-CAM connects to a Wi-Fi network and hosts a simple webpage that allows the user to:

- 📹 View a live camera stream
- 📸 Capture a photo
- 💾 Save the captured photo to a Micro SD card
- 🌐 Access the camera through a browser
- 📱 Use either a smartphone or laptop

The complete system works locally over Wi-Fi without requiring an external cloud service.

---

# 🎯 Aim

To create an ESP32-CAM based system that provides:

- Live camera streaming
- Browser-based image capture
- Micro SD card image storage
- Wireless access over Wi-Fi

---

# ⚙️ Main Features

| Feature | Description |
|---|---|
| 📹 Live Streaming | Displays the camera feed in a web browser |
| 📸 Photo Capture | Captures the current camera frame |
| 💾 SD Card Storage | Saves captured images as JPEG files |
| 🌐 Web Interface | Provides browser-based camera control |
| 📡 Wi-Fi Access | Allows wireless access to the ESP32-CAM |
| 🖥 Serial Monitor | Displays connection and storage information |

---

# 🧰 Components Required

| Component | Quantity | Purpose |
|---|---:|---|
| ESP32-CAM AI Thinker | 1 | Main controller and camera |
| Micro SD Card | 1 | Stores captured photographs |
| USB-to-TTL Programmer | 1 | Uploads program to ESP32-CAM |
| Jumper Wires | As required | Programming connections |
| Wi-Fi Network | 1 | Wireless communication |
| Laptop / Smartphone | 1 | Opens the camera webpage |

---

# 🔄 Working Principle

```text
ESP32-CAM Starts
        ↓
Camera Initializes
        ↓
Micro SD Card Mounts
        ↓
ESP32-CAM Connects to Wi-Fi
        ↓
HTTP Web Server Starts
        ↓
IP Address is Displayed
        ↓
User Opens IP Address
        ↓
Live Camera Stream Appears
        ↓
User Clicks Capture Photo
        ↓
Current Camera Frame is Captured
        ↓
JPEG Image is Saved to SD Card
```

---

# 🎥 Camera Configuration

| Setting | Value |
|---|---|
| Pixel Format | JPEG |
| Frame Size | QVGA |
| Resolution | 320 × 240 |
| JPEG Quality | 10 |
| Frame Buffer | 1 |
| Camera Clock | 13 MHz |
| Grab Mode | `CAMERA_GRAB_LATEST` |
| Vertical Flip | Enabled |

---

# 📚 Libraries Used

The experiment uses the following libraries:

```cpp
#include "esp_camera.h"
#include <WiFi.h>
#include <WebServer.h>
#include "FS.h"
#include "SD_MMC.h"
```

### Library Purpose

| Library | Purpose |
|---|---|
| `esp_camera.h` | Controls the ESP32-CAM camera |
| `WiFi.h` | Connects ESP32-CAM to Wi-Fi |
| `WebServer.h` | Creates the web server |
| `FS.h` | Provides file-system support |
| `SD_MMC.h` | Provides Micro SD card access |

---

# 📡 Wi-Fi Configuration

Before uploading the program, replace the Wi-Fi placeholders:

```cpp
const char *ssid = "YOUR_WIFI_NAME";
const char *password = "YOUR_WIFI_PASSWORD";
```

> ⚠️ **Important:** Never upload your real Wi-Fi password to a public GitHub repository.

---

# 🛠 Setup Procedure

### 1. Insert the SD Card

Insert the Micro SD card into the ESP32-CAM SD card slot.

### 2. Connect ESP32-CAM to USB-to-TTL

Connect the ESP32-CAM to the USB-to-TTL programmer.

### 3. Open Arduino IDE

Launch Arduino IDE on your computer.

### 4. Select Board

Select the ESP32-CAM AI Thinker board.

### 5. Select COM Port

Select the correct COM port.

### 6. Enter Wi-Fi Details

Update:

```cpp
YOUR_WIFI_NAME
YOUR_WIFI_PASSWORD
```

with your local Wi-Fi details.

### 7. Upload the Program

Verify and upload the program to the ESP32-CAM.

### 8. Open Serial Monitor

Set the baud rate to:

```text
115200
```

### 9. Check Initialization

You should see messages confirming:

- Camera initialization
- SD card mounting
- Wi-Fi connection
- HTTP server start

### 10. Open the Camera Page

Copy the IP address shown in Serial Monitor and open it in your browser.

Example:

```text
http://192.168.x.x
```

### 11. Capture an Image

Click:

```text
Capture Photo
```

The image will be saved to the Micro SD card.

---

# 🌐 Web Interface

The ESP32-CAM hosts a simple webpage containing:

```text
ESP32-CAM Live Stream

[ Live Camera Feed ]

[ Capture Photo ]
```

The live stream is available through:

```text
/stream
```

The image capture request is handled through:

```text
/capture
```

---

# 💾 SD Card Storage

The SD card is initialized using:

```cpp
SD_MMC.begin("/sdcard", true);
```

When initialization is successful:

```text
SD Card Mounted
```

is displayed in the Serial Monitor.

---

# 🖼 Image File Naming

Each captured image receives a unique filename.

Example:

```text
/photo_12345.jpg
```

The filename is generated using:

```cpp
String filename =
    "/photo_" +
    String(millis()) +
    ".jpg";
```

This helps prevent images from overwriting each other.

---

# 🖥 Expected Serial Monitor Output

After successful startup:

```text
Camera initialized successfully!
SD Card Mounted
Connecting to WiFi....
WiFi Connected!
Camera URL: http://192.168.x.x
HTTP server started
```

After capturing a photo:

```text
Photo saved: /photo_12345.jpg
```

---

# ✅ Expected Output

After successful implementation:

- Camera initializes successfully
- SD card mounts correctly
- ESP32-CAM connects to Wi-Fi
- IP address is shown in Serial Monitor
- Browser displays the live stream
- Capture button captures the current camera frame
- Captured image is saved to the SD card
- Browser displays confirmation after saving the photo

---

# 🧪 Result

The **ESP32-CAM Live Streaming with SD Card Photo Capture** experiment was successfully implemented.

The system can:

- Stream live camera video
- Capture images from a browser
- Store captured images on a Micro SD card
- Provide wireless camera access using Wi-Fi

---

# 🧠 What You Learn

This experiment provides practical experience with:

- ESP32-CAM programming
- Camera initialization
- Wi-Fi communication
- HTTP web servers
- Embedded web interfaces
- Live image streaming
- Micro SD card interfacing
- File handling
- JPEG image capture
- Browser-based controls

---

# 🔧 Troubleshooting

| Problem | Possible Cause | Solution |
|---|---|---|
| Camera initialization failed | Incorrect camera settings | Check AI Thinker configuration |
| Wi-Fi not connecting | Incorrect credentials | Verify SSID and password |
| SD Card Failed | Card not inserted correctly | Reinsert the SD card |
| SD card write failed | Card/file-system issue | Format or replace SD card |
| Webpage not opening | Different Wi-Fi network | Connect both devices to same network |
| No camera stream | Camera/server issue | Restart ESP32-CAM |
| Capture failed | Camera frame unavailable | Restart camera system |
| No IP address | Wi-Fi connection failed | Check Serial Monitor |

---

# 📂 Suggested Repository Structure

```text
ESP32-CAM-Experiment-No4/
│
├── index.html
├── README.md
├── LICENSE
│
├── src/
│   └── ESP32_CAM_SD_Capture.ino
│
├── images/
│   ├── esp32-cam.png
│   ├── micro-sd-card.png
│   ├── usb-ttl.png
│   └── hardware-setup.png
│
└── docs/
    └── experiment-documentation.pdf
```

---

# 🚀 Future Improvements

The project can be extended with:

- 📅 Date and time based image filenames
- 🚶 Motion detection
- 🔔 PIR sensor triggering
- 📲 Telegram alerts
- ☁️ Cloud image upload
- 🖼 Browser-based SD card gallery
- 🗑 Delete image option
- ⬇ Download captured images
- 🔦 Flash LED control
- 🎥 Resolution control
- 🔐 Password-protected web interface
- 📡 Remote monitoring over the internet

---

# 💡 Applications

This project can be used as a base for:

- Home surveillance
- Smart door cameras
- Wildlife monitoring
- Farm monitoring
- Industrial inspection
- Remote equipment monitoring
- Security cameras
- Image logging systems
- IoT monitoring devices

---

# ⚠️ Safety Notes

- Check all connections before powering the ESP32-CAM.
- Use the correct voltage supply.
- Do not short GPIO pins.
- Insert the Micro SD card correctly.
- Avoid removing the SD card while a file is being written.
- Never expose Wi-Fi passwords in a public GitHub repository.

---

# 🏁 Project Summary

This experiment combines:

```text
ESP32-CAM
    +
Wi-Fi
    +
Live Camera Streaming
    +
HTTP Web Server
    +
Photo Capture
    +
Micro SD Card Storage
```

into a complete embedded camera application.

---

## 👨‍💻 Developed As

Part of an **ESP32-CAM and IoT Experiment Series**.

---

## 📜 License

This project is intended for **educational and learning purposes**.
