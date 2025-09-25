# How to Build Obelix Robots

This guide explains how to create both versions of our robot step by step. Instructions for **Obelix 1.0** (prototype) and **Obelix 2.0** (final model) are provided separately.

---

## Obelix 1.0 — Prototype

### 1. Components Required

- **Servomotor:** SG90  
- **DC Motor:** Force-Up 6V 1000 RPM Carbon Brushed DC Motor  
- **Motor Driver:** TB6612FNG  
- **Microcontroller:** Arduino Uno  
- **Ultrasonic Sensors:** 4 × HC-SR04  
- **Color Sensor:** TCS3200  
- **Accumulators (Batteries):** 2 (one for motors, one for Arduino)  
- **Cables:** Appropriate Arduino cables  
- **Wheels:** Old camera tires or similar  
- **Optional:** DC-DC converter for voltage regulation, BMS module for battery protection  

### 2. 3D Printed Parts

- **Bottom Part (Corpus Base):** [https://github.com/Eisenwall/WRO---Union/blob/main/files%20and%20details%20to%20create%20obelix/buttompart.stl]  
- **Motor Converter:** [https://github.com/Eisenwall/WRO---Union/blob/main/files%20and%20details%20to%20create%20obelix/converter%20for%20motor.stl]  
- **Turning Mechanism Parts1:** [https://github.com/Eisenwall/WRO---Union/blob/main/files%20and%20details%20to%20create%20obelix/details%20for%20turning0.stl]
- **Turning Mechanism Parts2:** [https://github.com/Eisenwall/WRO---Union/blob/main/files%20and%20details%20to%20create%20obelix/details%20for%20turning1.stl] 
- **Wheels:** [https://github.com/Eisenwall/WRO---Union/blob/main/files%20and%20details%20to%20create%20obelix/forward%20and%20back%20wheels.stl]  
- **Middle Part of Robot:** [https://github.com/Eisenwall/WRO---Union/blob/main/files%20and%20details%20to%20create%20obelix/middlepart.stl]  
- **Top Part of Robot:** [https://github.com/Eisenwall/WRO---Union/blob/main/files%20and%20details%20to%20create%20obelix/toppart.stl]  

### 3. Assembly Notes

1. Print all 3D parts and verify fit.  
2. Assemble the **bottom part first**, then attach the **middle** and **top parts**.  
3. Install motors and connect them via the motor converter.  
4. Mount the wheels and check for smooth rotation.  
5. Install sensors (ultrasonic and color) in the designated positions.  
6. Connect the Arduino and motor driver using cables.  
7. Mount the servo for **Ackerman steering** to control the front wheels.  
8. Arrange batteries and power connectors so they do not interfere with wheels or sensors.  
