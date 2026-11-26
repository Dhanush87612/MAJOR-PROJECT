CAN Based Intelligent Bus Accident Prevention and Automatic Braking System using LPC2129

A CAN-based automotive safety system that detects obstacles, monitors vehicle speed, and automatically controls braking to prevent bus accidents. The project uses three LPC2129 ARM7 microcontrollers, MCP2551 CAN transceivers, an ultrasonic sensor, a potentiometer, and a DC motor.

Table of Contents

1. Overview
2. Objectives
3. Hardware Components
4. Software and Tools
5. System Block Diagram
6. Software Architecture
7. Working Principle
8. CAN Communication Messages
9. Peripheral Interfacing
10. Learning Outcomes
11. Important Implementation Notes
12. Future Enhancements

1. Overview

This project implements an intelligent bus accident prevention and automatic braking system using CAN communication.

The system consists of three Electronic Control Units (ECUs), each using an LPC2129 ARM7 microcontroller.

- ECU1 detects obstacles using an HC-SR04 ultrasonic sensor.
- ECU3 measures vehicle speed using a potentiometer through ADC.
- ECU2 receives distance and speed information through CAN communication and controls the DC motor using PWM.

The system automatically reduces motor speed or stops the motor depending on the distance to the obstacle.

A 16×2 LCD displays distance, speed, and braking status. An LED and buzzer provide warning indications.

2. Objectives

- Implement CAN-based communication using LPC2129.
- Detect obstacles using an ultrasonic sensor.
- Measure vehicle speed using a potentiometer.
- Transmit sensor information between three ECUs.
- Implement automatic braking based on obstacle distance.
- Control DC motor speed using PWM.
- Display distance, speed, and braking status on an LCD.
- Provide warning indications using LED and buzzer.

3. Hardware Components

- LPC2129 ARM7 microcontroller – 3 units.
- MCP2551 CAN transceiver – 3 units.
- HC-SR04 ultrasonic sensor.
- 10K potentiometer.
- L293D motor driver.
- DC motor.
- 16×2 LCD.
- Buzzer.
- LED.
- 120Ω termination resistors – 2 units.
- Regulated 5V and 12V power supplies.
- Connecting wires and supporting components.

4. Software and Tools

- Embedded C
- Keil µVision IDE
- LPC2129 device header (lpc21xx.h)
- Flash Magic
- Proteus 8.13 (for simulation, if used)
- ARM7 microcontroller programming

5. System Block Diagram

The system consists of three ECUs connected through a common CAN bus.

          ECU1
       LPC2129
           |
     HC-SR04 Sensor
           |
        MCP2551
           |
           |
========== CAN BUS ==========
           |
           |
        MCP2551
           |
       LPC2129
          ECU2
           |
    -----------------
    |       |       |
  L293D   LCD    Buzzer
    |
  DC Motor
           |
========== CAN BUS ==========
           |
           |
        MCP2551
           |
       LPC2129
          ECU3
           |
      Potentiometer
           |
          ADC

6. Software Architecture

The firmware is divided into three independent ECU programs.

1. ECU1 – Obstacle Detection

Responsible for:

- Initializing GPIO and Timer0.
- Generating a 10µs trigger pulse.
- Reading the ultrasonic echo signal.
- Calculating obstacle distance.
- Transmitting distance through CAN ID 0x100.

2. ECU3 – Speed Measurement

Responsible for:

- Initializing ADC.
- Reading the potentiometer value.
- Converting ADC readings into speed percentage.
- Transmitting speed through CAN ID 0x200.

3. ECU2 – Engine Control and Automatic Braking

Responsible for:

- Initializing CAN communication.
- Receiving distance and speed information.
- Comparing distance with braking conditions.
- Controlling DC motor speed using PWM.
- Activating LED and buzzer.
- Displaying system information on LCD.
- Transmitting brake status through CAN ID 0x300.

7. Working Principle

1. ECU1 initializes the ultrasonic sensor and CAN communication.
2. The HC-SR04 sensor detects the obstacle in front of the bus.
3. ECU1 calculates the distance using the echo pulse duration.
4. ECU1 transmits distance information to ECU2 through CAN ID 0x100.
5. ECU3 reads the potentiometer using the ADC peripheral.
6. The ADC value is converted into a speed percentage from 0 to 100.
7. ECU3 transmits speed information through CAN ID 0x200.
8. ECU2 receives both distance and speed information.
9. ECU2 compares the received values with the braking conditions.
10. The motor speed is reduced or stopped automatically.
11. The LCD displays distance, speed, and braking status.
12. The LED and buzzer provide warning indications.
13. ECU2 transmits brake status through CAN ID 0x300.

