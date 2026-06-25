# My Role

This document explains my role in the DFT Pipe Inspection Robot project in a public-safe way. It focuses on engineering responsibilities, technical contribution areas, and system-level involvement without exposing production source code, confidential hardware files, private pin maps, credentials, or client-sensitive implementation details.

---

## Role Overview

My role in this project is focused on embedded systems development, firmware architecture, multi-board communication, Raspberry Pi integration, system debugging, and technical coordination across hardware and software layers.

The project involves a distributed embedded robotics system where a Raspberry Pi 4 acts as the master controller and multiple STM32-based boards control different robot subsystems. My work connects the low-level embedded firmware with the higher-level Raspberry Pi software, camera streaming, DFT gauge integration, and operator GUI workflow.

---

## Main Contribution Areas

My main contribution areas include:

* STM32 firmware development
* RS-485 communication implementation
* Multi-board embedded system coordination
* Stepper motor control
* Non-blocking motion-control logic
* Raspberry Pi integration
* Camera streaming coordination
* DFT gauge serial integration
* PyQt5 GUI coordination
* PCB design support
* Hardware bring-up and debugging
* System-level testing and validation

---

## Embedded Firmware Development

I worked on firmware development for STM32-based controller boards used in different robot subsystems.

Firmware responsibilities included:

* GPIO configuration
* UART communication
* RS-485 direction control
* Stepper motor driver control
* Timer-based motor pulse handling
* Encoder or input handling where required
* Limit/safety input handling where required
* Status reporting to the Raspberry Pi
* Board-level command execution
* Debugging firmware behavior on hardware

The firmware was structured to separate reusable modules from board-specific behavior so that each controller board could focus on its own subsystem.

---

## RS-485 Multi-Board Communication

A major part of my work involved communication between the Raspberry Pi master and multiple STM32 slave boards over a shared RS-485 bus.

My responsibilities included:

* Implementing address-based communication behavior
* Supporting command/response communication flow
* Managing half-duplex communication behavior
* Validating board responses
* Debugging bus-level issues
* Testing communication with individual boards and then with the full multi-board setup

This helped create a cleaner and more reliable way for the Raspberry Pi to coordinate different robot subsystems over one shared communication link.

---

## Motion Control and State Machines

The robot uses stepper motors for multiple mechanical actions, including drive movement, centering mechanisms, inspection movement, and actuator deployment.

My work included:

* Implementing stepper motor control logic
* Using timer-based pulse generation
* Managing motor enable and direction control
* Supporting controlled movement commands
* Moving toward non-blocking state machine behavior
* Handling motion completion events
* Supporting safety-related stop behavior where required

The non-blocking state-machine approach was important because the firmware needed to continue handling communication and status updates while movement was active.

---

## Raspberry Pi Integration

The Raspberry Pi acts as the central controller of the robot.

My work involved supporting integration between the Raspberry Pi and the embedded controller boards, including:

* Communication with STM32 boards
* Serial communication handling
* Command forwarding
* Event/status processing
* DFT gauge integration flow
* Camera streaming coordination
* Connection between robot hardware and operator GUI

This layer helped bridge low-level embedded control with higher-level operator interaction.

---

## DFT Gauge Integration

The robot integrates a Dry Film Thickness gauge for coating measurement.

My contribution involved supporting the software flow required to read and forward gauge measurements as part of the inspection workflow.

This included:

* Understanding the gauge communication behavior
* Handling serial reading logic
* Coordinating measurement requests with the GUI workflow
* Forwarding valid measurement data to the operator side
* Managing timeout/no-reading behavior at a system level

This part of the project required treating the gauge as an external instrument rather than a simple continuously streaming sensor.

---

## Camera Streaming and GUI Coordination

The robot uses multiple cameras for live operator visibility.

My work supported the integration between:

* Raspberry Pi camera streaming
* Operator laptop GUI
* Robot control commands
* Status and reading updates

The GUI side was coordinated using PyQt5, while the Raspberry Pi handled camera streams and communication with embedded boards and instruments.

My contribution focused on ensuring that camera views, robot state, control commands, and measurement events could work together as part of one operator workflow.

---

## PCB Design and Hardware Support

The project involved custom board-level work for the embedded controller hardware.

My role included:

* Supporting PCB design decisions
* Reviewing hardware behavior during firmware testing
* Assisting with board bring-up
* Debugging motor driver behavior
* Testing communication interfaces
* Validating sensor/input behavior
* Supporting integration between firmware and hardware

This work required practical debugging across both firmware and electronics hardware.

---

## Testing and Debugging

Testing and debugging were important parts of my role because the system included several interacting layers.

My debugging work included:

* Board-level firmware testing
* RS-485 communication validation
* Motor driver behavior testing
* Raspberry Pi to STM32 communication testing
* Camera stream stability checks
* DFT gauge reading flow verification
* GUI and robot state coordination
* Full-system integration support

The system was tested in stages to isolate issues before moving toward complete integration.

---

## Engineering Value Demonstrated

This project demonstrates my experience in:

* Distributed embedded system design
* STM32 firmware development
* RS-485 multi-board communication
* Stepper motor control
* Raspberry Pi system integration
* Embedded Linux-based workflows
* External instrument integration
* Camera streaming coordination
* Operator GUI workflow integration
* Hardware debugging and validation
* System-level embedded robotics development

---

## Public-Safe Note

This document describes my engineering role at a high level. It intentionally excludes production source code, confidential implementation details, private pin maps, client-owned hardware files, credentials, and internal deployment information.

