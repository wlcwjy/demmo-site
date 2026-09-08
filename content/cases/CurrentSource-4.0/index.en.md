---
title: "Disinfection Toothbrush Box Controller"
layout: 'single'
categories: ["Personal Care"]
weight: 7
date: 2026-09-08
cascade:
  showDate: false
  showAuthor: false
  invertPagination: true
---

## Introduction

----

<div style="text-align: justify;">

&emsp;&emsp;The Disinfection Toothbrush Box Controller is a low-cost, low-power control solution specifically designed for portable personal care devices. Developed based on the Nyecon NY8 series microcontroller, it integrates core functions including lithium battery charge/discharge management, UVC deep-UV sterilization, motor drive, and human-machine interaction indicators. Leveraging the high integration and 内部技能lent cost-performance ratio of the NY8 series chip, this solution achieves ultimate cost control and power optimization while maintaining full functionality, making it an ideal choice for electric toothbrush disinfection boxes, travel sterilization cases, and similar products.
</div>

## Hardware Showcase

{{< figure src="featured.png" 
           title="Disinfection Toothbrush Box Controller PCB Front View" 
           caption="💡 Disinfection Toothbrush Box Controller PCB Front View" 
           width="800" >}}

{{< figure src="hardware-back.png" 
           title="Disinfection Toothbrush Box Controller PCB Back View" 
           caption="💡 Disinfection Toothbrush Box Controller PCB Back View" 
           width="800" >}}

## Software Showcase

{{< figure src="software-main.png" 
           title="Disinfection Toothbrush Box Controller Software Project Showcase" 
           caption="💡 Software Project Showcase - Developed based on Nyecon NY8 series microcontroller." 
           alt="Disinfection Toothbrush Box Controller Project Showcase" 
           width="800" >}}

## Functional Specifications

### 1. Core Electrical Specifications and Cost Advantages

------

<div style="text-align: justify;">

> **Ultimate Cost-Optimized Solution**:
>  - **Main Controller Chip:** Adopting the Nyecon (Nyecon) NY8 series 8-bit microcontroller. This chip features a built-in high-precision RC oscillator, ADC, and PWM, with extremely simplified peripheral circuits. It delivers sufficient computing power while minimizing BOM cost, offering strong market competitiveness.
>  - **High Integration:** A single chip integrates button detection, LED driving, UVC control, and battery management logic, eliminating the need for additional expensive dedicated logic chips.

> **Electrical Parameter Specifications**:
>  - **Operating Voltage:** 3.0V ~ 4.2V (compatible with a single lithium battery cell).
>  - **Quiescent Power Consumption:** < 1μA. The ultra-low standby power consumption ensures that the battery can maintain charge for months even when the product is unused for extended periods.
>  - **Charging Specifications:** Supports 4.5V~5.5V USB input, with charging current set at 100mA, balancing charging efficiency and battery lifespan.
>  - **Under-Voltage Protection:** 3.0V output cutoff, 3.2V warning, preventing battery damage from over-discharge.

</div>

### 2. UVC Disinfection Function and Low-Power Management

------

<div style="text-align: justify;">

> This controller is specifically designed for UVC deep-UV sterilization, achieving maximum energy savings through precise timing control while ensuring effective sterilization.

> **UVC Disinfection Parameters**:
>  - **UV Light Wavelength:** 270~280nm (the golden band for deep-UV sterilization).
>  - **Beam Angle:** 120-degree wide-angle coverage, ensuring no dead zones inside the disinfection chamber.
>  - **Operating Power:** 240mW (at the LED), with radiant power of approximately 6mW.

> **Smart Operating Modes**:
>  - **Single Operation Duration:** Set to 3 minutes. This duration is sufficient for a complete sterilization cycle, after which the device automatically powers off to avoid unnecessary power consumption.
>  - **Low-Power Logic:** The chip automatically enters sleep mode when idle, retaining only button wake-up functionality, keeping the overall system power consumption in the microampere range.

</div>

### 3. On-Board Protection and Hardware Architecture

------

<div style="text-align: justify;">

> **Lithium Battery Protection Mechanisms**:
>  - **Over-Charge Protection:** 4.3V charge cutoff, 4.1V recovery, preventing battery swelling.
>  - **Over-Current Protection:** 9A hardware-level over-current protection, preventing circuit damage from short circuits.
>  - **Low-Voltage Protection:** 2.4V discharge cutoff, 3.0V recovery, protecting battery activity.

> **Hardware Architecture**:
>  - **MCU:** Nyecon NY8 series (SOP8 package), compact and space-saving.
>  - **Power Management:** Built-in LDO and charging management circuit, supporting direct USB charging.
>  - **Drive Circuit:** MOSFET-driven UVC LEDs for fast switching response.

</div>

### 4. Operation and Indicator Instructions

------

<div style="text-align: justify;">

> **Button Operation Logic**:
>  - **Power On/Off:** In the powered-off state, a short press turns on the device with the blue indicator LED lighting up; in the powered-on state, a long press turns off the device.
>  - **Auto Power-Off:** Automatically enters the powered-off state after 3 minutes of operation, requiring no manual intervention.

> **LED Indicator Status**:
>  - **Operating Status:** Blue indicator LED stays on, indicating active disinfection or operation.
>  - **Charging Status:** Connecting the USB charger automatically wakes the device into charging mode. The red indicator LED lights up during charging and turns off when fully charged.

</div>