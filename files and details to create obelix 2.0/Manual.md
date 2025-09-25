# How to Build Our Robot

This guide explains how to create our robot step by step.

---

## 1. Components Required

Before starting, make sure you have the following components:

- **Servomotor:** SG90  
- **DC Motor:** Force-Up 6V 1000 RPM Carbon Brushed DC Motor  
- **Motor Driver:** TB6612FNG  
- **Microcontroller:** Arduino Uno  
- **Ultrasonic Sensors:** 4 × HC-SR04  
- **Color Sensor:** TCS3200  
- **Accumulators (Batteries):** 2 (one for the motors, one for Arduino)  
- **Cables:** Appropriate Arduino cables  
- **Wheels:** You can use old camera tires or similar for the robot wheels  
- **Optional:** DC-DC converter for voltage regulation, BMS module to protect your batteries  

---

## 2. 3D Printed Parts

You will need to 3D print the following components for your robot:

- **Bottom Part (Corpus Base):**  
  [buttompart.stl](https://github.com/Eisenwall/WRO---future-engineers/blob/main/files%20for%20creating%20robot/buttompart.stl)  

- **Motor Converter (Connect Motors to LEGO Parts):**  
  [converter for motor.stl](https://github.com/Eisenwall/WRO---future-engineers/blob/main/files%20for%20creating%20robot/converter%20for%20motor.stl)  

- **Turning Mechanism Parts:**  
  [details for turning0.stl](https://github.com/Eisenwall/WRO---future-engineers/blob/main/files%20for%20creating%20robot/details%20for%20turning0.stl)  
  [details for turning1.stl](https://github.com/Eisenwall/WRO---future-engineers/blob/main/files%20for%20creating%20robot/details%20for%20turning1.stl)  

- **Wheels:**  
  [forward and back wheels.stl](https://github.com/Eisenwall/WRO---future-engineers/blob/main/files%20for%20creating%20robot/forward%20and%20back%20wheels.stl)  
  [forward wheels.stl](https://github.com/Eisenwall/WRO---future-engineers/blob/main/files%20for%20creating%20robot/forward%20wheels.stl)  

- **Middle Part of Robot:**  
  [middlepart.stl](https://github.com/Eisenwall/WRO---future-engineers/blob/main/files%20for%20creating%20robot/middlepart.stl)  

- **Top Part of Robot:**  
  [toppart.stl](https://github.com/Eisenwall/WRO---future-engineers/blob/main/files%20for%20creating%20robot/toppart.stl)  

---

## 3. Assembly Notes

1. Start by printing all 3D parts and checking their fit.  
2. Assemble the bottom part first, then attach the middle and top parts.  
3. Install motors and connect them via the motor converter.  
4. Mount the wheels and check for smooth rotation.  
5. Install sensors (ultrasonic and color) in their respective positions.  
6. Connect the Arduino and motor driver using the cables.  