Automatic Braking Conditions

Distance| Action| Indication
Above 100 cm| Normal motor speed| No warning
60–100 cm| Reduce motor speed| LED ON
30–60 cm| Reduce motor speed sharply| Buzzer
Below 30 cm| Stop motor| LED and continuous buzzer

At high speed, the braking thresholds are adjusted to start braking earlier.

8. CAN Communication Messages

The project uses standard CAN 2.0A communication with 11-bit identifiers.

CAN Message IDs

CAN ID| Transmitter| Receiver| Data
0x100| ECU1| ECU2| Obstacle distance
0x200| ECU3| ECU2| Vehicle speed
0x300| ECU2| All ECUs| Brake status

CAN Bus Configuration

- CAN Protocol: CAN 2.0A
- CAN Identifier: 11-bit standard ID
- CAN Bit Rate: 500 kbps
- CAN Transceiver: MCP2551
- CAN Bus: Two-wire communication (CAN_H and CAN_L)
- Termination: 120Ω at both physical ends.

Data Format

- CAN ID 0x100: Distance in centimetres (2 bytes).
- CAN ID 0x200: Speed percentage (1 byte).
- CAN ID 0x300: Brake status (1 byte).

Brake status:

- 0 – Normal
- 1 – Warning
- 2 – Brake

9. Peripheral Interfacing

CAN Communication

LPC2129 Signal| Pin
CAN TXD1| P0.24
CAN RXD1| P0.25

The CAN controller communicates with the MCP2551 transceiver.

Ultrasonic Sensor

Sensor Signal| LPC2129 Pin
Trigger| P0.4
Echo| P0.5

The ultrasonic sensor measures obstacle distance.

Potentiometer

Signal| LPC2129 Pin
Analog Output| P0.28 (AD0.1)

The ADC converts the analog voltage into a digital value.

Motor Control

Signal| LPC2129 Pin
PWM1| P0.0
Motor Direction 1| P0.6
Motor Direction 2| P0.7

The L293D motor driver controls the DC motor.

LCD, LED and Buzzer

- LCD: P1 GPIO interface (4-bit mode).
- LED: P0.11.
- Buzzer: P0.10.

The LCD displays the system status, while the LED and buzzer indicate warning and braking conditions.

10. Learning Outcomes

- LPC2129 ARM7 microcontroller programming.
- CAN 2.0A communication protocol.
- MCP2551 CAN transceiver interfacing.
- Ultrasonic sensor interfacing.
- ADC programming and analog-to-digital conversion.
- PWM-based motor speed control.
- LCD interfacing.
- GPIO programming.
- Timer0-based delay generation.
- Automotive embedded system development.
- Multi-ECU communication architecture.

11. Important Implementation Notes

- Three separate LPC2129 microcontrollers are required.
- Each ECU requires an independent Embedded C program.
- Three separate Keil projects and HEX files are required.
- MCP2551 is used as the CAN transceiver.
- CAN communication operates at 500 kbps according to the design.
- Two 120Ω resistors are required at the physical ends of the CAN bus.
- ECU1 and ECU3 transmit data every 100ms.
- ECU2 receives the sensor information and performs automatic braking.
- The potentiometer simulates the vehicle speed sensor.
- The DC motor represents the bus engine.
- The project is a laboratory prototype, not a certified vehicle braking system.

12. Future Enhancements

- Replace the ultrasonic sensor with radar or LiDAR.
- Integrate a real wheel-speed sensor.
- Implement a real electronic or pneumatic braking actuator.
- Add GPS and GSM modules for accident alerts.
- Implement driver drowsiness detection.
- Add additional CAN nodes for rear obstacle detection.
- Improve fail-safe braking and communication monitoring.
- Develop the system for real-time automotive safety applications.

Project Information

Project Title: CAN Based Intelligent Bus Accident Prevention and Automatic Braking System using LPC2129

Microcontroller: LPC2129 (ARM7TDMI-S)

CAN Transceiver: MCP2551

Communication Protocol: CAN 2.0A

Obstacle Sensor: HC-SR04 Ultrasonic Sensor

Speed Sensor: 10K Potentiometer

Motor Driver: L293D

Display: 16×2 LCD

Programming Language: Embedded C

IDE: Keil µVision

Simulation Tool: Proteus 8.13 (if used)
