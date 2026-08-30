---
title: "ACDC1800w 核心板"
date: 2026-09-01 
type: 'sample'
layout: 'single'
cascade:
  showDate: false
  showAuthor: false
  invertPagination: true
---

## 简介

<div style="text-align: justify;">

&emsp;&emsp;ACDC1800w 核心板是一款基于 STM32F105RxT6 系列微控制器的核心板，集成蓝牙无线通信模组，专为物联网场景打造。系统主要功能包括 AC/DC 电源转换控制、风扇散热管理、状态指示（LED）、通信接口(CAN) 以及电源/负载保护监测。软件需根据硬件原理图实现相应的 GPIO 控制、ADC 采样及 PWM 输出逻辑。

</div>

## 硬件展示
{{< figure src="featured.png" 
           title="STM32 核心板硬件实物" 
           caption="💡 采用高密度器件排布，集成多相电源管理与高速信号处理单元，展示极致的 PCB 空间利用率。" 
           alt="STM32 核心板" 
           width="800" >}}

{{< figure src="hardware-back.png" 
           title="STM32 核心板硬件实物" 
           caption="💡 板载高性能蓝牙通信模组，支持低功耗无线调试与数据透传，为 ACDC1800W 系统赋予灵活的 IOT 互联能力。" 
           alt="STM32 核心板" 
           width="800" >}}

## 软件展示
{{< figure src="software.png" 
           title="STM32 核心板软件项目" 
           caption="💡 软件项目工程展示。" 
           alt="STM32 核心板软件项目" 
           width="800" >}}

## 功能说明
------
### 🔋  功率流数字控制

<div style="text-align: justify;">

- **动态电压/电流调节**：支持上位机指令实时下发，软件内部通过 PID 算法调节 PWM 输出，实现电压无级调节及 恒流限流控制。

- **继电器时序逻辑(内置严格的上电/掉电时序状态机)**：

  - *Pre-charge*：预充电检测。
  - *Main-Connect*：主回路吸合。
  - *Aux-Control*：辅助降压板联动控制。

- **电池握手协议**：在输出使能前，自动执行电池在位检测与电压校验，防止空载或反接损坏。

</div>

------
### ❄️ 智能热管理系统

<div style="text-align: justify;">

- **自适应风扇曲线**：摒弃传统的档位控制，采用连续 PWM 调速。软件根据 NTC 温度采样值，动态拟合最佳转速曲线，兼顾静音与散热。

- **风扇故障诊断**：实时监测风扇 FG 信号（转速反馈）。一旦检测到风扇堵转或未接入，立即触发降额运行或停机保护，并上报故障码。

</div>

------
### 👁️ 沉浸式状态反馈

<div style="text-align: justify;">

- **RGB/LED 光语系统(通过不同频率与颜色的组合，直观传递设备“情绪”)**：

  - 🟢 **Green (Solid)**: 能量满格（充满）。
  - 🔴 **Red (2Hz Flash)**: 能量注入中（充电）。
  - 🟡 **Yellow (Alert)**: 系统异常或故障预警。

</div>

------
### 🛡️ 多维安全防御矩阵

<div style="text-align: justify;">

- **硬件级快速保护**: 监测短路保护引脚电平，微秒级响应切断输出。
- **软件过温/过压保护**： 双重 ADC 采样校验，防止单点失效。
- **通信看门狗**： CAN 通信超时检测，防止失控。

</div>

------

## 📡 数据采集与安全保护

<div style="text-align: justify;">

- **多路 ADC 采样**：实时采集关键模拟量，包括：
- **输入/输出电压**：监测电网输入及转换后的直流输出。
- **输出电流**：通过采样电阻和放大电路获取精确电流值。
- **温度监测**： 采集 NTC 热敏电阻数据，用于过温保护。
- **硬件保护联动**：监测短路保护信号及过压保护信号，一旦硬件层面触发异常，软件需立即响应并切断输出。

</div>