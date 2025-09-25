# WRO---Union
# Team Union — Obelix (WRO Future Engineers)

**Project name:** Obelix — Self-Driving Car (WRO Future Engineers)  
**Team name:** Union  
**Team members:** Abdullah Beshirov, Murad Mehdi, Jamil Hajizade  
**GitHub account:** Eisenwall  
**Repository structure:** `obelix/` contains `obelix_1.0/` and `obelix_2.0/` folders, documentation and code.

---

## 1. Project overview

This repository contains the design, firmware, mechanical notes and media for the Obelix self-driving vehicle developed by Team Union for the WRO Future Engineers category. The project has two main hardware versions:

- **Obelix 1.0** — initial prototype using Ackerman steering mechanism and the following sensors and components (see Hardware).  
- **Obelix 2.0** — improved final model using Solid Axle Steering system, improved motor and power-switch, structure and wiring improvements. The `obelix_2.0/new_changes` folder documents differences compared to Obelix 1.0.

The focus of the project is robust autonomous lane keeping, pillar (traffic sign) recognition (colour classification), obstacle avoidance using ultrasonic sensors, and reliable parallel parking manoeuvre. All documentation and source code included here aims to be reproducible for other teams.

---

## 2. Team and responsibilities

- **Abdullah Beshirov** — mechanical assembly, differential and drivetrain  
- **Murad Mehdi** — sensors integration, electronics wiring, power management  
- **Jamil Hajizade** — firmware, algorithms (obstacle avoidance and parking), repository & documentation

---

## 3. Short summary of Obelix 1.0 and 2.0

**Obelix 1.0 (prototype)**  
- Steering: **Ackerman steering mechanism** for front wheels.  
- Drive: differential power applied to the rear axle via a LEGO-based differential assembly. Two driving wheels powered by a single driving axle with differential gearing derived from LEGO parts (small gear + cross shafts).  
- Motors: 1 × *Force-Up 6V 1000 RPM* brushed DC motor (high torque).  
- Steering servo: 1 × SG90 (controls front pair via Ackerman linkages).  
- Sensors: 4 × HC-SR04 ultrasonic sensors (front, left, right, rear), 1 × TCS3200 color sensor (front).  
- Controller: **Arduino Uno** (main control and sensor reading).  
- Motor driver: **TB6612FNG** (drives DC motor; mounted on the chassis but not rigidly fixed).  
- Power: 2 × 18650 Li-ion cells for Arduino power and 6 × AA batteries for motor supply; XL4015 module used for regulating motor supply where necessary.  
- Chassis, tires: custom chassis, drive shafts and a tire made from bicycle camera rubber for traction.

**Obelix 2.0 (final)**  
- Steering: **Solid Axle Steering** (mechanically simpler and more robust). Ackerman replaced.  
- Motor replaced with **Force-Up 6V 625 RPM** (lower RPM, higher torque).  
- Added main toggle switch (tumbler) for motor on/off.  
- Wiring, cabling and battery sockets reorganised for reliability.  

---

## 4. Hardware (detailed list)

All components (more details in `obelix/*/components/details.txt`):

- Arduino UNO — 1 pcs  
- HC-SR04 Ultrasonic sensor — 4 pcs (front, left, right, rear)  
- TCS3200 Color sensor — 1 pcs (front)  
- SG90 micro servo — 1 pcs (steering for Obelix 1.0)  
- Force-Up DC Motor (6V, 1000 RPM) — Obelix 1.0 — 1 pcs  
- Force-Up DC Motor (6V, 625 RPM) — Obelix 2.0 — 1 pcs  
- Motor driver TB6612FNG — 1 pcs  
- XL4015 (step-down / current regulator) — 1 pcs  
- Li-ion 18650 battery holder / 2 cells — 1 set (for Arduino)  
- AA battery holder (6×AA) — 1 set (for motor pack)  
- Breadboard (partial removed piece used in chassis) — small section mounted on back  
- Mechanical parts: LEGO differential piece + cross axles, custom links for Ackerman, bolts (Ø3mm × 15mm and smaller), cable ties, wheel rims, rubber tyre (from bicycle camera)  
- Cables: ~40 wires used for traction banding and wiring

(See `BOM.csv` for table version.)

---

## 5. Software architecture and files

This project uses a single microcontroller (Arduino Uno) as main controller for both sensors and actuators. The code is organized as:

/code  
/arduino  
obelix.ino <-- main firmware  
drive_control.h/.cpp  
sensors.h/.cpp  
config.h <-- pin mapping, thresholds and calibration constants  

/tools  
serial_logger.py <-- optional serial log viewer (desktop)

**Main behaviour (firmware)**:  
- Read sensors (HC-SR04 x4, TCS3200)  
- Low-level motor control through TB6612FNG (PWM and direction)  
- Steering control through SG90 (PWM for Ackerman) or Solid Axle linkage in v2.0  
- Finite state machine for: `WAIT_START -> LANE_FOLLOW -> PILLAR_HANDLING -> PARKING_MANEUVER -> STOP`  
- Logging via Serial (for debug)

