# 🏥 Hospital Monitoring System

An Arduino-based patient monitoring system built as a university project. It covers environmental safety, biometric sensing, medication management, and fall detection — with all data reported to Google Sheets via Wi-Fi.

---

## Modules

**Heart Rate Monitor**  
An Arduino reads data from a Pulse sensor, a homemade GSR sensor, and an MPU6050 at 100Hz and sends it over serial to MATLAB. MATLAB handles all signal processing — bandpass/lowpass filtering, peak detection, and BPM calculation — and classifies the patient's state (Resting, Active, Stressed, or Panic Attack) in a live UI.

**Time Medicine Reminder & Glucose Detection**  
An RTC keeps track of time and triggers servo motors to open medicine box compartments at scheduled intervals. An LCD shows the current time and next dose, and a buzzer + button confirm the patient took their medicine. A water level sensor on the same board monitors glucose levels and buzzes on abnormal readings.

**Safe Climate Keeper**  
Monitors room temperature and humidity via a DHT sensor (displayed on LCD). An MQ2 gas sensor and IR flame sensor watch for hazards — a buzzer only triggers when both smoke and flame are detected simultaneously to avoid false alarms.

**Fall Detection & Data Storage**  
An MPU6050 tracks patient orientation and triggers a buzzer if a fall is detected (roll > ±60°). An ESP8266 Wi-Fi module on the same board collects data from all modules and sends periodic reports to Google Sheets via the Google Sheets API.

> The project can be built with as few as **2 Arduinos** — one dedicated to the Heart Rate Monitor (required for MATLAB serial at 100Hz), and one handling all other modules.

---

## Setup

**Arduino**
1. Install [Arduino IDE](https://www.arduino.cc/en/software)
2. Install these libraries via Library Manager: `RTClib`, `DHT sensor library`, `LiquidCrystal_I2C`, `ArduinoJson`, `ESP8266WiFi`, `ESP8266HTTPClient`
3. Fill in your Wi-Fi credentials and Google Script URL in `arduino/config/config.h`
4. Flash each `.ino` to its Arduino

**MATLAB**
1. Required toolboxes: Signal Processing, Instrument Control
2. Update the `serialPort` variable in `matlab/heart_rate_dsp/main.m` to match your COM port
3. Run `main.m` — the live UI starts automatically

**Google Sheets**
1. Create a Google Sheet → Extensions → Apps Script
2. Deploy a Web App that accepts POST requests and appends rows
3. Paste the deployment URL into `config.h` as `SCRIPT_URL`

---

## License

MIT License
