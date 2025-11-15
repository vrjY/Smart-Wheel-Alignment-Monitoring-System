🚗 Smart Wheel Alignment Monitoring System
ESP32 + MPU6050 (Future Upgrade: STM32 + High-Precision IMUs)

This project implements a real-time wheel alignment monitoring system using an ESP32 and an MPU6050 IMU.
The ESP32 hosts a web-based dashboard that streams live gyro angle, acceleration, and temperature data using Server-Sent Events (SSE).

Future versions will shift to STM32 and higher-accuracy sensors for better sampling, precision, and automotive-grade reliability.

📌 Project Overview

The system continuously reads gyroscope and accelerometer data from the MPU6050, corrects drift using predefined thresholds, and streams the processed values to a browser interface.
All UI files are served directly from the ESP32’s LittleFS, and calibration can be performed wirelessly using built-in reset endpoints.

🔧 Core Features

Real-time wheel angle tracking (X, Y, Z) using gyroscope accumulation

Drift correction thresholds for cleaner and more stable results

Accelerometer monitoring for tilt detection and motion verification

Temperature readings from the MPU6050

High-frequency live updates via SSE (10ms gyro, 200ms accel)

Web dashboard hosted directly on ESP32

Recalibration controls — reset all axes or individually reset X/Y/Z

LittleFS integration for serving HTML/CSS/JS files

🧠 How It Works
1️⃣ Sensor Initialization

The MPU6050 is initialized using Adafruit_MPU6050.
If the sensor is not detected, the ESP32 halts to avoid invalid readings.

2️⃣ WiFi + Web Server Setup

The ESP32 connects to WiFi and launches an AsyncWebServer on port 80.
Static dashboard files are served from LittleFS using server.serveStatic().

3️⃣ Sensor Data Streaming (SSE)

Three timed SSE channels push live data to the dashboard:

Stream	Endpoint Name	Frequency
Gyroscope	gyro_readings	Every 10 ms
Accelerometer	accelerometer_readings	Every 200 ms
Temperature	temperature_reading	Every 1 second
4️⃣ Gyroscope Drift Correction

Small fluctuations below these thresholds are ignored:

float gyroXerror = 0.07;
float gyroYerror = 0.03;
float gyroZerror = 0.01;


Valid changes are accumulated into:
gyroX, gyroY, gyroZ → representing wheel alignment angle deviations.

5️⃣ Reset/Calibration Endpoints

The ESP32 exposes easy calibration routes:

/reset     – reset all axes  
/resetX    – reset X axis  
/resetY    – reset Y axis  
/resetZ    – reset Z axis  

📁 Project Structure
/data
│── index.html
│── script.js
└── style.css

main.ino       → ESP32 firmware

🚀 Future Upgrades

Migration to STM32 for higher accuracy and better hardware timing

Adoption of advanced IMUs (ICM-20948, BMI270, BNO080, etc.)

Kalman / Complementary filtering for smoother angle estimation

Bluetooth / LoRa communication for vehicle-mounted deployments

Full UI upgrade with graphs, logs, and calibration tools
