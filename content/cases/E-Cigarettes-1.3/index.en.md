---
title: "E-cigarette Controller Solution"
layout: 'single'
categories: ["Personal Care"]
weight: 9
date: 2026-09-08
cascade:
  showDate: false
  showAuthor: false
  invertPagination: true
---

## Introduction

----

<div style="text-align: justify;">

&emsp;&emsp;The E-cigarette Controller Solution is a high-cost-performance, low-power control solution specifically designed for e-cigarette devices. Developed based on the Nyecon NY8 series microcontroller, this solution integrates core functions such as lithium battery charge/discharge management, atomization output control, multiple safety protections, and human-machine interaction indicators. Leveraging the high integration and excellent cost-effectiveness of the NY8 series chip, this solution achieves ultimate cost control and power consumption optimization while ensuring full functionality, making it an ideal choice for e-cigarette products.
</div>

## Hardware Showcase

{{< figure src="featured.png" 
           title="E-cigarette Controller PCB Front View" 
           caption="💡 E-cigarette Controller PCB Front View" 
           width="800" >}}

{{< figure src="hardware-back.png" 
           title="E-cigarette Controller PCB Back View" 
           caption="💡 E-cigarette Controller PCB Back View" 
           width="800" >}}

## Software Showcase

{{< figure src="software-main.png" 
           title="E-cigarette Controller Software Project Showcase" 
           caption="💡 Software Project Showcase - Developed based on Nyecon NY8 series MCU." 
           alt="E-cigarette Controller Project Showcase" 
           width="800" >}}

## Functional Description

### 1. Core Electrical Specifications & Cost Advantage

------

<div style="text-align: justify;">

> **Ultimate Cost-Optimized Solution**:
>  - **Main Control Chip:** Adopts the Nyecon NY8 series 8-bit microcontroller. This chip features a built-in high-precision RC oscillator, ADC, and PWM. Its extremely streamlined peripheral circuit minimizes BOM cost while providing sufficient computing power, making it highly competitive in the market.
>  - **Integration:** A single chip integrates key detection, LED driving, atomization output control, and battery management logic, eliminating the need for additional expensive dedicated logic chips.

> **Electrical Specifications**:
>  - **Operating Voltage:** 3.0V ~ 4.2V (compatible with a single 3.7V lithium battery).
>  - **Static Power Consumption:** < 5μA. The extremely low standby power consumption ensures the battery can last for months even when the product is not in use for a long time.
>  - **Charging Specification:** Supports 5V/400mA USB input charging.
>  - **Load Capability:** Can drive heating coils with a minimum resistance of 0.35Ω, meeting high-power output requirements.

</div>

### 2. Intelligent Operation & Interaction Logic

------

<div style="text-align: justify;">

> This controller provides users with clear status feedback through simple button operations and LED indicators.

> **Power On/Off & Lock**:
>  - **Five-Click Power On/Off:** Press the button 5 times consecutively within 2 seconds to lock or unlock the vaping function. Upon successful mode switching, the 4 battery level indicator lights will flash 3 times as a prompt.

> **Battery Level Indication**:
>  - **Power-On Self-Test:** When the device is powered on, the 4 battery level indicator lights will flash 3 times. If no vaping is detected, it will automatically enter sleep mode.
>  - **Charging Status:** During charging, the 4 battery level indicator lights will dynamically display the current battery level. When charging, the first light stays on, the second light flashes, and so on. When all 4 LED lights are constantly on, it indicates the battery is fully charged.

</div>

### 3. Hardware Architecture & Multiple Safety Protections

------

<div style="text-align: justify;">

> **Multiple Safety Protection Mechanisms**:
>  - **Short-Circuit Protection:** When a short circuit in the heating coil is detected, the system will immediately cut off the output and alert the user by flashing the 4 battery level indicator lights 3 times, effectively preventing device damage and safety hazards.
>  - **Over-Inhalation Protection:** If a single vaping session exceeds 10 seconds, the system will automatically stop the output and prompt the user by flashing the 4 battery level indicator lights 5 times, protecting both the user and the device.
>  - **Low-Voltage Protection:** When the battery voltage drops below 3.2V, the last battery level indicator light will flash 15 times, and vapor production will stop. The low-voltage alarm will automatically clear once the voltage recovers.

> **Hardware Architecture**:
>  - **MCU:** Nyecon NY8 series, compact and powerful.
>  - **Power Management:** Built-in charging management circuit supports direct USB charging.
>  - **Drive Circuit:** Uses a MOSFET to drive the heating coil, ensuring fast switching response and stable output.

</div>

### 4. Software Logic & User Experience

------

<div style="text-align: justify;">

> **Intelligent Wake-up Logic**:
>  - In sleep mode, once the device detects a user's vaping action, it will be immediately awakened and enter full-power output state, ensuring a rapid response.

> **Low-Power Design**:
>  - The chip automatically enters sleep mode when idle, retaining only the key wake-up function. This keeps the overall system power consumption at the microampere level, maximizing standby time.

> **Status Feedback**:
>  - All operational and protection statuses are clearly fed back through LED indicators, allowing users to understand the device status at any time for a more reassuring experience.

</div>