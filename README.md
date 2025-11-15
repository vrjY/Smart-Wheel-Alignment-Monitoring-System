Smart Wheel Alignment Monitoring System – ESP32 + MPU6050

This project implements a real-time wheel alignment monitoring system using an ESP32 and an MPU6050 IMU. The ESP32 hosts a lightweight web dashboard that streams live orientation, acceleration, and temperature data, making it possible to track wheel alignment angles accurately and wirelessly.

Future versions of the system will transition to STM32 and higher-precision IMUs for improved sampling rates, reliability, and automotive-grade performance.

📌 Project Overview

The system reads orientation and acceleration data from the MPU6050 sensor and transmits it to a browser-based interface using Server-Sent Events (SSE).
Gyroscope drift is compensated using adjustable error thresholds, and live readings are continuously streamed for visualization.

The ESP32 serves a web interface from LittleFS and handles commands such as resetting gyro offsets, making it easy to recalibrate the system without reflashing the board.

🔧 Core Features

Real-time gyro angle tracking (X, Y, Z) with drift correction

Live accelerometer data for secondary motion/tilt verification

MPU6050 temperature monitoring

ESP32-hosted web dashboard using AsyncWebServer

Server-Sent Events stream for high-frequency sensor updates

Reset controls (reset all axes or individual X/Y/Z readings)

File hosting through LittleFS to serve HTML/CSS/JS dashboard files

🧠 How It Works (Code Breakdown)
1. Sensor Initialization

Adafruit_MPU6050 is used to initialize and configure the IMU.
If the sensor isn’t detected, the system halts to prevent faulty readings.

2. WiFi + Web Server Setup

The ESP32 connects to a WiFi network and launches an AsyncWebServer on port 80.
Static files (index.html, etc.) are served from LittleFS.

3. Data Streaming using SSE

Three independent, timed streams send:

Gyro angles (gyro_readings) every 10 ms

Accelerometer values (accelerometer_readings) every 200 ms

Temperature (temperature_reading) every 1 second

This ensures smooth real-time updates on the dashboard.

4. Gyroscope Drift Adjustment

Raw gyro changes below defined thresholds are ignored:

float gyroXerror = 0.07;
float gyroYerror = 0.03;
float gyroZerror = 0.01;


Valid movements are accumulated into gyroX, gyroY, and gyroZ—representing wheel angle deviations.

5. Reset Endpoints

The ESP32 exposes the following routes for quick recalibration:

/reset → reset all axes

/resetX → reset X axis

/resetY → reset Y axis

/resetZ → reset Z axis

📁 Project File Structure
/data
   ├── index.html
   ├── script.js
   └── style.css

/main.ino   → Main ESP32 code

🚀 Future Improvements

Migration to STM32 MCU for automotive-grade performance

Integration of better IMU sensors (BMI270, ICM20948, etc.)

More accurate drift modeling and Kalman/Complementary filters

Bluetooth/LoRa connectivity for vehicle-mounted deployments
