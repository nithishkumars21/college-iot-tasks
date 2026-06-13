# college-iot-tasks
object detection, green house monitoring, ldr light intensity


# 🌿 IoT Lab – 6th Semester | Embedded System & IoT Lab
**E.G.S. Pillay Engineering College** | B.E. Electronics & Communication Engineering (Reg. 2023)

This repository contains hands-on lab experiments built with the **ESP8266 NodeMCU** and **Blynk IoT platform**, covering real-world IoT use cases: environment monitoring, light-based automation, and object detection.

---

## 📁 Repository Structure

```
📦 IoT-Lab-6thSem
 ┣ 📄 greenhouse_monitoring.ino   → Experiment 1: Greenhouse Environment Monitor
 ┣ 📄 ldr_sensor.ino              → Experiment 2: LDR-Based Automatic Light Control
 ┣ 📄 object_detection.ino        → Experiment 3: IR Sensor Object Detection
 ┗ 📄 README.md
```

---

## ⚙️ Hardware & Platform Requirements

| Component | Details |
|---|---|
| Microcontroller | ESP8266 NodeMCU |
| Sensors | DHT11, LDR, IR Sensor |
| Cloud Platform | Blynk IoT (v2.0) |
| IDE | Arduino IDE |
| Libraries | `ESP8266WiFi`, `BlynkSimpleEsp8266`, `DHT` |

---

## 🔐 Credentials Setup (IMPORTANT)

> **Do NOT hardcode your credentials in the sketch.** Create a `secrets.h` file and add it to `.gitignore`.

**Create `secrets.h`:**
```cpp
#define BLYNK_TEMPLATE_ID   "your_template_id"
#define BLYNK_TEMPLATE_NAME "your_template_name"
#define BLYNK_AUTH_TOKEN    "your_auth_token"

#define WIFI_SSID "your_wifi_ssid"
#define WIFI_PASS "your_wifi_password"
```

**Add to `.gitignore`:**
```
secrets.h
```

**Include in your sketch:**
```cpp
#include "secrets.h"
```

---

## 🧪 Experiment 1: Greenhouse Environment Monitor

### 📌 Objective
Monitor temperature, humidity, and light intensity in real time via the Blynk dashboard — simulating a smart greenhouse environment.

### 🔩 Components
- ESP8266 NodeMCU
- DHT11 Temperature & Humidity Sensor
- LDR (Light Dependent Resistor)
- 10kΩ Resistor

### 🔌 Circuit Connections

| Component | ESP8266 Pin |
|---|---|
| DHT11 Data | D5 |
| LDR Output | A0 |
| LDR Divider | 3.3V → LDR → A0 → 10kΩ → GND |

### 📲 Blynk Virtual Pins

| Virtual Pin | Data |
|---|---|
| V2 | LDR raw value (0–1023) |
| V3 | Temperature (°C) |
| V4 | Humidity (%) |

### 🔄 Working
- Data is read every **500 ms** using a non-blocking `millis()` timer.
- DHT11 provides temperature and humidity; LDR gives ambient light level.
- All values are pushed to the Blynk app for live visualization.

### 📊 Sample Serial Output
```
LDR: 712 | Temp: 31.00 C | Humidity: 65.00 %
LDR: 685 | Temp: 31.00 C | Humidity: 65.00 %
```

---

## 🧪 Experiment 2: LDR-Based Automatic Light Control

### 📌 Objective
Automatically turn an LED ON or OFF based on ambient light levels — and reflect the state on the Blynk dashboard.

### 🔩 Components
- ESP8266 NodeMCU
- LDR
- LED
- 10kΩ Resistor

### 🔌 Circuit Connections

```
3.3V ── LDR ── A0 ── 10kΩ ── GND
LED anode → D4 (via current-limiting resistor) → GND
```

> **Note:** Bright light → higher ADC value. Darkness → lower ADC value.

### 📲 Blynk Virtual Pins

| Virtual Pin | Data |
|---|---|
| V0 | LED state (1 = ON, 0 = OFF) |

### ⚡ Logic

| LDR ADC Value | Condition | LED |
|---|---|---|
| < 500 | Dark / Low light | ON |
| ≥ 500 | Bright / Sufficient light | OFF |

### 🔄 Working
- LDR value is read every **500 ms**.
- Threshold is set to **500** (can be made dynamic via Blynk slider on V_THRESHOLD).
- LED state is written to Blynk for remote monitoring.

### 📊 Sample Serial Output
```
ldrValue: 312
ledON
ldrValue: 756
ledOFF
```

---

## 🧪 Experiment 3: IR Sensor Object Detection with Blynk Alert

### 📌 Objective
Detect the presence of an object using an IR sensor and trigger a real-time alert on the Blynk dashboard.

### 🔩 Components
- ESP8266 NodeMCU
- IR Proximity Sensor (digital output)

### 🔌 Circuit Connections

| Component | ESP8266 Pin |
|---|---|
| IR Sensor OUT | D5 |
| IR Sensor VCC | 3.3V / 5V |
| IR Sensor GND | GND |

### 📲 Blynk Virtual Pins

| Virtual Pin | Data |
|---|---|
| V0 | "Object Detected" / "No Object" (String) |

### ⚡ Logic

| IR Output (Digital) | Meaning |
|---|---|
| LOW (0) | Object Detected |
| HIGH (1) | No Object |

### 🔄 Working
- IR sensor is polled every **500 ms** using non-blocking `millis()`.
- On detection: sends `"Object Detected"` to Blynk V0 and triggers a **Blynk Event Log** (`object_detected`).
- On clear: sends `"No Object"` to Blynk V0.

### 📊 Sample Serial Output
```
Connecting to WiFi and Blynk...
Blynk Connected
Object Detected
Object Detected
No Object
```

---

## 🚀 Getting Started

### 1. Install Arduino IDE Dependencies
Go to **Sketch → Include Library → Manage Libraries** and install:
- `Blynk` by Volodymyr Shymanskyy
- `DHT sensor library` by Adafruit
- `ESP8266` board package (via Board Manager)

### 2. Configure Board
- Go to **Tools → Board → ESP8266 Boards → NodeMCU 1.0 (ESP-12E Module)**
- Port: Select the correct COM port

### 3. Set Up Credentials
Create a `secrets.h` file as described in the [Credentials Setup](#-credentials-setup-important) section above.

### 4. Create Blynk Template
- Sign in at [blynk.cloud](https://blynk.cloud)
- Create a new template and configure virtual pins as described per experiment
- Copy your `Template ID`, `Template Name`, and `Auth Token` into `secrets.h`

### 5. Upload Sketch
- Open the `.ino` file for the desired experiment
- Click **Upload**

---

## 📚 Key Concepts Demonstrated

- **ESP8266 WiFi connectivity** with cloud IoT platforms
- **Non-blocking timing** using `millis()` instead of `delay()`
- **Sensor interfacing**: Analog (LDR, DHT11) and Digital (IR)
- **Blynk virtual pins** for bidirectional cloud communication
- **Event logging** on Blynk for alert-based IoT applications

---

## ⚠️ Known Limitations

- DHT11 has ±2°C / ±5% RH accuracy — not suitable for precision applications.
- LDR threshold (500) is hardcoded; calibrate based on your ambient conditions.
- Blynk free tier has limited event log entries per day.
- ESP8266 operates at 3.3V logic — do not connect 5V sensors directly without a level shifter.

---

## 👨‍💻 Author

**Nithishkumar** — B.E. ECE, 6th Semester (Reg. 2023)
E.G.S. Pillay Engineering College

---

## 📄 License

This project is for **academic/educational purposes** only.
