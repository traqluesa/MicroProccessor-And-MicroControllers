Dual CPU Microprocessor Safety System

Fault Detection, Heartbeat Monitoring & Fail-Safe Architecture

📌 Project Overview

This project implements a dual-processor safety monitoring system designed for high-risk industrial environments. The primary objective is to ensure that a critical machine remains under control even if its main controller fails.

In such environments, machines often operate inside hazard zones that are inaccessible to human operators. A failure in the main control processor may go unnoticed, causing the system to continue operating in an unsafe state.

To address this problem, the system separates machine control and controller supervision across two independent processors.

⚠️ Problem Definition

In industrial systems such as nuclear cooling units, heavy press machines, or automated production lines, the main controller (CPU1) is responsible for all operational decisions.

If CPU1:

freezes,

crashes,

or stops executing instructions correctly,

operators outside the hazard zone may not be aware of the failure. This can result in loss of control, unsafe machine behavior, or critical system damage.

Therefore, monitoring only the machine output is insufficient. The controller itself must be continuously monitored by an independent unit.

✅ Proposed Solution

This project introduces a Dual-Processor (Master–Slave) Safety Protocol.

CPU1 (Master Controller) Handles machine logic, fault detection, heartbeat generation, and DAC signaling.

CPU2 (Slave / Watchdog Processor) Monitors CPU1 through a heartbeat mechanism and enforces a fail-safe state if CPU1 becomes unresponsive.

By distributing responsibilities, the system ensures that any failure in the main controller is detected in real time.

This design reflects real-world industrial watchdog architectures used in safety-critical systems.

🧩 System Architecture 🔹 CPU1 – Control & Fault Unit

Responsibilities:

Reads TESTER and FAULT signals

Detects rising edge of the FAULT signal

Increments and stores fault count

Sends:

Normal operational data

Fault counter value

Heartbeat signal (0x55)

Generates a visible DAC pulse

Displays fault count on 7-segment display

🔹 CPU2 – Monitor & Fail-Safe Unit

Responsibilities:

Receives data from CPU1

Displays:

Counter value on 7-segment

Operational data on LED bar

Monitors heartbeat timing

Triggers fail-safe mode if heartbeat is missing

🔄 Communication Protocol Signal Type Value Description Heartbeat 0x55 CPU1 is alive Counter Data 00H Fault counter Normal Data Bit-based LED control Timeout — Fail-safe activation ⚙️ I/O Port Usage CPU1 Port Function 00H 7-segment output (fault counter) 01H Output to CPU2 (data / heartbeat) 02H DAC output 01H (IN) TESTER & FAULT inputs CPU2 Port Function 00H Read counter from CPU1 01H Read protocol data 03H LED bar output ⏱ Heartbeat & Fail-Safe Mechanism

CPU1 periodically sends a heartbeat code (0x55)

CPU2 resets its internal timer upon receiving the heartbeat

If the heartbeat is not received within a defined time window:

LED outputs are disabled

System enters fail-safe mode

This mechanism ensures continuous supervision of the main controller.

🔌 DAC Pulse Generation

CPU1 generates a DAC pulse for visualization and timing verification:

DAC output set to 0x0F

Delay loop for visibility

DAC reset to 0x00

🧪 Demonstrated Concepts

Dual CPU communication

Rising-edge fault detection

Heartbeat protocol

Watchdog timer

Fail-safe system design

Hardware-level delay loops

DAC signal generation

LED bar & 7-segment control

🛠 Technologies Used

Assembly Language

Digital Circuit Simulation

Custom Educational CPU Architecture

Port-based I/O communication

📁 Project Structure


📦 Dual-CPU-Microprocessor
 ┣ 📜 UnnamedCircuit88.pbs   # Control, fault detection, heartbeat, DAC
 ┣ 📜 cpu1.mc8   # Control, fault detection, heartbeat, DAC
 ┣ 📜 cpu2.mc8   # Monitoring, watchdog, fail-safe logic
 ┣ 📜 README.md  # Project documentation
🎓 Course Information

Developed as part of a Microprocessors / Microcontroller Systems course.
