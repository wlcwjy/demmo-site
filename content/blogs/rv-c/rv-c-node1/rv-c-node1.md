---
title: "序言&CAN ID 解析"
date: 2026-10-22
description: "序言&CAN ID 解析"
summary: "使用 Mermaid 可以轻松地将图表和流程图添加到文章中。"
tags: []
type: 'sample'
---

[TOC]

## 1.序言

&emsp;&emsp;最近在开发RV-C 协议相关的产品,网上检索关于RV-C协议的内容相对较少,只能自己摸索.这里写一些自己理解协议的笔记,便于开发中随时回溯,也方便与需要使用这个协议的朋友互相学习探讨.

&emsp;&emsp;RV-C全称‌**RV Industry Association Communication**‌(房车行业协会通信),是由‌**北美房车工业协会（RVIA）**‌制定的基于‌**CAN 2.0 总线**‌的房车专用通信标准，用于实现房车内部电子设备间的标准化数据交换 。‌‌‌协议中简介的原文如下:

&emsp;&emsp; RV-C is a CAN-based communication network for recreation vehicles. This document details physical, administration, diagnostic, and application requirements to allow all RV-C network connected devices to interoperate and be maintained. 

&emsp;&emsp;RV-C协议**核心目的**主要解决以下两大问题:

- ‌**互操作性**‌：解决不同制造商设备之间的兼容性问题，使控制系统、电源系统、娱乐系统等能够协同工作 。
- ‌**标准化**‌：为制造商提供统一的设计规范，降低开发复杂度与成本，同时提升用户的使用体验 

&emsp;&emsp;目前开发欧美房车相关的产品大多少都使用该协议.

&emsp;&emsp;协议中的第二章主要写的物理层相关的内容,也就是硬件相关的内容,我们这里主要的做的是解析协议,就直接跳过第二章.

&emsp;&emsp;那就从协议中的第三章开始吧.

## 2.核心术语说明

### 2.1 **Node or "RV-C Node" or "CAN Node"** —— 硬件实体

&emsp;&emsp;原文:A microprocessor that sends and receives data on an RV-C bus. More technically, a CAN 2.0B master node consisting of a transceiver, controller, and microprocessor/microcontroller to implement the RV-C protocol.

&emsp;&emsp;原文大致意思是收发器、控制器和 MCU 组成的 CAN 2.0B 主节点。协议在这里把“Node”严格定义为**物理上的硬件板子**。它强调的是硬件的物理存在（一个收发器、一个 CAN 控制器）。说白了就是一个基本硬件实体的意思主要的界限就是含有一个CAN通讯能力的硬件实体,比如说一个逆变器,当然如果这个设备中除了逆变还包含一个充电器,但是它共用一个CAN收发设备虽然是包含了两个设备但也是一个 **Node or "RV-C Node" or "CAN Node"**.这里有三个名称为了后续的方便统一使用**RV-C Node**描述该术语.

### 2.2 **SA (Source Address)** —— 节点的唯一身份证

&emsp;&emsp;原文:A unique identifier for each node on an RV-C network. Source addresses may be hard coded by a device, or claimed dynamically on startup per section 3.3 of this specification. A node maintains a single source address regardless of the number of functions or device types it supports.

&emsp;&emsp;原文的大致意思是**SA (Source Address)**是网络上每个节点的唯一标识符。**节点仅维护一个唯一的源地址（single source address）**，无论它支持多少功能。**SA = 硬件板子的 IP 地址**。无论这个板子上插了多少个功能模块，或者跑了多少个虚拟机，它在总线上的物理身份（Source Address）只有一个。就像2.1中举的例子,一个逆变器如果除了逆变还包含一个充电器,但是它共用一个CAN收发也就是一个**Node or "RV-C Node" or "CAN Node"**,就只能申请一个**SA**.这个**SA**名称后续再CANID组成中有使用,所以比较关键.

 ### 2.3 **Device Type (设备类型) **—— 节点内部的软件分身

&emsp;&emsp;原文:A set of functions or capabilities possessed by a node e.g. “awning”, “inverter”, or “water heater”. Each device type has a set of RV-C messages associated with it as defined in section 6 of this specification; e.g. a node supporting the “awning” device type might broadcast/receive RV-C messages “AWNING_COMMAND” and/or “AWNING_STATUS”.
&emsp;&emsp;A node may support multiple device types or multiple instances of a device type (e.g. “awning 1” and “awning 2”.)

&emsp;&emsp;原文的主要意思是**RV-C Node**拥有的一组功能或能力（如“遮阳棚”、“逆变器”）。节点可以支持多种设备类型或同一设备类型的多个实例（如 `awning 1` 和 `awning 2`）。这是为“多合一”硬件（比如前面的说的一个逆变器如果除了逆变还包含一个充电器）的实现提供基础。**Device Type = 软件逻辑上的虚拟设备**。虽然物理上只有一个 SA，但板子内部的 MCU 可以“精神分裂”，在逻辑上扮演多个 Device Type（比如扮演逆变器，又扮演充电器）。

&emsp;&esmp;原文中提到了 `awning 1` 和 `awning 2`，这就是协议中 **`Instance`（实例）** 概念的雏形，用于区分同一类型下的不同虚拟设备。但是协议中的术语中并没有**`Instance`（实例）**的说明,后面成章节才有说明,做一个关于**`Instance`（实例）**也放再后面再说,并且后续了解的内容积累多了一并说说关于**`Instance`（实例）**的各种示例.

### 2.4 **DSA (Default Source Address)** —— 诊断报警时的“临时工牌号”

