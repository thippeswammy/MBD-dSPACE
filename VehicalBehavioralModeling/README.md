# Model-Based Design (MBD) with dSPACE: Vehicle Behavioral Modeling (SIL & HIL) 🚗

## Demo
![ControlDesk Dashboard](https://github.com/user-attachments/assets/3a174b9b-5b71-4718-aaca-5e0c92c59162)
![Simulink Behavior Model](https://github.com/user-attachments/assets/397e3260-6c69-41c0-bb7a-a3acf7a20c2e)
![Harware Arduino -dspace scalexio interface model](https://github.com/user-attachments/assets/d1ae4ad6-a251-4081-93c9-97403e7fad36)

## 📖 Overview

This repository presents a complete Model-Based Design (MBD) workflow, taking a vehicle's behavioral logic from an idealized software simulation down to real-time physical hardware constraints. 

The project utilizes **MATLAB/Simulink** to build the core logic (ignition, gearbox, acceleration, braking, steering, indicators), and transitions it through **Software-in-the-Loop (SIL)** and **Hardware-in-the-Loop (HIL)** testing utilizing the **dSPACE SCALEXIO** system. An Arduino Mega acts as the physical prototype Electronic Control Unit (ECU) to demonstrate the hardware-software handshake by driving physical actuators (servos, LEDs).

---

## 💻 System Requirements

To fully explore, simulate, or reproduce this project, you will need:

**Software:**
- MATLAB / Simulink
- dSPACE ConfigurationDesk (for `.htfx` I/O mapping)
- dSPACE ControlDesk (for `.CDE` real-time GUI and manual state validation)

**Hardware Setup (For HIL Phase):**
- dSPACE SCALEXIO HIL System
- Arduino Mega 2560 (Acting as prototype ECU)
- Standard Servo Motor (for Steering representation)
- LEDs and physical switches (for indicators, ignition, and status warnings)

---

## 🛠️ Project Architecture & Testing Phases

The project is structured to follow industry-standard MBD validation steps:

### 1. Action-Based Behavioral Modeling (MATLAB/Simulink)
Developed a functional plant model (`Veh_Behavior_Modeling_Exp1.slx`) representing the vehicle's core actions rather than purely physical kinematics. This subsystem-heavy model processes driver inputs and scales them for hardware-level execution.

### 2. Software-in-the-Loop (SIL) Verification
Before deploying to hardware, the model's logic was rigorously verified:
* **Internal Signal Generation:** Test inputs were generated entirely within the software environment.
* **Validation via ControlDesk:** Using loopback signals, combinations of simulated driver inputs were manually injected and monitored via a custom dSPACE **ControlDesk** dashboard to ensure the behavioral model met all fundamental system requirements.

### 3. Hardware-in-the-Loop (HIL) Deployment
The transition from simulation to real-time physical testing:
* **dSPACE Integration:** The validated Simulink model was mapped and compiled using **ConfigurationDesk** and loaded onto the dSPACE SCALEXIO hardware for real-time execution.
* **Arduino ECU Prototype (`ArduionModelInterWithdSPACE.slx`):** An Arduino Mega was interfaced with the dSPACE unit to read physical sensor inputs and command physical actuators, validating the final hardware-software integration.

---

## ⚙️ Vehicle Model Specifications

The core logic processes raw analog/digital inputs and translates them into constrained output behaviors.

| Feature | Input Range | Output Behavior | Functional Description |
|---------|-------------|-----------------|------------------------|
| **Ignition ON/OFF** | `0` (OFF) / `1` (ON) | `0V` or `5V` (Digital) | Toggles vehicle power state. Drives a digital output for voltage control. |
| **Gearbox** | `0-341` (R)<br>`341-682` (N)<br>`682-1023` (F) | `-1` (Reverse)<br>`0` (Neutral)<br>`1` (Forward) | Translates analog potentiometer ranges into discrete gear selections. |
| **Acceleration** | `0-1023` | `0-100` (Speed Value) | Scales input to represent engine RPM and vehicle speed limits. |
| **Car Brake** | `0-1023` | `0` (Overrides throttle) | Engages braking logic; forces speed to zero if the input value drops below 200. |
| **Hand Brake** | `0` (OFF) / `1` (ON) | Warning Light + Speed Drop | Activates dashboard warning LED and gradually overrides acceleration to reduce speed. |
| **Steering Angle** | `0-1025` | `-27.8° to 27.8°` | Converts analog input into specific physical angle bounds for the steering servo motor. |
| **Indicator** | `0-341` (L)<br>`341-682` (Null)<br>`682-1023` (R) | Left Blinker / Right Blinker | Activates corresponding indicator logic and LED blink rates. |

---

## 🔌 Arduino Mega Hardware Pinout

For the HIL phase, the physical interface relies on bidirectional communication between dSPACE SCALEXIO output pins and the Arduino Mega.

**Digital Outputs (Actuators & Status):**
* **Pins 8 - 13:** LED Control Signals (Left/Right Indicators, Handbrake Warning, Power Status)
* **Pin 9 (Servo):** Standard Servo Write for physical Steering Angle representation
* **Pin 3 (PWM):** Motor speed control (Throttle representation)

**Inputs (Sensors & Switches):**
* **Analog A0 / A1:** Sensor readings (Throttle/Brake mapping)
* **Digital 2 - 7:** Button and switch states (Ignition, Handbrake)

---

## 🗂️ Project Directory Structure

```text
📦 MBD-dSPACE
 ┣ 📂 BasicSignalGeneration/     # Fundamentals: Analog/Digital Signal Gen
 ┣ 📂 VehicalBehavioralModeling/ # Core Vehicle Logic & HIL Integration
 ┃ ┣ 📂 ConfigurationDesk/       # Hardware Topology maps (.htfx)
 ┃ ┣ 📂 ControlDesk/             # HIL/SIL Manual Validation Dashboards (.CDE)
 ┃ ┗ 📂 Matlab/                  
 ┃    ┣ 📜 Veh_Behavior_Modeling_Exp1.slx      # Core behavioral plant model
 ┃    ┗ 📜 ArduionModelInterWithdSPACE.slx     # Arduino hardware interface model
 ┗ 📂 vehicleController/         # Additional experimental control logic

 ## 👤 Author

**Thippeswamy K S**  
Pre-Final Year, Electronics and Communication Engineering  
GITAM Deemed to be University, Bengaluru  

---

I would like to express my sincere gratitude to:

- **Dr. Prithvi Sekhar Pagala** for continuous guidance and technical support  
- **dSPACE Team** for providing the tools and learning environment  
- **Rahul and Harisankar** for their valuable assistance during the project  
- **GITAM Deemed to be University** for enabling this hands-on experience  

---

## 📜 License

This project is open-source and available under the MIT License.
