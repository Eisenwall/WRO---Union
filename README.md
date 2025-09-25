# WRO---Union
# Team Union — Obelix (WRO Future Engineers 2025)

**Project name:** Obelix — Autonomous Robot  
**Team name:** Union  
**Team members:**  
- Abdullah Beshirov — Tester  
- Murad Mehdi — Programmer  
- Jamil Hajizade — Designer  

**GitHub account:** [Eisenwall](https://github.com/Eisenwall)  
**YouTube channel:** N/A  

---

## Project Overview

Our team, Union, has developed **Obelix**, an autonomous robot designed for the **WRO Future Engineers 2025** competition. The primary objective of Obelix is to navigate an obstacle-laden arena autonomously, detect and avoid objects using sensors, and maintain precise movement control with high reliability. Obelix has been developed in two versions: **Obelix 1.0** (prototype) and **Obelix 2.0** (final model). Each version demonstrates progressive improvements in mechanical design, sensor integration, and driving control systems.

We are a dedicated, goal-oriented, and hardworking team. Abdullah Beshirov focused on testing, Murad Mehdi handled programming, and Jamil Hajizade designed the mechanical and aesthetic parts of the robot. Together, we aimed to optimize the robot for maximum efficiency and accuracy on the competition field.

---

## Robot Versions

### Obelix 1.0 (Prototype)

Obelix 1.0 served as the prototype for testing sensor integration, motor control, and mechanical design. Its specifications include:

**Components:**
- **Motors:** 1 × Force-Up 6V 1000 RPM Carbon Brushed DC Motor  
  - Specially manufactured for professional mini sumo robots  
  - High torque and speed suitable for competitions  
  - 6V operating voltage (up to 12V for increased speed and torque)  
  - Specifications:
    - Operating Voltage: 6–12V  
    - Max Voltage: 12V  
    - Speed: 1000 RPM (6V)  
    - No-load Current: 30 mA (6V)  
    - Stall Current: 3.2 A (6V)  
    - Torque: 2.1 kg/cm nominal, 3.7 kg/cm stall  
    - Weight: 9.5 g  
- **Servo:** 1 × SG90 for steering front wheels via **Ackerman steering system**  
- **Ultrasonic Sensors:** 4 × HC-SR04 (1 front, 2 sides, 1 rear)  
- **Color Sensor:** TCS3200, positioned at the front of the robot  
- **Microcontroller:** Arduino Uno  
- **Voltage Regulator:** XL4015 to supply motors with correct voltage  
- **Power Supply:** 2 × Li-ion 18650 for Arduino, 6 × AA batteries for motors  
- **Motor Driver:** TB6612FNG (free-floating due to mechanical design)  
- **Differential system:** Custom LEGO-based differential with three inner gears transmitting motor torque to wheels  
- **Wheels and Steering:** Front wheels connected via Ackerman system using bolts and approximately 40 wires forming tire-like structures using bicycle tube material  

This version used differential drive for two wheels, controlled via the Arduino and color sensor for environmental feedback. The layout optimized sensor placement and power distribution to avoid interference with mechanical components.

---

### Obelix 2.0 (Final Model)

Obelix 2.0 incorporates mechanical and electronic improvements, including:

**Major Upgrades:**
- Replaced Ackerman steering with **Solid Axle Steering system** for increased stability  
- Added a **toggle switch** for motor power control  
- Updated motor: **Force-Up 6V 625 RPM Carbon Brushed DC Motor**  
  - Designed for mini sumo robots with high durability and reliability  
  - Specifications:
    - Operating Voltage: 6–12V  
    - Max Voltage: 12V  
    - Speed: 625 RPM (6V)  
    - No-load Current: 40 mA (6V)  
    - Stall Current: 2.6 A (6V)  
    - Torque: high enough for obstacle navigation  
    - Weight: 9.5 g  

All other components (ultrasonic sensors, color sensor, Arduino Uno, XL4015, battery setup) remained the same. The changes improved maneuverability, stability, and energy efficiency while maintaining the same overall robot footprint.

---

## Folder Structure

obelix/
│
├── obelix 1.0/
│ ├── README.md
│ ├── instructions_to_create.txt
│ ├── how_it_works
│ └── components/
│ └── details.txt (3D models uploaded separately)
│
├── obelix 2.0/
│ ├── new_changes.txt
│ ├── instructions_to_create.txt
│ ├── how_it_works
│ ├── how_it_looks
│ └── components/
│ └── details.txt (3D models uploaded separately)
│
└── README.md (this file)

pgsql
Копировать код

- `obelix 1.0/` contains all prototype documentation, assembly instructions, and 3D models.  
- `obelix 2.0/` includes the latest version documentation, updates compared to 1.0, assembly instructions, and component list.  
- `README.md` summarizes the entire project, robot specifications, and team details.

---

## How It Works

Obelix navigates using **ultrasonic sensors** to measure distances and a **TCS3200 color sensor** to detect markers or lines. Two motors are driven via **differential drive**, while the front wheels steer according to either the Ackerman (1.0) or Solid Axle (2.0) system. The Arduino reads sensor inputs, calculates steering adjustments, and controls motor speed and servo position in real time.

The robot intelligently adjusts its path based on distance changes from obstacles, ensuring efficient navigation and obstacle avoidance. The system handles complex turns and detects objects approaching from different angles.

---

## Sample Arduino Code

```cpp
#include <Servo.h>
Servo myServo;

#define PWMA A3
#define AIN1 A4
#define AIN2 A5

#define TRIG_F 2
#define ECHO_F 3

#define TRIG_R 4
#define ECHO_R 5

#define TRIG_L 6
#define ECHO_L 7

#define TRIG_B 8
#define ECHO_B 9

float prevR = 0, currR = 0, prevL = 0, currL = 0, prevB = 0, currB = 0, prevF = 0, currF = 0;
long duration;
int servoPos = 89;
int stepCount = 0;
int x;
bool xSet = false;

float getDistance(int trigPin, int echoPin) {
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);

  duration = pulseIn(echoPin, HIGH, 20000);
  if(duration == 0) return 9999;
  return (duration * 0.0343) / 2;
}

void setup() {
  pinMode(TRIG_R, OUTPUT);
  pinMode(ECHO_R, INPUT);
  pinMode(TRIG_L, OUTPUT);
  pinMode(ECHO_L, INPUT);
  pinMode(TRIG_B, OUTPUT);
  pinMode(ECHO_B, INPUT);
  pinMode(TRIG_F, OUTPUT);
  pinMode(ECHO_F, INPUT);

  myServo.attach(A2);
  myServo.write(servoPos);
  delay(300);

  pinMode(PWMA, OUTPUT);
  pinMode(AIN1, OUTPUT);
  pinMode(AIN2, OUTPUT);

  digitalWrite(AIN1, HIGH);
  digitalWrite(AIN2, LOW);
  analogWrite(PWMA, 255);

  Serial.begin(9600);
}

void loop() {
  if (stepCount >= 12) {
    analogWrite(PWMA, 0);
    myServo.write(89);
    while (true) {}
  }

  prevR = currR;
  currR = getDistance(TRIG_R, ECHO_R);
  prevL = currL;
  currL = getDistance(TRIG_L, ECHO_L);
  prevB = currB;
  currB = getDistance(TRIG_B, ECHO_B);
  prevF = currF;
  currF = getDistance(TRIG_F, ECHO_F);

  float diffR = currR - prevR;
  float diffL = currL - prevL;
  float diffB = currB - prevB;
  float diffF = currF - prevF;

  if(!xSet){
    if (currL < 18 && currF > currB) x = 1;
    else if (currL < 18 && currF < currB) x = 2;
    else if (currL > 70 && currF < currB) x = 3;
    else if (currL > 70 && currF > currB) x = 4;
    else if (currR < 35 && currF < currB) x = 5;
    else if (currR < 35 && currF > currB) x = 6;
    else if (currR > 55 && currF > currB) x = 7;
    else if (currR > 55 && currF < currB) x = 8;
    xSet = true;
  }

  if(currL > 125 && currB > 150){
    myServo.write(62);
    delay(670);
    servoPos = 89;
    myServo.write(servoPos);
    stepCount++;
  } else if(currR > 125 && currB > 140){
    myServo.write(114);
    delay(670);
    servoPos = 89;
    myServo.write(servoPos);
    stepCount++;
  }

  if (currL < 150 && currR < 150) {
    if(diffR > 0.5 || diffL < -0.5) servoPos = 98;
    else if(diffR < -0.5 || diffL > 0.5) servoPos = 80;
    else servoPos = 89;
    myServo.write(servoPos);
    delay(10);
  }
}
