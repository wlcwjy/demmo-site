---
title: "Fascia Gun Controller Solution"
layout: 'single'
categories: ["Personal Care"]
weight: 10
date: 2026-09-08
cascade:
  showDate: false
  showAuthor: false
  invertPagination: true
---

## Introduction

----

<div style="text-align: justify;">

&emsp;&emsp;The Fascia Gun Controller Solution is a high-cost-performance, low-power control solution specifically designed for deep muscle massage devices. Developed based on the Nuvoton series microcontroller, this solution integrates core functions such as brushless motor drive control, three-speed adjustment, LED battery indicator, and multiple safety protections. Leveraging the high integration and 内部技能lent cost-effectiveness of the Nuvoton series chip, this solution achieves ultimate cost control and power consumption optimization while ensuring powerful power output and precise speed control, making it an ideal choice for fascia guns, massage guns and other health care products.

</div>

## Hardware Showcase

{{< figure src="featured.png" 
           title="Fascia Gun Controller PCB Front View" 
           caption="💡 Fascia Gun Controller PCB Front View" 
           width="800" >}}

{{< figure src="hardware-back.png" 
           title="Fascia Gun Controller PCB Back View" 
           caption="💡 Fascia Gun Controller PCB Back View" 
           width="800" >}}

## Software Showcase

{{< figure src="software-main.png" 
           title="Fascia Gun Controller Software Project Showcase" 
           caption="💡 Software Project Showcase - Developed based on Nuvoton series MCU." 
           alt="Fascia Gun Controller Project Showcase" 
           width="800" >}}

## Functional Description

### 1. Core Electrical Specifications & Cost Advantage

------

<div style="text-align: justify;">

> **Ultimate Cost-Optimized Solution**:
>  - **Main Control Chip:** Adopts the Nuvoton series microcontroller. This chip features a built-in high-precision RC oscillator, ADC, and PWM. Its extremely streamlined peripheral circuit minimizes BOM cost while providing sufficient computing power, making it highly competitive in the market.
>  - **Integration:** A single chip integrates key detection, LED driving, brushless motor drive control, and battery management logic, eliminating the need for additional expensive dedicated motor driver chips, significantly reducing system cost.

> **Electrical Specifications**:
>  - **Rated Operating Voltage:** DC14.8V (compatible with 4S lithium battery pack).
>  - **Rated Power:** Approximately 22W, meeting high-torque continuous output requirements.
>  - **Shutdown Static Current:** < 5μA (mainly battery self-discharge). The extremely low standby power consumption ensures the battery can last for months even when the product is not in use for extended periods.
>  - **Startup Standby Current:** < 200mA.
>  - **Maximum Operating Current:** 6A, supporting powerful massage output.
>  - **Charging Specification:** Supports AC100V~240V wide voltage input, output DC16.8V/700mA, fully charges in approximately 3 hours.

</div>

### 2. Motor Control & Three-Speed Adjustment

------

<div style="text-align: justify;">

> This controller adopts a constant speed control strategy, achieving smooth switching between three massage speed levels through precise PWM duty cycle adjustment, meeting the massage needs of different body parts and intensities.

> **Three-Speed Adjustment Control**:
>  - **Level 1 (Low Speed):** Massage vibration frequency approximately 1800rpm, suitable for superficial muscle relaxation and daily health care.
>  - **Level 2 (Medium Speed):** Massage vibration frequency approximately 2400rpm, suitable for moderate intensity muscle massage.
>  - **Level 3 (High Speed):** Massage vibration frequency approximately 3000rpm, suitable for deep muscle tissue relaxation and post-exercise recovery.
>  - **Control Method:** Constant speed control, control frequency fixed at approximately 12kHz. The motor controller adjusts the PWM duty cycle based on the feedback actual speed, achieving precise motor speed control.
>  - **Stroke Amplitude:** Approximately 12mm, effectively reaching deep muscle layers.

> **Brushless Motor Drive**:
>  - **Motor Model:** Compatible with BL4520O brushless motor, rated speed 3500rpm, rated torque 60mNm.
>  - **Maximum Speed:** No-load speed up to 3800rpm, maximum current limit 6A, ensuring ample power without exceeding limits.
>  - **PWM Control Frequency:** Recommended 20kHz for quiet and smooth operation, reducing audible noise.

</div>

### 3. Hardware Architecture & Multiple Safety Protections

------

<div style="text-align: justify;">

> **Multiple Safety Protection Mechanisms**:
>  - **Stall Protection:** When the motor stall time exceeds 2 seconds, the controller immediately stops output to prevent motor burnout. Protection is cleared by restarting.
>  - **Low Voltage Protection:** When the battery voltage drops below approximately 13V, the controller stops output and alerts the user via LED indicator; when the voltage drops below the startup voltage of approximately 11.8V, the product is prohibited from starting, effectively preventing battery over-discharge damage.
>  - **Overcharge Protection:** When the battery voltage exceeds the full charge voltage of approximately 16.8V during charging, the controller cuts off the charging circuit to prevent battery swelling or fire.
>  - **Charging Timeout Protection:** When connected to a charger for continuous charging for 4 hours and the battery is still not fully charged, the controller automatically cuts off the charging circuit to prevent safety hazards from prolonged charging.
>  - **Overtemperature Protection:** When the driver IC temperature reaches the 110°C protection threshold, the controller stops output to prevent device damage from overheating; automatically recovers when temperature decreases.

> **Hardware Architecture**:
>  - **MCU:** Nuvoton series, compact and powerful.
>  - **Battery System:** Compatible with CHY-4S1P (4S1P) lithium battery pack, nominal capacity 2000mAh, rated power 29.6Wh, cycle life 800 cycles (capacity >80%).
>  - **Power Management:** Built-in charging management circuit supports AC100V~240V wide voltage charger direct charging.
>  - **Drive Circuit:** Uses MOSFET to drive the brushless motor, ensuring fast switching response and stable, reliable output.

</div>

### 4. Operation & Indicator Instructions

------

<div style="text-align: justify;">

> **Button Operation Logic**:
>  - **Power On:** In the shutdown state, long press the button for approximately 1 second to power on. The product defaults to Level 1 operation.
>  - **Power Off:** In the operating state at any level, long press the button for approximately 1 second to power off.
>  - **Level Switching:** In the operating state, at Level 1 or 2, short press the button once to increase level; at Level 3, short press the button once to power off directly. To continue use, simply power on again.

> **LED Battery Indicator**:
>  - **1 LED Lit:** Battery 0%~33%.
>  - **2 LEDs Lit:** Battery 33%~66%.
>  - **3 LEDs Lit:** Battery 66%~100%.
>  - **Charging Indicator:** After connecting the charger, LEDs perform a running display based on current battery level; after charging is complete, all 3 LEDs stay on continuously, indicating the battery is fully charged.
>  - **Low Battery Alert:** When battery level is insufficient, the corresponding LED flashes as a prompt.

> **Massage Head Compatibility**:
>  - **Ball Head:** Suitable for full-body muscle fascia tissue, softer texture, usable on chest, back, waist, and arms.
>  - **U-Shaped Head:** Suitable for massaging the neck and spine, also usable on shoulders and triceps.
>  - **Cylindrical Head:** Suitable for joints and palms; joint striking requires professional operation.
>  - **Flat Head:** Suitable for large muscle groups, harder plastic texture with greater pressure, providing deeper muscle stimulation.

</div>