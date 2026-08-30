---
title: "DCDC-MPPT Digital Power Supply" 
date: 2026-09-01 
type: 'sample' 
layout: 'single' 
cascade:   
    showDate: false   
    showAuthor: false   
    invertPagination: true
---

## Introduction

------

  The DCDC-MPPT Digital Power Supply is a digital power system designed around the STM32F334 microcontroller. Utilizing an advanced four-switch synchronous BUCK-BOOST topology, it integrates the main power stage, driver circuitry, auxiliary power supply, and signal conditioning circuits. Beyond high-precision DC-DC conversion, this module offers abundant communication and debugging interfaces (SWD, UART). It supports voltage and current adjustments via physical buttons or digital commands, and features MPPT (Maximum Power Point Tracking) technology for highly efficient solar power applications.

## Hardware Showcase

{{< figure src="featured.png"
           title="DCDC-MPPT Digital Power Supply Hardware Circuit"
           caption="💡 3D PCB View of the DCDC-MPPT Digital Power Supply System. This circuit board is specifically designed for high-performance power conversion, with a core digital control unit based on STM32. The upper section is a high-power-density area featuring high-power MOSFETs and a filter capacitor array, responsible for efficient digital DC-DC power stage conversion. The lower section is a precision control and interface area housing the main MCU and communication ports. It runs complex MPPT (Maximum Power Point Tracking) algorithms to adjust load impedance in real-time for maximum energy harvesting efficiency. Additionally, it includes a smart cooling fan interface and multiple hardware protection circuits to ensure system stability and safety under full-load conditions."
           alt="3D View of DCDC-MPPT Digital Power Supply Hardware Circuit"
           width="800" >}}

## Software Showcase

{{< figure src="software-mppt.png"
           title="STM32 Core Board Software Project"
           caption="💡 Software Project Showcase - MPPT Control Related Code."
           alt="STM32 Core Board Software Project"
           width="800" >}}

{{< figure src="software-pi.png"
           title="STM32 Core Board Software Project"
           caption="💡 Software Project Showcase - PI Control CV/CC/MPPT Loop Control Related Code."
           alt="STM32 Core Board Software Project"
           width="800" >}}

## Features

### 1. Core Electrical Specifications

------

Based on the hardware design metrics, the system features a wide input voltage range and highly flexible output characteristics:

- **Topology**: Four-switch BUCK-BOOST (supports seamless buck-boost transition).
- **Input Characteristics**: Wide-range DC input from **12Vdc to 48Vdc**.
- **Output Characteristics**:
  - Voltage Range: **5Vdc to 48Vdc** (covering common low-voltage load requirements).
  - Current Capability: Rated output of **5A**, with a recommended maximum operating power of **200W**.
  - Ripple Performance: Peak-to-peak ≤ 1% of output voltage, ensuring clean power delivery.
- **Switching Frequency**: 100kHz (high-frequency design reduces the size of passive components).

### 2. Safety Protection System

------

The system incorporates comprehensive hardware and software protection mechanisms to ensure device safety under abnormal operating conditions. All protections support **automatic recovery**:

- **Short Circuit Protection (SCP)**: Real-time output monitoring with immediate shutdown upon short circuit.
- **Over-Voltage / Under-Voltage Protection (OVP/UVP)**:
  - Input Side: Triggers protection when <12V or >48V.
  - Output Side: Triggers protection when >48V.
- **Over-Current Protection (OCP)**: Limits maximum output current to within 5A.

### 3. MPPT Functionality

------

Powered by the robust processing capabilities of the STM32F334 and its 12-bit high-speed ADC, this power supply possesses the necessary hardware foundation to achieve highly efficient **MPPT (Maximum Power Point Tracking)**.

- **Algorithm Implementation**:
  The core of MPPT lies in finding the optimal operating point of the solar panel or energy source in real-time. The fast ADC integrated within the STM32F334 can simultaneously sample input voltage ($V_{in}$) and input current ($I_{in}$) at microsecond-level speeds to calculate instantaneous power ($P_{in}$). The MCU runs a Perturb and Observe (P&O) algorithm internally, dynamically adjusting the PWM duty cycle of the BUCK-BOOST circuit to change the equivalent input impedance, thereby locking onto the maximum power point.
- **Application Advantages**:
  In solar power generation or energy harvesting systems, when fluctuations in light intensity cause input voltage variations, the DCDC-MPPT digital power supply can automatically adjust its operating state. This ensures maximum energy transfer efficiency from the source, significantly improving efficiency compared to traditional fixed-voltage charging.

### 4. Hardware Interfaces and Human-Machine Interaction

------

To facilitate debugging and monitoring, the module is equipped with rich human-machine interaction interfaces:

- **OLED Display**: Real-time display of input/output voltage, current, power, and MPPT parameters.
- **Physical Controls**: Onboard buttons for manual fine-tuning of output voltage or current limits.
- **Debugging Interfaces**: Reserved SWD programming/debugging port and USART communication port.