---

## 6. Pin mapping (recommended, edit `config.h` as needed)

- HC-SR04 front: TRIG D2, ECHO D3  
- HC-SR04 left: TRIG D4, ECHO D5  
- HC-SR04 right: TRIG D6, ECHO D7  
- HC-SR04 rear: TRIG D8, ECHO D9  
- TCS3200 S0..S3: D10..D13 (or analog read if using frequency counting)  
- Servo: D11 (PWM)  
- TB6612FNG: PWM_A D5, AIN1 D12, AIN2 D13 (adjust as needed)  
- Start push button: digital pin A0 (if used)

---

## 7. How to build / compile / upload (Arduino)

1. Install Arduino IDE (latest stable).  
2. Open `code/arduino/obelix.ino`.  
3. Edit `config.h` to match your wiring (pins and battery voltages).  
4. Select board `Arduino Uno`, correct COM port.  
5. Compile and Upload.  
6. Place robot in starting zone, switch batteries on, press start (or send serial start if configured).

---

## 8. Tests, videos and performance

**Videos:** Put links in `videos.txt` and in this README when uploaded (YouTube unlisted/public). Each challenge must have at least one demo ≥30 seconds.

**Expected Behaviours:**  
- Lane follow stable (tested on practice mat)  
- Pillar detection via TCS3200 (red vs green classification)  
- Parallel parking routine triggered after lap completion

---

## 9. Reproducibility & files to reproduce the build

- `obelix_1.0/components/details.txt` — component list and small CAD notes  
- `obelix_1.0/instructions_to_create.txt` — assembly guide (mechanical connections, bolts, link lengths)  
- `obelix_2.0/new_changes/*` — changes from 1.0 to 2.0  
- `/models` — STL / CAD files (3D models to be uploaded)  
- `docs/wiring_diagram.png` — wiring diagram (add when available)

---

## 10. Commit history requirements (WRO)

- Commit 1 — not later than 2 months before competition (≥20% of final code)  
- Commit 2 — not later than 1 month before competition  
- Commit 3 — not later than 2 weeks before competition  

Repository must be **public** for at least 12 months.

---

## 11. Licence & credits

- MIT License (`LICENSE`)  
- Hardware components: suppliers listed in BOM  
- Team-built mechanical parts and software modules credited in `CREDITS.md`

---

## 12. Contact

Team Union — Team Captain: Jamil Hajizade — `<<team.email@example.com>>`

---

# Engineering Journal — Team Union — Obelix

**Team:** Union  
**Robots:** Obelix 1.0 (prototype), Obelix 2.0 (final)  
**Authors:** Abdullah Beshirov, Murad Mehdi, Jamil Hajizade  

---

## Entry 1 — Mobility Management (design & choices)
**Date:** 2025-09-25  
**Goal:** Design reliable drivetrain and steering that meet WRO rules (4 wheels, one driving axle, one steering actuator).  

**Components used:**  
- Drive motor (Obelix 1.0): Force-Up 6V 1000 RPM  
- Drive motor (Obelix 2.0): Force-Up 6V 625 RPM  
- Differential assembly: LEGO differential piece + cross shafts  
- Steering: SG90 + Ackerman linkage (v1.0); Solid Axle (v2.0)  

**Design rationale:**  
- Single driving axle with differential complies with WRO rules. LEGO differential allows independent wheel rotation.  
- Ackerman servo used for simplicity; replaced with Solid Axle for repeatable steering in final version.  
- Bicycle camera rubber used on rims for traction.  

**Testing:**  
- 10 straight runs at 6V; corrected slight right drift by adjusting servo and mounting.  
- Overheating risk tested; duty cycles recommended.

---

## Entry 2 — Power & Sensor Management
**Date:** 2025-09-25  
**Power system:**  
- Arduino: 2 × 18650 Li-ion (regulated).  
- Motors: 6 × AA batteries, TB6612FNG, XL4015 for regulation.  
- Obelix 2.0: added main toggle switch.  

**Sensors:**  
- HC-SR04 x4 for obstacle detection (front, left, right, rear)  
- TCS3200 for pillar colour detection  

**Testing & measurements:**  
- Idle motor current ~30–40 mA; stall 2.6–3.2 A  
- Ultrasonic reliable for 5–200 cm, filtered in firmware  

---

## Entry 3 — Obstacle Management & Algorithms
**Date:** 2025-09-25  
**Goal:** Describe obstacle and parking strategy  

**Strategy summary:**  
- Finite state machine:  
  - `START` → `FOLLOW_LANE` → while laps < 3: `PILLAR_RESPONSE` → if lap_complete: `PARKING_SEQUENCE` → `STOP`  
- PILLAR_RESPONSE:  
  - TCS3200 reads pillar colour  
  - GREEN → left-bias trajectory  
  - RED → right-bias trajectory  
- LANE FOLLOW:  
  - Ultrasonic left+right sensors for lane centering  
  - Front sensor for obstacle proximity  
- PARKING:  
  - Predefined parallel parking trajectory using distances (coordinate-free)  
