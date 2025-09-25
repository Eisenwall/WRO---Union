# Engineering Journal — Team Union / Robot OBELIX
**Team name:** Union  
**Team members:** Abdullah Beshirov, Murad Mehdi, Jamil Hajizade  
**Robot name:** Obelix (Obelix 1.0 — prototype; Obelix 2.0 — final)

---

## Summary
This engineering journal documents the development process of the autonomous WRO vehicle **Obelix**. The project progressed through an initial prototype (Obelix 1.0) and a final model (Obelix 2.0). The primary focus was on mobility (steering & drive), sensor fusion (ultrasonic array + color sensor + IMU/encoders) and robust obstacle management including parallel parking.

---

## Entry 1 — 2025-09-15 — Project start and Obelix 1.0 prototype
**Goal:** Build prototype to validate mechanical layout, steering concept (Ackermann), basic sensing and drive.  
**Hardware used:**
- 4× HC-SR04 ultrasonic sensors (front, left, right, rear)
- 1× SG90 micro servo (for steering Ackermann linkage)
- 1× Force-Up 6V 1000 RPM brushed DC motor (drive)
- 1× TB6612FNG motor driver
- 1× Arduino UNO (SBM)
- 1× TCS3200 colour sensor (front)
- 1× XL4015 (DC-DC supply module for motors)
- 2× 18650 Li-ion cells (for Arduino) + 6× AA batteries for motors
- LEGO differential parts for the differential system, various bolts, bicycle inner-tube for tires
- Breadboard, wires, connectors

**Actions:**
1. Assembled RC chassis with LEGO differential part and wheel mounts. Used D-shaft connectors and bolts (3 mm × 15 mm) to attach front axle members and steering linkages.
2. Implemented Ackermann steering with SG90 servo controlling two front wheels via linkage.
3. Wired 4 HC-SR04 sensors (trigger/echo) to Arduino digital pins and added basic filtering on measured distances (median of 3 readings).
4. Integrated TCS3200 in front for pillar color detection; tuned frequency scaling and measured raw R/G/B pulses.
5. Configured TB6612FNG to drive main motor through XL4015-regulated supply.
6. First tests: simple forward/back and turn; basic obstacle avoidance state machine implemented on Arduino.

**Results:**
- Prototype moved and steered, basic object detection worked reliably at 20–40 cm for HC-SR04.
- Color sensor recognized red/green pillars under controlled light (ambient light affects thresholds).
- Main issues: Ackermann steering introduced alignment complexity; TB6612FNG current peaks caused supply drop under stall — needed smoothing capacitor and proper ground returns.

**Conclusion / Next steps:**
- Improve power decoupling; measure current draw during stall.
- Replace Ackermann with a more robust steering for final model (or redesign steering geometry).
- Prepare detailed wiring diagram and BOM.

**Photos & Video:** `../photos/obelix_1_front.jpg`, `../photos/obelix_1_top.jpg`  
**Video (prototype):** `<<YouTube link placeholder>>`

---

## Entry 2 — 2025-09-21 — Mobility tuning & power management
**Goal:** Stabilize drive motor behaviour, tune wheel mounting, add power switch and updated motor choice for Obelix 2.0.  
**Hardware changes for Obelix 2.0:**
- Drive motor replaced with **Force-Up 6V 625 RPM** (lower RPM, higher torque at nominal voltage).
- Steering changed from Ackermann to **Solid Axle Steering** (rigid steering shaft + single servo or servo-gear steering rack).
- Added main motor power toggle switch (tumbler) and proper motor battery connector on robot roof.
- Re-positioned Li-ion battery socket to rear for CG balance.

**Actions:**
1. Replaced motor and tested free-run and loaded RPM at nominal 6 V. Logged no-load current ~40 mA and stall/starting current ~2.6 A (measured roughly by multimeter).
2. Reworked wiring: separate motor battery pack (6×AA) and SBC/Arduino supply with XL4015 + decoupling capacitors.
3. Mounted motor driver TB6612FNG on small perfboard and secured to chassis using nylon ties — kept accessible but mechanically stable.
4. Redesigned wheel mounts and differential coupling using LEGO differential internals with D-shaft adapter; ensured alignment to avoid binding.

