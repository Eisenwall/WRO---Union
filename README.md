# WRO---Union
# Team Union — Obelix (WRO Future Engineers 2025)

**Project:** Obelix — Self-Driving Car (WRO Future Engineers)  
**Team name:** Union  
**Team members:** Abdullah Beshirov, Murad Mehdi, Jamil Hajizade  
**Robot name / versions:** Obelix 1.0 (prototype) — Obelix 2.0 (final)

---

## 1. Project overview

This repository documents the design, hardware, software, tests and evidence for the Obelix autonomous vehicle created for the WRO Future Engineers Self-Driving Cars challenge. The project goal is to produce a small four-wheeled steering vehicle that completes three autonomous laps on a randomized track, obeys coloured traffic pillars, and performs a parallel parking manoeuvre. Emphasis is on reproducible engineering: clear BOM, wiring, CAD/photos, annotated source code, test records and demonstration videos.

Obelix has two major versions:
- **Obelix 1.0** (prototype): differential drive with Ackerman-type steering mechanics for the front wheels controlled by a single SG90 servo; sensors: 4×HC-SR04 ultrasonic (front, left, right, rear), TCS3200 color sensor; controllers: Arduino UNO (SBM) + driver TB6612FNG; power regulation with XL4015 buck; drive motors: Force-Up 6V 1000 RPM brushed DC motors.
- **Obelix 2.0** (final): improvements include Solid Axle Steering system replacing Ackerman linkage, updated drive motors Force-Up 6V 625 RPM for better torque and controllability, addition of a main motor toggle switch, improved cable routing and battery sockets repositioned for balance and protection.

This README explains the hardware, wiring, software architecture, build and run steps, test results and reproducibility information.

---

## 2. Hardware summary

**Chassis & mechanical**
- Four-wheel car, 300×200×≤300 mm envelope (final dimensions must conform to rules).
- Differential assembly built from LEGO differential parts and cross axles, custom mounts, bicycle inner-tube material used for tires.
- Ackerman steering (1.0) → Solid Axle steering (2.0). Steering actuator: SG90 servo.

**Drive**
- 2 × Force-Up brush DC motors (prototype: 1000 RPM @ 6V; final: 625 RPM @ 6V). Specs (typical): nominal torque and stall torque as provided by vendor; full spec recorded in BOM.
- Motor driver: TB6612FNG (mounted but in our final layout kept modular so it can be attached/detached).

**Controls & electronics**
- Main microcontroller: Arduino UNO (SBM) — low-level motor & servo control, sensors sampling, serial interface.
- Voltage regulator / buck converter: XL4015 for motor supply regulation.
- Power:
  - 2 × Li-ion 18650 (in holder) — supply for Arduino/aux electronics via regulator as needed.
  - 6 × AA cell battery pack (for motor pack) in 2nd holder, with additional connector and main toggle switch (Obelix 2.0).
- Sensors:
  - 4 × HC-SR04 ultrasonic sensors (front, left, right, rear).
  - 1 × TCS3200 color sensor (front, for pillar color classification).
  - (Optional) wheel encoder(s) if available — our design supports 1 encoder on drive axle.
- Misc: push button start, wiring, breadboard junction, connectors, cable ties, mounting hardware.

Full BOM in `BOM.csv`.

---

## 3. Wiring & electrical notes

Key wiring notes (a simple diagram is provided in `docs/wiring_diagram.png` — see that file):
- Arduino GND must be common with motor driver GND and battery negative.
- TB6612FNG: VM connected to motor battery pack (after XL4015 if regulated), VCC to Arduino 5V (or regulated 5V), PWMs and DIR pins to Arduino digital pins per `code/arduino/motor_control.ino`.
- TCS3200 color sensor connected to digital pins for output and control frequency (documented in `/code/sensors/tcs3200/README.md`).
- HC-SR04 ultrasonic sensors connected with trig/echo pins; use simple software filters to remove spurious readings.
- Toggle switch (Obelix 2.0) placed in motor positive supply line to allow safe motor ON/OFF before starting electronics.

**Safety**: ensure motors and batteries are disconnected during upload of Arduino sketch. Judges may inspect that no RF modules are active.

---

## 4. Software architecture

`/code` is organized into modules:

- `/code/arduino/` — Arduino UNO sketches: `motor_control.ino` (motor and servo control, serial command interface), `sensors_read.ino` (optional).
- `/code/sbc/` — (optional) SBC-level scripts if using Raspberry Pi for vision. In Obelix we used Arduino-only stack for reliability; perception and color classification implemented on Arduino (TCS3200).
- `/code/drive_control/` — high-level motion primitives: `go_forward(distance)`, `turn(angle)`, `park_parallel()` — these are implemented as sequences of commands for Arduino motor controller.
- `/code/perception/` — color sensor routines and ultrasonic-based obstacle detection.
- `/code/utils/` — logging, config loader, calibration utilities.

The main run process (Arduino-only approach):
1. Power up and ensure start button is not pressed.
2. System enters WAIT state.
3. On judge "Go", press start button → Arduino executes main mission FSM: lap counting, pillar handling (left/right decision based on TCS3200), and parking routine.

---

## 5. How to build and run (quick)

1. **Clone the repository**
```bash
git clone https://github.com/<<your_account>>/wro-union-obelix.git
cd wro-union-obelix