&emsp;&emsp;原文：An identifier associated with each device type as defined in Table 7.2. The DSA is included when a node broadcasts its diagnostic message (DM_RV); a node supporting multiple device types broadcasts the DM_RV for each of its device types with the correct DSA in each DM_RV message.

&emsp;&emsp;原文提及**DSA**与每个 Device Type 关联的标识符,具体定义再表7.2中。在广播 `DM_RV` 诊断报文时必须携带。支持多种设备类型的节点，需要为每个设备类型发送对应的 DM_RV(这里提及的DM_RV是RV-C协议的相当重要的数据包，后续会专门解析说明)。**DSA**主要解决一个**RV-C Node**中包含多个**Device Type**情况下**诊断混淆**的问题。

> #### **对于一个**RV-C Node**只能有一个**SA**，那么当 一个内**RV-C Node**有`Device Type A` 和 `Device Type B` 同时报警时，怎么区分是谁在报警？**

> 这就是 **DSA** 存在的意义。当发送 `DM_RV` 时，协议报文中填上特定 Device Type 对应的 DSA，这样就能分清一个**RV-C Node**内具体是`Device Type A` 还是 `Device Type B` 报警了

> #### 那又有一个新的问题对于一个**RV-C Node**只能有一个**SA**，那么当 一个内**RV-C Node**有`Device Type A1` 和 `Device Type A2` 同时报警时，那DSA岂不是也是一样的，那又怎么区分是谁在报警？**
>
> 这里只要就涉及到了DM_RV中的的**Bank Select**，整个协议中关于**Bank**的说明较少，个人认为如果**SA**一样，**DSA**也一样的情况下就需要使用BANK来区分 **Device Type**了，后续解析DM_RV时候在详细说明

## 3 RV-C 报文**在总线上的原始 29-bit CAN ID 结构**以及**数据域**

&emsp;&emsp;CAN 报文的原始表格如下

| DLL fields  |           | RV-C name (abbreviation) | Description                                                  |
| ----------- | --------- | ------------------------ | ------------------------------------------------------------ |
| Name        | Bit       |                          |                                                              |
| Base ID     | 28 to 26  | Priority                 | 111b – Lowest priority 000b – Highest priority               |
| Base ID     | 25        | Reserved                 | Always 0b                                                    |
| Base ID     | 24 to 18  | Data Group Number - High | Identifies how the data packet should be parsed, possibly in combination with the DGN-Low |
| Extended ID | 17 and 16 | Data Group Number - High | Identifies how the data packet should be parsed, possibly in combination with the DGN-Low |
| Extended ID | 15 to 8   | Data Group Number - Low  | Either determines the target node for the message, or with the DGN-High determines how the data packet should be parsed. |
| Extended ID | 7 to 0    | Source address (SA)      | Shall be unique for each node                                |
| Data        | 64 to 0   | Data                     | Defined in detail in the RV-C application profile specification |

------

### **3.1 报文的底层身份：29-bit CAN ID 的三大要素**

&emsp;&emsp;RV-C 是基于 CAN 2.0b 扩展帧协议设计的，它的 29 位 ID（`Base ID` + `Extended ID`）被严格划分为了三部分，分别承担不同的职责：

#### **3.1.1 优先级 (Priority) —— 决定谁先发**

- **位置**：Bit 28 ~ 26（3 bit）
- **作用**：这是总线的**“VIP通道”**。当多个设备同时在总线上发消息时，数字越小的优先级越高（`000b` 最高，`111b` 最低）。
- **实际应用**：例如，`DM_RV`（诊断报警）的默认优先级是 `6` (`110b`)，而常规状态帧的优先级可能是 `7`。这意味着当逆变器报警时，即使总线很堵，报警信息也能挤到最前面先发出去，防止被常规数据淹没。

#### **3.1.2 数据组编号 (DGN) —— 决定“内容是什么”**

- **位置**：Bit 24 ~ 8（17 bit），由 DGN-High (24-18) 和 DGN-Low (15-8) 组合而成。
- **作用**：这相当于标明报文的**“内容类型”**。
- **实际应用**：协议的DGN所以内容定义都再表7.7中，比如DGN=1FFFF表中含义为DATE_TIME_STATUS,解析端看到它就知道这是DATE_TIME_STATUS数据。

#### **3.1.3 源地址 (Source Address, SA) —— 决定“我是谁”**

- **位置**：Bit 7 ~ 0（8 bit）
- **作用**：这是节点的**“身份证号”**。协议要求每个节点的 SA 必须是唯一的。
- **总线仲裁的兜底机制**：协议中特意强调了：“当优先级相同时，SA 作为最后的仲裁决胜局（tiebreaker）。”这里关于SA 的仲裁也是RV-C的重要内容，后续会详细说明

------

### **3.2 报文的实际载荷：数据域 (Data)**

- **位置**：`Data`，共 64 到 0 位（即标准 CAN 帧的 8 字节数据）。
- **作用**：传递真实的数据包，具体数据含义协议有关于每一个DGN的数据说说明。

------

# **总结**
&emsp;&emsp;OK,关于序言就先写怎么多，下一个章节重点说一下**instance(实例)**，这个其实非常关键，但是协议中的说明的内容并不多。




&emsp;&emsp;**以上所有都是个人的学习笔记，若有错误的地方欢迎大家指点。**

  **探索不能只是看结果，过程也很重要。知识就是财富，每次积累一点点，终究会有质的飞跃。**

&emsp;&emsp;**个人工作室:https://site.demmo.top/**

  **联系方式[wlc@demmo.top](mailto:wlc@demmo.top),[wlcwjy@163.com](mailto:wlcwjy@163.com),欢迎大家指教。**
                          **幽灵**