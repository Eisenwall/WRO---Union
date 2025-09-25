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
- Motor driver: **TB6612FNG** (drives DC motor; mounted on the chassis but not rigidly fixed if not required by layout).  
- Power: 2 × 18650 Li-ion cells for Arduino power and 6 × AA batteries for motor supply; XL4015 module used for regulating motor supply where necessary.  
- Chassis, tires: custom chassis, drive shafts and a tire made from bicycle camera rubber for traction.

**Obelix 2.0 (final)**  
- Steering: **Solid Axle Steering** (simplified, mechanically more rigid). Ackerman replaced.  
- Motor replaced with **Force-Up 6V 625 RPM** (more torque at lower RPM).  
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
config.h <-- pin mapping, thresholds and simple calibration constants
/tools
serial_logger.py <-- optional serial log viewer (desktop)


**Main behaviour (firmware)**:
- Read sensors (HC-SR04 x4, TCS3200).  
- Low-level motor control through TB6612FNG (PWM and direction).  
- Steering control through SG90 (PWM).  
- State machine for: `WAIT_START -> LANE_FOLLOW -> PILLAR_HANDLING -> PARKING_MANEUVER -> STOP`.  
- Logging via Serial (for debug).

---

## 6. Pin mapping (recommended, edit `config.h` as needed)

- HC-SR04 front: TRIG D2, ECHO D3  
- HC-SR04 left: TRIG D4, ECHO D5  
- HC-SR04 right: TRIG D6, ECHO D7  
- HC-SR04 rear: TRIG D8, ECHO D9  
- TCS3200 S0..S3: D10..D13 (or analog read if using frequency counting)  
- Servo: D11 (PWM)  
- TB6612FNG: PWM_A D5, AIN1 D12, AIN2 D13 (adjust for actual pins)  
- Start push button: digital pin A0 (if used)  
> **Note:** Fill exact pins into `config.h` before upload.

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
- Lane follow stable (tested on practice mat).  
- Pillar (red/green) detection via TCS3200 (basic color classification using frequency ratio thresholds).  
- Parallel parking routine triggered after lap completion.

---

## 9. Reproducibility & files to reproduce the build

- `obelix_1.0/components/details.txt` — component list and small CAD notes.  
- `obelix_1.0/instructions_to_create.txt` — step-by-step assembly guide (mechanical connections, bolts and link lengths).  
- `obelix_2.0/new_changes/*` — changes from 1.0 to 2.0.  
- `/models` — STL / CAD files (3D models to be uploaded by team).  
- `docs/wiring_diagram.png` — wiring diagram (add when available).

---

## 10. Commit history requirements (WRO)

**IMPORTANT:** The repository must contain **at least 3 commits** with dates as required by WRO rules. For international submission ensure:

- Commit 1 — not later than 2 months before competition (contains ≥20% of final code).  
- Commit 2 — not later than 1 month before competition.  
- Commit 3 — not later than 2 weeks before competition.

**When submitting:** make the repository **public** and keep public for at least 12 months.

---

## 11. Licence & credits

All project files are provided under the MIT License (see `LICENSE`). Hardware components obtained from suppliers (listed in BOM). The team built custom mechanical parts and software modules; credits are in `CREDITS.md`.

---

## 12. Contact

Team Union — Team Captain: Jamil Hajizade — `<<team.email@example.com>>`


---

# 2) `engineering_journal.md` (скопируй в `obelix/docs/engineering_journal.md`) — **английский**

