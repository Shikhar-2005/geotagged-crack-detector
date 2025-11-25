## geotagged-crack-detector
Real-time crack detection using Raspberry Pi, USB camera, TensorFlow Lite model (Edge Impulse), GPS (NEO-6M), and Telegram bot notifications. Includes headless mode and geotagged image capture.
##Geo-Tagged Crack Detection System using Raspberry Pi

A real-time surface crack detection system built using a Raspberry Pi, USB camera, TensorFlow Lite (Edge Impulse) model, NEO-6M GPS module, and Telegram Bot for instant alert notifications.
Supports GUI mode and headless (no monitor) mode.

Features

Real-time crack detection using a quantized TFLite model

GPS geotagging using NEO-6M (latitude & longitude)

Telegram bot alerts with:

Crack image

Prediction confidence

GPS coordinates

Clickable Google Maps link

Automatic FPS counter

Heatmap overlay toggle (press 'a')

Zoom toggle (press 's')

Graceful exit (press 'f')

Runs with or without GUI (headless mode)

🧰 Hardware Used
Component	Purpose
Raspberry Pi 4 (4GB/8GB)	Main compute board
USB Camera	Real-time imaging
NEO-6M GPS Module	Location acquisition
Jumper Wires (F-F / F-M)	Connections
Powerbank / 5V Supply	Power source
🔌 GPS Wiring (NEO-6M → Raspberry Pi GPIO)
GPS Pin	Raspberry Pi Pin	Description
VCC	Pin 2 (5V)	Power
GND	Pin 6 (GND)	Ground
TX	Pin 10 (GPIO15 / RXD)	GPS → Pi
RX	Pin 8 (GPIO14 / TXD)	Pi → GPS

GPS only needs VCC, GND, TX to work (RX optional).
The blue LED blinks when satellite lock is achieved.

🧠 AI Model (Edge Impulse)

Platform: Edge Impulse

Model type: Image classification

Input resolution: 96×96

Format: Quantized TensorFlow Lite (.tflite)

Performance:

Accuracy: ~90–95% (depends on dataset)

Latency: Fast (optimized TFLite + XNNPACK)

Dataset Split

80% Training

20% Testing

Why this approach?

Edge Impulse simplifies dataset creation, augmentation, model training & deployment.

TFLite model is optimized for low-power devices like Raspberry Pi.

📂 Project Structure
surface_crack_detection/
│── model/
│   └── model_quant.tflite
│── images/
│── gps_helper.py
│── surface_crack_detection_quant.py        ← GUI version
│── surface_crack_headless.py               ← No-GUI version
│── crack_alert.jpg
│── crack_alert_headless.jpg
│── README.md

🧭 GPS Helper Script

gps_helper.py continuously reads GPS NMEA data and returns:

latitude

longitude

fix status

Used to embed coordinates inside detection results.

💬 Telegram Notifications

You create a Telegram bot using @BotFather, get:

BOT_TOKEN

CHAT_ID

Use Python requests to send:

crack alert message

GPS coordinates

image evidence

Google Maps link

This allows remote monitoring.

▶️ Running the Project
1. Activate Virtual Environment
source ~/surface_crack/venv/bin/activate

2. Navigate to Project
cd ~/surface_crack/surface_crack_detection

🖥️ GUI Version (with VNC or Monitor)
python3 surface_crack_detection_quant.py


Window Controls:

Key	Action
a	Toggle Heatmap
s	Toggle Zoom
f	Quit
🖧 Headless Mode (No Monitor)

Works via SSH.

python3 surface_crack_headless.py


This version does not show GUI
Instead it:

saves crack image

sends Telegram alert

prints logs in terminal

🌍 Google Maps Link Example

Telegram message includes:

Crack Detected!
Confidence: 92.3%
GPS: 12.935607, 77.564345
https://maps.google.com/?q=12.935607,77.564345

🧪 Testing GPS
python3 gps_helper.py


Example output:

Waiting for GPS fix…
12.9356075 77.5643458

🧪 Testing Camera
python3 - << 'EOF'
import cv2
for i in range(6):
    print(i, cv2.VideoCapture(i).isOpened())
EOF

📦 Deployment Notes

Works with USB cameras (best option)

Pi Camera Module 3 (NoIR) works only with libcamera, not OpenCV V4L

Use powerbank for outdoor use

GPS needs at least 20–40 seconds under open sky for satellite lock

🏁 Future Improvements

Cloud logging dashboard

Use YOLOv8-Nano for more accurate crack segmentation

Use 4G/LTE module instead of WiFi

Add battery voltage monitoring

👥 Team Presentation Summary

This project demonstrates the integration of:

Embedded systems

Computer vision

AI at the edge

Real-time telemetry

IoT (Telegram alerts + GPS)
