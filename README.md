# WRO---Union
# Team Union — Obelix (WRO Future Engineers)

**Team name:** Union  
**Team members:** Abdullah Beshirov, Murad Mehdi, Jamil Hajizade  
**Repository owner:** GitHub: `Eisenwall`  
**Project:** Obelix — Autonomous self-driving car prototype for WRO Future Engineers  
**Robots included:** Obelix 1.0 (prototype), Obelix 2.0 (final model)

---

## Summary (project overview)

This repository contains the engineering documentation, source code, mechanical models, wiring diagrams and media for the Obelix self-driving robot series designed by Team Union for the WRO Future Engineers challenge. The goal of the project was to design and implement a compact autonomous 4-wheeled vehicle that can navigate a changing racetrack, detect colored pillars (traffic signs), respect passing rules (red-right / green-left) and perform a parallel parking maneuver.

Two physical prototypes are included:
- **Obelix 1.0** — initial prototype using Ackermann-style steering (servo SG90), Force-Up 6V 1000 RPM DC motor driving the differential assembly (LEGO differential used), 4× HC-SR04 ultrasonic sensors, TCS3200 color sensor, Arduino UNO as SBM for motor-control, XL4015 step-down, TB6612FNG motor driver.
- **Obelix 2.0** — refined model. Changes vs 1.0: Solid Axle Steering system (simpler steering), improved motor Force-Up 6V 625 RPM, added master switch for motors, improved battery layout and mechanical robustness. Core sensors and controllers remain similar, wiring improved for reliability.

This repository is intended to be **fully reproducible**: CAD/bin files (models), wiring diagram, bill of materials (BOM), code modules and engineering journal entries are provided.

---

## Design goals & constraints

- Vehicle size and mass suitable for WRO rules (compact, < 300×200×300 mm target).  
- Autonomous operation: the robot must start from a waiting state and begin only after the allowed start button is pressed. No RF/BT/WiFi control during runs.  
- Reliable detection of colored pillars (traffic signs) and robust lane following.  
- Safe power design: separate power for SBC/SBM and motors, main switches present, voltage regulator for motors and controllers.  
- Documentation and commit history meeting WRO guidelines (README ≥5000 characters, at least 3 commits).

---

## Hardware summary

**Chassis & mechanical:**
- Custom chassis, mixture of 3D printed parts and LEGO differential elements used for gear/differential on Obelix 1.0.
- Ackermann steering mechanism used in Obelix 1.0 (servo + linkages). In Obelix 2.0 replaced with Solid Axle Steering for simplicity and reliability.

**Drive & steering:**
- Force-Up 6V DC motors:
  - Obelix 1.0: Force-Up 6V 1000 RPM (Swedish-gearbox) — used to drive differential assembly.
  - Obelix 2.0: Force-Up 6V 625 RPM (lower RPM, higher torque) — final motor.
- Steering (1.0): SG90 servo controlling Ackermann linkage.
- Steering (2.0): Solid axle steering with mechanical tie rods.
- Differential assembly made from LEGO pieces (one gear drives the diff, long cross-shaped axles attach wheels).

**Electronics & sensors:**
- Arduino UNO (SBM) — motor/servo low-level control and sensor reading.
- TB6612FNG motor driver.
- XL4015 DC-DC buck converter for motor voltage stabilization.
- TCS3200 color sensor (front, used for pillar colour detection).
- 4× HC-SR04 ultrasonic sensors (front, left, right, rear) for obstacle detection and distance measurement.
- Power: 2× Li-ion 18650 cells for Arduino side (in holder), separate 6× AA battery pack for motors; two battery jacks present (one for each pack). Master power switch for motors on v2.
- Misc: breadboard area (for prototyping), connectors, bolts (M3 × 15 mm and smaller), tyre material (camera inner-tube used as tyre), ~40 wires used for wiring harness.

---

## Software & code layout

All code is in `/code`. Main architecture:
- `/code/drive_control/` — motor and steering low-level control, PWM wrappers, basic PID loops (if used).
- `/code/sensors/` — HC-SR04 reading, TCS3200 reading module, IMU calibration (if present).
- `/code/perception/` — color classification logic and debounce filters for pillar detection.
- `/code/planner/` — finite-state machine handling lap counting, corner transitions, emergency stops, parking manoeuvre.
- `/code/utils/` — serial communication to Arduino, logging and configuration loader.

**Main entrypoint:** `code/run.py` — starts services, opens serial to Arduino, waits for physical start button, logs run data to `/logs`.

**Notes on communication:** Arduino <-> SBC should use serial (UART) only. All wireless modules must be disabled during runs per WRO rules.

---

## How to reproduce / build (high-level)

1. Clone:
```bash
git clone https://github.com/Eisenwall/wro-union-obelix.git
cd wro-union-obelix
