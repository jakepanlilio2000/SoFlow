
# SOFLOW Irrigation System

The **SOFLOW Project** is a complete **smart irrigation system** designed for farms and gardens.  
It provides **autonomous irrigation**, **real-time monitoring**, and **cloud integration** through Firebase.  

This documentation serves as the **master README** – guiding you from **hardware setup** to **firmware structure**, **cloud integration**, and **safety procedures**.  
All code, wiring, and firmware are modular and scalable.

---

## Table of Contents
1. [System Architecture](#%EF%B8%8F-system-architecture)
2. [Bill of Materials (BOM)](#-bill-of-materials-bom)
3. [Wiring and Schematics](#-wiring-and-schematics)
4. [Firmware Structure](#%EF%B8%8F-firmware-structure)
5. [Cloud Integration & Mobile App](#%EF%B8%8F-cloud-integration--mobile-app)
6. [Safety & Power Management](#-safety--power-management)
7. [Commissioning Guide](#-commissioning-guide)
8. [Troubleshooting](#%EF%B8%8F-troubleshooting)
9. [Future Improvements](#-future-improvements)
10. [License](#-license)

---

## System Architecture

The system is designed with **distributed intelligence** for redundancy and reliability:

- **Field Nodes (ESP32):**
  - Monitor soil moisture in specific zones  
  - Display status on **I2C LCDs**  
  - Control **12V solenoid valves**  
  - Sync all data to **Firebase Cloud**  

- **Main Controller (ESP32 Gateway + Arduino Mega):**
  - **Arduino Mega**
    - Polls **RS-485 NPK sensor**
    - Drives the **12V pump** and **buzzer**
  - **ESP32 Gateway**
    - Acts as **cloud bridge**
    - Handles **water level safety cutoff**
    - Aggregates and syncs system data  

 **Diagram (placeholder)**  
![System Architecture](docs/system_architecture.png)

---

##  Bill of Materials (BOM)

| Category            | Components                                                                 |
|---------------------|-----------------------------------------------------------------------------|
| **MCUs**           | 2x ESP32 DevKit V1, 1x Arduino Mega                                         |
| **Sensors**        | Capacitive soil moisture, RS-485 NPK, float-type water level switch         |
| **Actuators**      | 12V solenoid valves, 12V DC pump, buzzer                                    |
| **Power & Safety** | 12V SLA battery, buck converters (12V→5V & 12V→3.3V), fuses, diodes, TVS    |
| **Comms & Display**| MAX485 modules, I2C 16x2 LCDs                                               |
| **Enclosures**     | IP65 weatherproof boxes, waterproof connectors, screw terminals             |
| **Wiring**         | AWG 22 for signals, AWG 16–18 for pump/valve loads                          |

 **Diagram (placeholder)**  
![Bill of Materials](docs/bom_layout.png)

---

## 🔌 Wiring and Schematics

### 🔹 Field Node (ESP32)
- Soil moisture → **Analog pin**  
- LCD → **I2C (SDA/SCL)**  
- Solenoid → **MOSFET + diode**  

 **Node Wiring Preview**  
![Node Wiring Diagram](docs/node_wiring.png)

---

### 🔹 Main Controller
- Arduino Mega ↔ MAX485 ↔ NPK Sensor  
- Mega → MOSFET → Pump (with flyback diode & fuse)  
- Mega ↔ ESP32 Gateway via I2C  
- Water level sensor → ESP32 GPIO  

 **Main Controller Schematic**  
![Main Controller Wiring](docs/main_controller.png)

---

## ⚙️ Firmware Structure

### 🔹 Field Node (ESP32)
- Reads soil moisture  
- Displays data on LCD  
- Sends updates to Firebase  
- Executes solenoid commands  

### 🔹 Arduino Mega
- Polls NPK sensor via RS-485  
- Controls pump & buzzer  
- Shares data with ESP32 Gateway  

### 🔹 ESP32 Gateway
- Syncs with Firebase  
- Applies irrigation logic  
- Distributes commands to nodes  

 **Firmware Flowchart (placeholder)**  
![Firmware Flow](docs/firmware_flow.png)

---

## ☁️ Cloud Integration & Mobile App

The system uses **Firebase Realtime Database** with a structured JSON schema:

```json
{
  "Nodes": {
    "Node1": { "Moisture": 35, "Valve": "ON" },
    "Node2": { "Moisture": 42, "Valve": "OFF" }
  },
  "Main": {
    "Pump": "ON",
    "WaterLevel": "LOW",
    "NPK": { "N": 55, "P": 23, "K": 40 }
  },
  "Config": {
    "AutoMode": true,
    "MoistureThreshold": 40
  }
}
````

### 📱 Mobile App Features

* Real-time soil and NPK readings
* Pump & solenoid manual override
* Auto/manual irrigation toggle
* Water level alerts

 **Mobile App Preview (placeholder)**
![Mobile App](docs/mobile_app.png)

---

## Safety & Power Management

* **Fuse every branch:** Pump (5–10A), solenoids (2A each), logic (1A)
* **Flyback diodes** on all inductive loads
* **TVS diodes** for surge protection
* **Reverse polarity diodes** on battery inputs
* **Weatherproof enclosures** for all electronics

 **Safety Diagram (placeholder)**
![Safety Wiring](docs/safety.png)

---

## Commissioning Guide

1. Power and test **each node individually**
2. Verify **LCD readings** and Firebase sync
3. Connect **pump and valves** with fuses
4. Simulate **low water level cutoff**
5. Enable **automatic irrigation** and test

---

## 🛠️ Troubleshooting

| Problem                 | Likely Cause                     | Fix                                     |
| ----------------------- | -------------------------------- | --------------------------------------- |
| No Wi-Fi connection     | Wrong SSID/password, weak signal | Check credentials, add external antenna |
| LCD not showing         | Wrong I2C address, wiring issue  | Scan I2C bus, fix wiring                |
| Pump/valves not working | MOSFET wired wrong, fuse blown   | Rewire MOSFET, check fuses              |
| Wrong moisture readings | Calibration off                  | Recalibrate dry/wet values              |
| Firebase not updating   | Wrong URL or rules               | Verify Firebase config & database rules |

---

## Future Improvements

* Add **LoRa modules** for long-range communication
* Integrate **AI irrigation predictions**
* Solar charging with **MPPT controller**
* Expand to **multi-zone irrigation** with more nodes

---

## License

This project is released under the **MIT License**.
Free to use, modify, and distribute with attribution.

---

✨ With **SOFLOW**, your irrigation system becomes **smarter, safer, and scalable**.
