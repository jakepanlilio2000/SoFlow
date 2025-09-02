# SOFLOW Irrigation System

This repository documents the **SOFLOW Project**, an end-to-end smart irrigation system design. It includes the full Bill of Materials (BOM), wiring and schematics, firmware structure, cloud integration, and safety protocols.  
This README provides **previews and explanations** to guide you through implementation.

---

## System Architecture Overview

The system uses a distributed, robust architecture:

- **Node 1 & Node 2 (ESP32):**  
  Independent field units monitoring soil moisture, showing local status on LCDs, and controlling 12V solenoid valves. They operate autonomously and sync data with Firebase.

- **Main Controller (ESP32 Gateway + Arduino Mega):**  
  - **Arduino Mega:** Dedicated to RS-485 NPK sensor polling and high-current pump control.  
  - **ESP32 Gateway:** Handles coordination, irrigation logic, water level sensing, Firebase integration, and communication with nodes.

---

## 1. Bill of Materials (BOM)

A detailed table lists all required components:

- **Microcontrollers:** ESP32 boards, Arduino Mega  
- **Sensors:** Capacitive soil moisture, RS-485 NPK, water level switch  
- **Actuators:** 12V solenoids, 12V DC pump, buzzer  
- **Power & Safety:** SLA battery, buck converters, fuses, diodes, TVS protection  
- **Communication & Display:** RS-485 module, I2C LCDs  
- **Enclosures & Wiring:** Weatherproof housings, screw terminals, appropriate wire gauges  

---

## 2. Wiring and Schematics

### Key Wiring Rules
- All components share a **common ground**.  
- **Buck converters** provide stable 5V and 3.3V rails.  
- **MOSFETs** switch solenoids and pump with flyback diodes for protection.  

### Node Schematic (Preview)
- Soil moisture sensor → ESP32 analog pin  
- LCD via I2C (SDA/SCL)  
- Solenoid driven by MOSFET, protected with flyback diode  

### Main Controller (Preview)
- Arduino Mega handles pump and RS-485 sensor  
- ESP32 Gateway communicates via I2C  
- Safety fuses, reverse polarity diodes, and transient suppression included  

---

## 3. Firmware (Overview)

Firmware is structured for each unit:

- **Node Firmware (ESP32):**  
  - Reads soil moisture, shows it on LCD, syncs with Firebase  
  - Receives solenoid commands remotely  

- **Main Controller - Arduino Mega:**  
  - Polls NPK sensor via RS-485  
  - Manages pump and buzzer  
  - Shares status via I2C to the ESP32 Gateway  

- **Main Controller - ESP32 Gateway:**  
  - Aggregates data from Firebase and Mega  
  - Runs irrigation logic (threshold-based)  
  - Sends solenoid commands and updates Firebase  

---

## 4. Cloud Integration & Mobile App

The system uses **Firebase Realtime Database** with a structured JSON layout:

- **Nodes** store soil moisture data and solenoid states  
- **Main** stores pump, water level, and NPK data  
- **Config** allows threshold settings and manual overrides  

**Mobile App Functions:**
- View sensor readings and pump status  
- Override solenoid commands  
- Switch between automatic and manual irrigation  

---

## 5. Safety, Commissioning, and Troubleshooting

### Safety
- Always fuse each branch (pump, solenoids, logic)  
- Use proper wire gauge for current loads  
- Place all electronics in weatherproof enclosures  

### Commissioning Steps
1. Test each node independently  
2. Integrate Arduino Mega and ESP32 Gateway  
3. Connect to Firebase and verify live data  
4. Simulate dry/wet conditions and water level safety cutoff  

### Troubleshooting Guide
- **No Wi-Fi:** Check SSID/password and coverage  
- **Blank LCD:** Confirm I2C address and wiring  
- **No pump/valve action:** Verify MOSFET wiring, fuses, and supply voltage  
- **Incorrect moisture readings:** Recalibrate dry/wet sensor constants  
- **No Firebase updates:** Confirm database URL and rules  

---

## Summary

This project provides a **scalable smart irrigation system** with distributed nodes, a robust dual-MCU main controller, Firebase cloud connectivity, and safety-first power management.  
By following the BOM, wiring previews, firmware structure, and commissioning steps, you can build and deploy the SOFLOW system reliably.
