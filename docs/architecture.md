# System Architecture

This document provides a public-safe overview of the DFT Pipe Inspection Robot architecture. It explains the major system blocks, communication flow, and engineering responsibilities without exposing production source code, private hardware files, confidential pin maps, or client-sensitive implementation details.

---

## 1. Architecture Overview

The DFT Pipe Inspection Robot follows a distributed embedded architecture.

A Raspberry Pi 4 acts as the master controller. It coordinates the operator GUI, camera streaming, DFT gauge integration, and communication with multiple STM32-based slave boards.

Each STM32 board is responsible for a specific robot subsystem, such as drive movement, inspection mechanism control, or actuator movement. The boards communicate with the Raspberry Pi through a shared RS-485 multi-drop bus.

At a high level, the system includes:

* Operator laptop with GUI
* Raspberry Pi 4 master controller
* Multiple STM32F401-based slave boards
* RS-485 multi-drop communication bus
* Stepper motor drivers
* Camera streaming subsystem
* DFT gauge integration
* Lighting and actuator control
* Hardware debugging and validation workflow

---

## 2. Simplified System Diagram

```text
Operator Laptop
    |
    | Ethernet
    |
Raspberry Pi 4 Master
    |
    | RS-485 Multi-Drop Bus
    |
    +-- STM32 Board 1: Drive Control
    |
    +-- STM32 Board 2: Legs + Inspection Drum Control
    |
    +-- STM32 Board 3: Brush + Probe Mechanism Control
    |
    +-- DFT Gauge Interface
    |
    +-- Camera Streaming
```

---

## 3. Master Controller

The Raspberry Pi 4 works as the central controller of the system.

Its responsibilities include:

* Receiving commands from the operator GUI
* Forwarding motion and actuator commands to STM32 slave boards
* Reading events and status messages from the embedded boards
* Coordinating DFT gauge readings
* Handling camera streaming
* Managing Ethernet-based communication with the operator laptop
* Acting as the bridge between the high-level GUI and low-level embedded controllers

The Raspberry Pi layer is implemented using Python-based services and communication scripts.

---

## 4. STM32 Slave Boards

The robot uses multiple STM32F401-based slave boards. Each board controls a dedicated subsystem to keep firmware responsibilities separated and easier to debug.

### Board 1 — Drive Control

This board handles robot locomotion. It controls stepper motors used for movement inside the pipe.

Main responsibilities:

* Drive motor control
* Forward/reverse motion
* Motion command execution
* Motor driver control
* Status reporting to the Raspberry Pi

### Board 2 — Legs and Inspection Drum

This board handles centering/positioning mechanisms and inspection drum movement.

Main responsibilities:

* Leg extension/retraction control
* Inspection drum rotation
* Encoder-based position feedback
* Motion status reporting
* Subsystem-level movement control

### Board 3 — Brush and Probe Mechanism

This board handles surface preparation and probe deployment mechanisms.

Main responsibilities:

* Brush deployment/retraction
* DFT probe deployment/retraction
* Limit switch handling
* Lighting/relay control
* Safety-related actuator stop behavior

---

## 5. RS-485 Communication Layer

The Raspberry Pi communicates with the STM32 boards using an RS-485 multi-drop bus.

RS-485 was selected because it is suitable for robust multi-board communication in electrically noisy embedded environments and supports differential signaling over longer wiring distances compared with single-ended UART.

The communication approach uses:

* One master device
* Multiple addressed slave boards
* Half-duplex communication
* Command/response style messaging
* Board-specific command filtering
* Status and completion messages from slave boards

This structure allows the Raspberry Pi to coordinate multiple embedded boards over a single shared communication bus.

---

## 6. Motion Control Architecture

The robot uses stepper motors for drive movement, inspection movement, and actuator positioning.

The firmware architecture supports:

* Stepper motor driver control
* Direction control
* Enable/disable handling
* Timer-based pulse generation
* Controlled motion execution
* Non-blocking state machine behavior
* Completion detection
* Safety stop behavior through limit inputs where required

A non-blocking motion approach is important because the firmware must continue handling communication and status events while movement is in progress.

---

## 7. Camera Streaming Subsystem

The robot uses multiple cameras for live inspection and operator feedback.

The camera subsystem allows the operator to monitor:

* Front view inside the pipe
* Probe/measurement area
* Brush/cleaning mechanism area

The Raspberry Pi handles camera capture and streaming. The operator GUI receives the camera views for real-time monitoring.

The public case study does not include private camera paths, exact stream configuration, or production deployment details.

---

## 8. DFT Gauge Integration

The system integrates a Dry Film Thickness gauge for coating thickness measurement.

The Raspberry Pi handles gauge communication and coordinates readings with the operator workflow.

At a high level, the integration supports:

* Serial communication with the gauge
* Reading capture when a valid measurement is available
* Forwarding measurement data to the operator GUI
* Associating readings with inspection workflow events

This part demonstrates external instrument integration inside a larger embedded robotics system.

---

## 9. Operator GUI

The operator GUI runs on a laptop and communicates with the Raspberry Pi over Ethernet.

The GUI provides:

* Robot movement control
* Actuator control
* Camera monitoring
* Status/event display
* DFT reading display
* Operator-assisted inspection workflow

The GUI is built using PyQt5 and is connected to the Raspberry Pi services through socket/network communication.

---

## 10. Firmware Architecture

The STM32 firmware follows a layered structure.

Common firmware responsibilities include:

* GPIO initialization
* UART/RS-485 communication
* Motor driver control
* State machine handling
* Timer-based pulse generation
* Encoder/limit switch handling where required
* Status reporting
* Error and safety behavior

The firmware is designed to keep board-specific behavior separate from reusable modules such as communication, motor control, and system utilities.

---

## 11. Engineering Focus Areas

This architecture demonstrates work across multiple embedded engineering areas:

* Distributed embedded control
* STM32 firmware development
* Raspberry Pi system integration
* RS-485 multi-board communication
* Stepper motor control
* Camera streaming
* External instrument integration
* PyQt5 GUI coordination
* Hardware bring-up
* Debugging and validation
* Industrial robotics system design

---

## 12. Public-Safe Scope

This document intentionally avoids sharing:

* Production firmware
* Private source code
* Confidential pin maps
* Client-owned hardware files
* Gerber files
* Credentials
* Exact deployment settings
* Internal IP addresses
* Sensitive protocol details
* Private test data

The goal of this repository is to demonstrate system architecture, engineering involvement, and embedded systems experience in a safe and professional way.

