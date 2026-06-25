# DFT Pipe Inspection Robot — Case Study

A public-safe case study of an ongoing multi-board embedded robotics system for internal pipe coating inspection.

The robot is designed to move inside industrial pipes and support Dry Film Thickness measurement, surface preparation, live visual inspection, lighting control, and operator-assisted movement.

> This repository contains documentation, architecture notes, diagrams, and simplified technical explanations only. It does not include production firmware, private hardware files, client-owned source code, credentials, or confidential implementation details.

---

## Project Overview

The DFT Pipe Inspection Robot is an embedded robotics system developed for internal pipe coating inspection. DFT stands for Dry Film Thickness, which refers to the coating thickness measurement performed inside the pipe.

The system combines microcontroller firmware, Raspberry Pi integration, RS-485 multi-board communication, stepper motor control, camera streaming, external instrument integration, and a laptop-based operator GUI.

At a high level, the robot includes:

* Drive control for movement inside the pipe
* Adjustable centering legs for pipe diameter adaptation
* Inspection drum movement
* Brush and probe deployment mechanisms
* DFT gauge integration
* Live camera streaming
* Onboard lighting control
* Operator GUI for monitoring and control

---

## System Architecture

The system follows a distributed embedded architecture:

* Raspberry Pi 4 works as the master controller.
* Multiple STM32F401-based boards work as slave controllers.
* Boards communicate with the Raspberry Pi over a shared RS-485 multi-drop bus.
* Stepper motors are controlled through external stepper motor drivers.
* Cameras stream video from the robot to the operator interface.
* A DFT gauge is integrated for coating thickness readings.
* A PyQt5-based GUI runs on the operator laptop.

Simplified architecture:

```text
Operator Laptop
    |
    | Ethernet
    |
Raspberry Pi 4
    |
    | RS-485 Multi-Drop Bus
    |
    +-- STM32 Board 1: Drive Control
    +-- STM32 Board 2: Legs + Inspection Drum
    +-- STM32 Board 3: Brush + Probe Mechanism
    |
    +-- DFT Gauge Integration
    +-- Camera Streaming
```

---

## My Role

My work on this project involves:

* STM32 firmware development
* RS-485 communication implementation
* Multi-board embedded system coordination
* Stepper motor control
* Non-blocking firmware state machine design
* Raspberry Pi integration
* Camera streaming support
* DFT gauge serial integration
* PyQt5 GUI coordination
* PCB design support
* Hardware testing and debugging
* System-level validation

---
## Documentation

This repository includes public-safe documentation for the project:

* [System Architecture](docs/architecture.md)
* [My Role](docs/my-role.md)
* [Engineering Challenges](docs/challenges.md)
* [Lessons Learned](docs/lessons-learned.md)
* [Disclaimer](DISCLAIMER.md)

## Technical Highlights

### Multi-Board Embedded Control

The robot uses multiple STM32F401-based boards, each responsible for a specific subsystem. This helps separate drive control, inspection movement, and probe/brush mechanisms into dedicated embedded controllers.

### RS-485 Communication

The Raspberry Pi communicates with the STM32 boards through a shared RS-485 bus. A lightweight addressed communication approach is used so that each board can receive commands and respond independently.

### Stepper Motor Control

Stepper motors are used for robot movement, inspection drum rotation, leg movement, and actuator deployment. Firmware was designed around controlled motion, hardware timing, and safe subsystem behavior.

### Raspberry Pi Master Integration

The Raspberry Pi coordinates communication between the operator GUI, STM32 slave boards, camera streams, and DFT gauge interface.

### Camera Streaming

Multiple cameras are used for live visual inspection. The system streams camera views to the operator interface for monitoring pipe interior, probe movement, and brush position.

### DFT Gauge Integration

The robot integrates a coating thickness gauge to acquire DFT measurements during inspection. The integration required serial communication handling and coordination with the operator workflow.

### Operator GUI

A PyQt5-based GUI is used on the operator laptop to control the robot, monitor camera feeds, and receive inspection events/readings.

---

## Technologies Used

* STM32F401
* Embedded C
* STM32CubeMX / HAL
* Raspberry Pi 4
* Python
* PyQt5
* RS-485
* UART
* Stepper motor control
* TB6560 stepper drivers
* Camera streaming
* MJPEG over HTTP
* DFT gauge integration
* KiCad
* Timer PWM
* EXTI
* State machines
* Hardware debugging

---

## Engineering Areas Demonstrated

This project demonstrates experience in:

* Embedded firmware development
* Distributed embedded architecture
* Robotics control
* Industrial inspection systems
* Multi-board communication
* Embedded Linux / Raspberry Pi integration
* Sensor and instrument integration
* Operator GUI coordination
* Hardware bring-up and debugging
* System-level embedded product development

---

## Current Status

This is an ongoing project. Public updates in this repository will include only safe documentation, diagrams, simplified explanations, and non-confidential snippets.

Production firmware, hardware design files, client-owned code, and confidential implementation details are intentionally excluded.

---

## Disclaimer

This repository is a public-safe portfolio case study. It is intended to demonstrate engineering involvement, architecture understanding, and embedded systems experience without exposing confidential project assets.
