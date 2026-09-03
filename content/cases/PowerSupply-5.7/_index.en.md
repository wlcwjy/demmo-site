---
title: "BOOST Power Supply Based on EG1164" 
type: 'sample' 
layout: 'single' 
categories: ["power"]
weight: 3 
date: 2026-08-31 
cascade: 
    showDate: false 
    showAuthor: false 
    invertPagination: true
---

  ## Introduction

  ------

  &emsp;&emsp;This module is a smart power driver board specifically designed for telescopic pole mechanisms. Utilizing the high-performance EG1164 chip as its core controller, the system achieves an efficient BOOST step-up conversion from a 12V DC input to a 24V DC output. Through an onboard high-power relay array, it precisely controls the direction of the output current, thereby driving loads (such as electric linear actuators) to achieve bidirectional extension and retraction.

  ## Hardware Showcase

  {{< figure src="featured.png"
  title="DCDC-MPPT Digital Power Hardware Circuit"
  caption="💡 3D Render of the 12V to 24V BOOST Step-up and Relay Reversing Driver Module Based on the EG1164 Chip"
  width="800" >}}

  ## Software Showcase

  {{< figure src="software-main.png"
  title="BOOST Power Supply Software Project Based on EG1164"
  caption="💡 Software Project Showcase - Relevant code for the main function."
  alt="BOOST Power Supply Project Based on EG1164"
  width="800" >}}

  {{< figure src="software-crontab.png"
  title="BOOST Power Supply Project Based on EG1164"
  caption="💡 Software Project Showcase - Relevant code for scheduled tasks."
  alt="BOOST Power Supply Project Based on EG1164"
  width="800" >}}

  ## Functional Specifications

  ### 1. Core Electrical Specifications

  ------

> **Input Voltage Range**: DC 12V (Nominal), supporting wide voltage input fluctuations.

> **Output Voltage**: Stable DC 24V (BOOST step-up mode).

> **Output Current Capability**:
>    - Continuous Output Current: 20A (Inferred from the onboard YA318-2A-U 12V 2X10A relay specifications, utilizing dual parallel or redundant design).
>   - Peak Current: Supports short-term overloads to handle motor startup surges.

> **Conversion Efficiency**: >90% (Benefiting from the EG1164 synchronous rectification technology and low Rds(on) MOSFETs).

> **Switching Frequency**: Approximately 100kHz-150kHz (Typical operating frequency of the EG1164, working in conjunction with the onboard inductor L1 and filter capacitors).

> **Control Logic Level**: TTL/CMOS compatible (3.3V/5V can drive the optocoupler or transistor control terminals).

  ### 2. Hardware Architecture and Key Components

  ------

> **Main Control Chip**: Utilizes the **EG1164** high-voltage, high-current synchronous rectification step-up chip. This chip features built-in power MOSFETs and a high-precision feedback loop to ensure stable 24V output.

> **Power Switching and Reversing Unit**:
>    - Onboard dual high-power relays.
>    - The relays adopt an H-bridge or polarity-reversing configuration, switching contacts via logic signals to change the polarity of the output terminals (VOUT+/VOUT-), thereby achieving motor forward and reverse rotation.

> **Filtering and Energy Storage System**:
>    - **Input/Output Filtering**: Multiple solid-state capacitors are distributed across the VIN and VOUT areas to effectively filter out high-frequency ripples and prevent battery voltage drops.
>   - **Power Inductor**: A large-sized shielded power inductor works with the chip for energy storage and release.

> **Drive and Protection Circuits**:
>    - **MOSFET Array**: Multiple TO-263 packaged high-power MOSFETs are onboard, used for auxiliary current expansion or as pre-stage switches for relay driving.
>    - **Optocoupler Isolation**: Optocoupler components are visible near the right-side pin header, providing electrical isolation between control signals and the high-voltage power section to enhance anti-interference capabilities.

  ### 3. Safety Protection System

  ------

 > To ensure equipment safety during the telescopic pole operation, this module is designed with multiple protection mechanisms:

>  **Overcurrent Protection (OCP)**: The EG1164 features built-in cycle-by-cycle current limiting. When the inductor current exceeds the threshold, the output is automatically shut down to prevent MOSFET burnout.

>  **Over-Temperature Protection (OTP)**: The chip integrates an internal temperature sensor. When the junction temperature exceeds the safe limit (typically 150℃), it automatically reduces frequency or shuts down.

> **Input Under-Voltage/Over-Voltage Lockout (UVLO/OVP)**: Prevents damage from battery over-discharge or abnormal input voltage spikes that could break down downstream circuits.

> **Output Short-Circuit Protection**: The circuit board rapidly cuts off the output if a short circuit occurs in the electric motor wiring.

  ### 4. Application Scenarios

  ------

> Electric lifting systems for surveillance poles/cameras.

> Solar panel angle adjustment actuator control.

> Height adjustment mechanisms for automated warehouse shelving.