```markdown
# Engineering Journal — Team Union — Obelix

**Team:** Union  
**Robots:** Obelix 1.0 (prototype), Obelix 2.0 (final)  
**Authors:** Abdullah Beshirov, Murad Mehdi, Jamil Hajizade  
**Dates:** entries below describe major development milestones

---

## Entry 1 — Mobility Management (design & choices)
**Date:** 2025-09-25  
**Goal:** Design reliable drivetrain and steering that meet WRO rules (4 wheels, one driving axle, one steering actuator).  

**Components used:**
- Drive motor (Obelix 1.0): Force-Up 6V 1000 RPM (specs: stall/current/torque in components file)  
- Drive motor (Obelix 2.0): Force-Up 6V 625 RPM (reduced RPM, higher torque)  
- Differential assembly: LEGO differential part + cross shafts (custom mounts)  
- Steering: SG90 + Ackerman linkage (Obelix 1.0); Solid axle steering for Obelix 2.0.

**Design rationale:**
- We chose a single driving axle with differential to comply with WRO rules (no differential wheeled base). Using LEGO differential parts allowed quick prototyping of torque transfer and independent wheel rotation during turns.
- The SG90 servo was used for prototype Ackerman due to simplicity; later replaced with a mechanically simpler and sturdier Solid Axle in v2.0 to reduce play and increase repeatability in parking.
- Wheel traction: we used bicycle camera rubber on rims to increase friction on the mat.

**Testing:**
- Performed 10 straight runs at nominal motor voltage (6V) to confirm straight-line behaviour; noted slight right drift – corrected by adjusting mounting and trimming servo neutral in code.
- Observed overheating risk during prolonged stall tests — solution: recommended duty cycles and thermal breaks during runs.

**Attachments:** photos `../photos/mobility_front.jpg`, CAD files in `/models`, assembly instructions `obelix_1.0/instructions_to_create.txt`.

**Next steps:** finalize wheel hubs, add simple encoder for closed-loop speed control (future improvement).

---

## Entry 2 — Power & Sense Management
**Date:** 2025-09-25  
**Goal:** Document the power distribution, sensor choices and wiring.

**Power system:**
- Arduino power: 2 × 18650 Li-ion (regulated step-down where necessary) — separate from motor pack.
- Motor power: 6 × AA cells in series → power to motors via TB6612FNG and XL4015 for adjustable supply when necessary.
- Switches: main motor tumbler added in Obelix 2.0 for quick safe cutoff.

**Sensors and reason for selection:**
- HC-SR04 (x4) — cheap and reliable sonar for short-range obstacle detection and coarse localization of walls and pillars. Mounted: one front, left, right, rear. Readouts used for simple obstacle state machine (stop/avoid/back up).
- TCS3200 — simple colour sensor for distinguishing red vs green pillar. We implemented thresholding on frequency ratios to classify colours.
- No camera/SBC in current design: chosen to comply with limited hardware and to minimize complexity and power consumption.

**Wiring & BOM summary:** see `BOM.csv` and `docs/wiring_diagram.png`. All grounds common. TB6612FNG receives motor supply separate from Arduino 5V logic. XL4015 used as motor regulator when needed.

**Testing & measurements:**
- Idle current of motors at 6V measured ~30–40 mA. Stall current measured under load ~2.6–3.2 A (spec dependent) — ensure fuse/protection and wiring capable of peak currents.
- Ultrasonic sensors reliable for distances 5–200 cm; filtering and median smoothing applied in firmware to reduce spurious readings.

**Next steps:** create formal wiring diagram (draw.io) and BOM with suppliers/part numbers. Add labels on chassis for battery holders.

---

## Entry 3 — Obstacle Management & Algorithms
**Date:** 2025-09-25  
**Goal:** Describe the strategy and algorithm for obstacle run and parking.

**Strategy summary:**
- Finite state machine:
  - `START` → `FOLLOW_LANE` → while laps < 3: `PILLAR_RESPONSE` → if lap_complete: `PARKING_SEQUENCE` → `STOP`
- PILLAR_RESPONSE:
  - Use TCS3200 to detect pillar colour when ultrasonic indicates pillar is within ~20-40 cm.
  - If **GREEN** → keep left of pillar (execute left-bias trajectory).
  - If **RED** → keep right of pillar (execute right-bias trajectory).
- LANE FOLLOW:
  - Use ultrasonic side sensor pair (left+right) and front sensor for coarse centering in lane (distance balancing). Combined with dead-reckoning from wheel rotations when available.
- PARKING:
  - On entering parking zone, execute predefined parallel parking trajectory: approach, reverse with steering at angle, forward adjust, stop with projection inside parking box. Parking judged by projection on mat — we implemented a coordinate-free approach using relative distances and stops.

**Pseudo-code (high level):**
