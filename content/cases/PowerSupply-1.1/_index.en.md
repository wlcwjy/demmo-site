---
title: "ACDC1800W Core Board" 
date: 2026-09-01 
type: 'sample' 
layout: 'single' 
cascade: 
    showDate: false 
    showAuthor: false 
    invertPagination: true
---

## Introduction
<div style="text-align: justify;">
  The ACDC1800W Core Board is a high-performance embedded solution based on the STM32F105RxT6 microcontroller, integrated with a Bluetooth wireless communication module. Designed specifically for IoT applications, the system features AC/DC power conversion control, intelligent fan thermal management, LED status indication, CAN communication interfaces, and comprehensive power/load protection monitoring. The software architecture implements precise GPIO control, ADC sampling, and PWM output logic tailored to the hardware schematic.
</div>

## Hardware Showcase

{{< figure src="featured.png"
title="STM32 Core Board Hardware"
caption="💡 Features high-density component placement, integrating multi-phase power management and high-speed signal processing units to demonstrate extreme PCB space utilization."
alt="STM32 Core Board"
width="800" >}}

{{< figure src="hardware-back.png"
title="STM32 Core Board Hardware (Back)"
caption="💡 Onboard high-performance Bluetooth module supports low-power wireless debugging and data transparent transmission, empowering the ACDC1800W system with flexible IoT connectivity."
alt="STM32 Core Board Back"
width="800" >}}

## Software Showcase

{{< figure src="software.png"
title="STM32 Core Board Software Project"
caption="💡 Overview of the software project engineering structure."
alt="STM32 Core Board Software Project"
width="800" >}}

## Functional Specifications

------

### 🔋 Digital Power Flow Control

<div style="text-align: justify;">

- **Dynamic Voltage/Current Regulation**: Supports real-time command execution from the host. The internal PID algorithm adjusts PWM output to achieve stepless voltage regulation and precise constant current limiting.
- **Relay Timing Logic (Built-in Strict Power-On/Off State Machine)**:
  - *Pre-charge*: Pre-charging detection.
  - *Main-Connect*: Main circuit relay engagement.
  - *Aux-Control*: Auxiliary buck board linkage control.
- **Battery Handshake Protocol**: Automatically executes battery presence detection and voltage verification before enabling output to prevent damage from no-load or reverse connection.

</div>

------

### ❄️ Intelligent Thermal Management System

<div style="text-align: justify;">

- **Adaptive Fan Curve**: Abandoning traditional gear-based control, the system uses continuous PWM speed regulation. The software dynamically fits the optimal speed curve based on NTC temperature sampling, balancing silence and cooling efficiency.
- **Fan Fault Diagnosis**: Real-time monitoring of the fan FG signal (speed feedback). Upon detecting a fan stall or disconnection, the system immediately triggers derating or shutdown protection and reports fault codes.

</div>

------

### 👁️ Immersive Status Feedback

<div style="text-align: justify;">

- **RGB/LED Optical Language System (Intuitively conveying device "emotions" through combinations of frequency and color)**:
  - 🟢 **Green (Solid)**: Energy Full (Charged).
  - 🔴 **Red (2Hz Flash)**: Energy Injecting (Charging).
  - 🟡 **Yellow (Alert)**: System Anomaly or Fault Warning.

</div>

------

### 🛡️ Multi-Dimensional Security Defense Matrix

<div style="text-align: justify;">

- **Hardware-Level Fast Protection**: Monitors short-circuit protection pin levels with microsecond-level response to cut off output.
- **Software Over-Temperature/Over-Voltage Protection**: Dual ADC sampling verification to prevent single-point failures.
- **Communication Watchdog**: CAN communication timeout detection to prevent loss of control.

</div>

------

## 📡 Data Acquisition & Safety Protection

<div style="text-align: justify;">

- **Multi-channel ADC Sampling**: Real-time acquisition of critical analog quantities, including:
- **Input/Output Voltage**: Monitoring grid input and converted DC output.
- **Output Current**: Acquiring precise current values via sampling resistors and amplifier circuits.
- **Temperature Monitoring**: Collecting NTC thermistor data for over-temperature protection.
- **Hardware Protection Linkage**: Monitoring short-circuit and over-voltage protection signals; the software responds immediately to cut off output once hardware-level anomalies are triggered.

</div>