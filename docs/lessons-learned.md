# Lessons Learned

This document summarizes the key engineering lessons learned during the DFT Pipe Inspection Robot project. The content is public-safe and avoids production source code, confidential hardware files, private pin maps, credentials, and client-sensitive implementation details.

---

## 1. Separate System Responsibilities Early

One of the most important lessons was the value of separating the system into clear functional blocks.

Instead of putting all control logic into one controller, the architecture used a Raspberry Pi master and multiple STM32 slave boards. Each embedded board handled a dedicated subsystem, which made the system easier to understand, test, and debug.

Key lesson:

> In a multi-board embedded system, clear subsystem boundaries reduce complexity and make debugging more manageable.

---

## 2. Use a Master-Controlled Communication Model

For a shared RS-485 bus, it is important to avoid uncontrolled responses from multiple devices.

A master-controlled communication model helped keep the bus predictable. The Raspberry Pi sends commands, and only the addressed STM32 board responds. This reduces the chance of bus conflicts and makes communication easier to validate.

Key lesson:

> In half-duplex multi-drop communication, predictable command/response behavior is more reliable than allowing multiple devices to transmit freely.

---

## 3. Validate Communication in Stages

Multi-board communication should not be tested all at once.

A staged validation approach is safer:

1. Test UART/RS-485 loopback.
2. Test Raspberry Pi to one STM32 board.
3. Test each board individually.
4. Test all boards together on the shared bus.
5. Validate complete command/response behavior under realistic conditions.

Key lesson:

> Debugging is faster when each layer is verified before full-system integration.

---

## 4. Avoid Blocking Firmware Loops

Blocking loops are risky in robotics and distributed embedded systems because they prevent the firmware from responding to communication, stop commands, status requests, or safety inputs.

Moving toward a non-blocking state-machine approach improved firmware responsiveness and made it easier to manage motion while still handling communication.

Key lesson:

> Long-running motor actions should be handled with state machines or non-blocking control logic, not blocking loops.

---

## 5. Timer-Based Motor Control Is More Reliable

Stepper motor control requires stable timing. Simple GPIO toggling can work for basic tests, but it becomes harder to maintain as the system grows.

Using timer-based pulse generation improves timing consistency and keeps the firmware structure cleaner.

Key lesson:

> Hardware timers are preferred for repeatable motor pulse generation in embedded motion-control systems.

---

## 6. Hardware Behavior Must Be Verified, Not Assumed

During integration, actual hardware behavior can differ from assumptions made during firmware development.

Motor driver enable polarity, signal voltage levels, wiring continuity, bus connections, and sensor behavior all need to be verified on real hardware.

Key lesson:

> Firmware decisions should be validated against real electrical behavior, not only datasheets or assumptions.

---

## 7. External Instruments Need Workflow-Aware Integration

The DFT gauge did not behave like a continuously streaming sensor. It provided useful readings only under valid measurement/contact conditions.

This required the software to handle measurement requests, pending states, valid readings, timeout behavior, and GUI coordination.

Key lesson:

> Instrument integration often requires understanding the user workflow, not only the serial protocol.

---

## 8. Camera Streaming Should Prioritize Practical Reliability

Multiple camera streaming can become complex if the pipeline depends on fragile dependencies or unsupported configurations on the operator machine.

A practical streaming approach was selected to keep the system easier to run, debug, and integrate with the GUI.

Key lesson:

> For field/operator systems, a simple and stable video pipeline is often better than a technically complex but fragile one.

---

## 9. Keep GUI and Hardware State Synchronized

In an operator-controlled robot, the GUI should not simply send commands. It also needs to reflect the actual robot state through events, status messages, readings, and error conditions.

The system required coordination between GUI commands, STM32 board responses, camera status, and DFT gauge readings.

Key lesson:

> A good operator GUI must be state-aware, not just command-driven.

---

## 10. Public Documentation Requires Careful Sanitization

This project involved real system architecture, firmware design, hardware integration, and client-sensitive work. Sharing everything publicly would not be appropriate.

The public case-study approach allows engineering experience to be shown without exposing production code or confidential implementation details.

Key lesson:

> A strong portfolio can demonstrate engineering capability through architecture, diagrams, lessons learned, and sanitized snippets without exposing private source code.

---

## 11. System Integration Is an Engineering Discipline

This project reinforced that embedded product development is not only about writing firmware.

It involves:

* Hardware understanding
* Firmware architecture
* Communication design
* Linux/Raspberry Pi integration
* GUI coordination
* Instrument interfacing
* Debugging
* Mechanical behavior awareness
* Validation planning

Key lesson:

> Real embedded systems require system-level thinking across firmware, hardware, software, and user workflow.

---

## 12. Document Decisions While the Project Is Active

Writing architecture notes, challenges, and lessons during the project makes it easier to explain the system later for portfolio, handoff, debugging, and future development.

Key lesson:

> Good engineering documentation is not only for others; it also improves your own understanding of the system.