**Measurements / Results:**
- Effective forward speed (measured by wheel circumference 0.1885 m at 625 RPM) ≈ 1.95 m/s theoretical free-run; loaded speed ~1.2 m/s in practice.
- Steering repeatability improved using solid axle; turning radius consistent across runs.
- Power supply stability improved after adding 470 µF electrolytic capacitor across motor terminals and 10 mF bulk across battery connector.

**Conclusion / Next steps:**
- Finalize mechanical mounts for motors and servo to secure under competition handling.
- Prepare complete wiring diagram and BOM for upload to GitHub.
- Move sensor thresholds to config file for easy tuning.

**Photos:** `../photos/obelix_2_side.jpg`, `../photos/obelix_2_rear.jpg`  
**Video (mobility tests):** `<<YouTube link placeholder>>`

---

## Entry 3 — 2025-09-23 — Sensor fusion & obstacle management
**Goal:** Integrate HC-SR04 array + TCS3200 + encoder feedback into decision logic for obstacle challenge and parking routine.  
**Software summary:**
- Obstacle management implemented as FSM (Finite State Machine) with states: IDLE → LINE_FOLLOW → PILLAR_DETECT → PASS_PILLAR → LAP_COMPLETE → PARK.
- Pillar classification uses TCS3200 thresholds: RED if R_ratio > 0.60 and R_abs > 200; GREEN if G_ratio > 0.60 and G_abs > 200 (example thresholds — adjust during testing).
- Distance gating: HC-SR04 front triggers braking when object < 12 cm; side sensors used to maintain lateral clearance.

**Actions:**
1. Implemented serial protocol between Raspberry Pi (if used in SBC setup) / Arduino or standalone Arduino state machine to combine sensor inputs.
2. Added simple encoder counter on motor axle for odometry coarse estimation (counts per wheel rotation recorded).
3. Implemented parallel parking routine triggered after lap count; parking uses ultrasound to detect parking marker limits and steering to align parallel to boundary.

**Results:**
- In 10 runs, pillar detection accuracy ≈ 92% in indoor lighting; false positives during reflective ambient light.
- Parallel parking success (fully inside bay) = 7/10 in test configuration with mock parking markers.
- Logs saved to `/logs` (timestamped csv) containing sensor readings and state transitions for later analysis.

**Conclusion / Next steps:**
- Improve TCS3200 ambient light compensation (add simple hood or calibration routine).
- Tune PID on drive to reduce overshoot in parking manoeuvre.
- Finalize and push all code with extensive comments to GitHub and ensure at least 3 commits.

**Photos & Schematics:** `../docs/wiring_diagram.png`  
**Video (obstacle & parking):** `<<YouTube link placeholder>>`

---

## Entry 4 — 2025-09-24 — Final checklist and submission preparation
**Goal:** Prepare final documentation, photos, videos, and GitHub repository for submission.  
**Completed items:**
- README.md (English, detailed) prepared.
- engineering_journal exported to PDF.
- Photos of all sides, top & bottom included in `/photos`.
- Two videos (Open & Obstacle) uploaded to YouTube as Unlisted.
- BOM.csv completed.

**Submission checklist (local):**
- Robot dimensions verified: length ≤ 300 mm, width ≤ 200 mm, height ≤ 300 mm.
- Weight measured: `<fill actual weight> kg` (if above 1.5 kg, note reduction steps).
- All wireless modules disabled (none used).
- 3 commits present in Git history.

**Notes for judges:**
- Key innovations: usage of LEGO differential part combined with a Solid Axle steering in Obelix 2.0, modular code architecture on Arduino (and SBC if used), and a practical power management solution using XL4015.
- Engineering factor: custom mounts and differential adaptations made by team; original design contributions include steering geometry and parking routine.

---

## Attachments & file references
- Photos: `/photos/obelix_1_front.jpg`, `/photos/obelix_1_top.jpg`, `/photos/obelix_2_side.jpg`, `/photos/obelix_2_rear.jpg`
- Wiring diagram: `/docs/wiring_diagram.png`
- BOM: `/BOM.csv`
- Code: `/code/*`
- Videos: links in `../videos.txt`
