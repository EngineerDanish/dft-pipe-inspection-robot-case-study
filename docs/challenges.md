# Engineering Challenges

This document summarizes the main engineering challenges addressed during the DFT Pipe Inspection Robot project. The details are intentionally public-safe and do not include production code, private hardware files, confidential pin maps, credentials, or client-sensitive implementation data.

---

## 1. Coordinating Multiple Embedded Boards

One of the main challenges was coordinating multiple STM32-based boards from a single Raspberry Pi master controller.

Each board controlled a different robot subsystem, so the architecture needed to keep responsibilities separated while still allowing the full robot to behave as one system.

Key considerations included:

* Separating subsystem responsibilities clearly
* Preventing command conflicts between boards
* Keeping board responses identifiable
* Maintaining predictable command execution
* Simplifying debugging across multiple controllers

The solution used a distributed architecture where the Raspberry Pi acted as the master controller and each STM32 board operated as an addressed slave device.

---

## 2. Reliable RS-485 Multi-Drop Communication

The robot required a shared communication bus between the Raspberry Pi and multiple STM32 boards.

RS-485 was selected for its robustness and suitability for multi-drop embedded systems. The challenge was to keep communication reliable while multiple boards shared the same bus.

Key challenges included:

* Half-duplex bus direction handling
* Address-based command routing
* Avoiding simultaneous responses
* Maintaining clean command/response behavior
* Validating communication across all boards
* Debugging wiring and signal-level issues

The communication flow was designed around a master-controlled polling and command model, where the Raspberry Pi sends commands and each addressed board responds only when required.

---

## 3. Non-Blocking Motion Control

The robot performs mechanical movements using stepper motors. Early motion-control approaches can easily become blocking if firmware waits inside long movement loops.

Blocking motion control creates problems because the board cannot respond properly to communication, safety events, or status requests during movement.

The firmware architecture was improved using non-blocking state machine behavior.

This allowed the firmware to:

* Start a motion command
* Continue checking communication
* Monitor movement progress
* Detect completion
* Report status back to the Raspberry Pi
* Handle safety or stop events where required

This approach improved responsiveness and made the firmware more suitable for a distributed robotic system.

---

## 4. Stepper Motor Timing and Driver Behavior

The project required controlled stepper motor movement for drive, drum, leg, brush, and probe mechanisms.

Challenges included:

* Generating stable step pulses
* Controlling motor direction and enable behavior
* Handling multiple motors across different boards
* Matching firmware timing with mechanical behavior
* Testing driver polarity and signal behavior
* Ensuring motion remained predictable during inspection workflow

Timer-based pulse generation was used to improve timing consistency compared with simple GPIO toggling.

---

## 5. Integrating Mechanical Safety Inputs

Some robot mechanisms required limit switches or position feedback to prevent over-travel and protect mechanical parts.

Challenges included:

* Detecting end-stop events reliably
* Stopping motion immediately when a limit condition occurred
* Keeping limit-switch behavior separate from normal movement logic
* Ensuring safe behavior during actuator deployment and retraction
* Testing hardware-level switch behavior with firmware logic

Interrupt-based input handling was used where fast response was required.

---

## 6. External DFT Gauge Integration

The robot integrates an external Dry Film Thickness gauge. Unlike a simple continuously streaming sensor, the gauge behavior depends on contact and measurement conditions.

Challenges included:

* Detecting valid readings only when available
* Handling serial communication with the gauge
* Avoiding stale or repeated readings
* Coordinating the measurement workflow with the GUI
* Reporting timeout or no-reading conditions cleanly
* Integrating the gauge into the larger robot control flow

A request-and-wait style workflow was used, where the operator action triggers a pending measurement request and the Raspberry Pi forwards the next valid reading to the GUI.

---

## 7. Multi-Camera Streaming

The robot uses multiple cameras for operator visibility.

Challenges included:

* Running multiple camera streams from the Raspberry Pi
* Maintaining stable camera mapping
* Handling camera disconnection or frozen frames
* Displaying live views in the GUI
* Keeping the video approach compatible with the operator laptop environment
* Balancing reliability and implementation complexity

A simple MJPEG-over-HTTP approach was selected for public-safe architectural simplicity and practical compatibility with the GUI.

---

## 8. Raspberry Pi and GUI Coordination

The Raspberry Pi had to coordinate between low-level embedded boards, camera streams, the DFT gauge, and a laptop-based operator GUI.

Challenges included:

* Managing multiple communication paths
* Keeping GUI actions synchronized with robot state
* Handling asynchronous events from embedded boards
* Forwarding readings and status updates
* Avoiding blocking behavior in the master software
* Keeping the system usable for an operator

The architecture separates GUI communication, board communication, camera streaming, and instrument reading into distinct software responsibilities.

---

## 9. Hardware Bring-Up and Validation

The project involved multiple hardware subsystems, including microcontroller boards, motor drivers, RS-485 transceivers, cameras, gauge interface, and actuator mechanisms.

Challenges included:

* Verifying board-level wiring
* Testing communication one board at a time
* Validating motor driver behavior
* Checking sensor/encoder/limit input response
* Debugging electrical and firmware issues together
* Moving from individual subsystem testing to integrated system testing

A staged validation approach was used: first testing subsystems individually, then validating board-level communication, and finally moving toward multi-board integration.

---

## 10. Public-Safe Documentation

Another challenge was documenting the project in a way that demonstrates engineering depth without exposing confidential implementation details.

This repository intentionally avoids:

* Production firmware
* Full pin maps
* Private protocol implementation details
* Client-owned source code
* Hardware design files
* Internal test data
* Credentials or network settings

Instead, it focuses on public-safe architecture, engineering challenges, and reusable lessons learned.

