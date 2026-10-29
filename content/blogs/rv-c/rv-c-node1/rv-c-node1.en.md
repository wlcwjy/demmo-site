---
title: "Preface & CAN ID Parsing"
date: 2026-10-22
description: "Preface & CAN ID Parsing"
summary: "Use Mermaid to easily add charts and flow diagrams to your articles."
tags: []
type: 'sample'
---

## 1. Preface
&emsp;&emsp;Recently I have been developing products based on the RV-C protocol. There is relatively little content about the RV-C protocol available online, so I have had to figure things out on my own. Here I am writing down some notes on my understanding of the protocol, both for easy reference during development and to share and discuss with friends who need to use this protocol.
&emsp;&emsp;RV-C stands for **RV Industry Association Communication**, a dedicated communication standard for recreational vehicles based on the **CAN 2.0 bus**, established by the **RV Industry Association (RVIA)** in North America. It is used to enable standardized data exchange among the electronic devices inside an RV. The original text of the protocol is as follows:
&emsp;&emsp;RV-C is a CAN-based communication network for recreation vehicles. This document details physical, administration, diagnostic, and application requirements to allow all RV-C network connected devices to interoperate and be maintained.
&emsp;&emsp;The **core purpose** of the RV-C protocol is mainly to solve the following two major problems:
- **Interoperability**: to solve compatibility issues between devices from different manufacturers, so that control systems, power systems, entertainment systems, etc. can work together.
- **Standardization**: to provide manufacturers with unified design specifications, reducing development complexity and cost while improving the user experience.
&emsp;&emsp;At present, most products for RVs in Europe and the United States use this protocol.
&emsp;&emsp;Chapter 2 of the protocol mainly covers the physical layer — that is, hardware-related content. Since what we are mainly doing here is parsing the protocol, I will skip Chapter 2 directly.
&emsp;&emsp;So let us start from Chapter 3 of the protocol.

## 2. Explanation of Core Terms
### 2.1 **Node or "RV-C Node" or "CAN Node"** —— Hardware Entity
&emsp;&emsp;Original: A microprocessor that sends and receives data on an RV-C bus. More technically, a CAN 2.0B master node consisting of a transceiver, controller, and microprocessor/microcontroller to implement the RV-C protocol.
&emsp;&emsp;Roughly speaking, the original text means a CAN 2.0B master node consisting of a transceiver, controller, and MCU. Here the protocol strictly defines "Node" as **the physical hardware board**. It emphasizes the physical existence of the hardware (a transceiver, a CAN controller). In plain terms, it means a basic hardware entity; the main boundary is a hardware entity that has CAN communication capability. For example, an inverter. Of course, if this device also contains a charger in addition to the inverter function, but it shares one CAN transceiver device — even though it contains two devices, it is still one **Node or "RV-C Node" or "CAN Node"**. Since these three names will be used later, for convenience I will uniformly use **RV-C Node** to describe this term.

### 2.2 **SA (Source Address)** —— The Node's Unique ID
&emsp;&emsp;Original: A unique identifier for each node on an RV-C network. Source addresses may be hard coded by a device, or claimed dynamically on startup per section 3.3 of this specification. A node maintains a single source address regardless of the number of functions or device types it supports.
&emsp;&emsp;Roughly speaking, the original text means that **SA (Source Address)** is the unique identifier of each node on the network. **A node maintains only one unique source address (single source address)**, regardless of how many functions it supports. **SA = the IP address of the hardware board**. No matter how many functional modules are plugged into this board, or how many "virtual machines" run on it, its physical identity (Source Address) on the bus is only one. Just like the example in 2.1: if an inverter also contains a charger, but they share one CAN transceiver — that is, one **Node or "RV-C Node" or "CAN Node"** — it can only claim one **SA**. This **SA** name is used in the composition of the CAN ID later, so it is quite important.

### 2.3 **Device Type** —— The Software "Split Personality" Inside a Node
&emsp;&emsp;Original: A set of functions or capabilities possessed by a node e.g. "awning", "inverter", or "water heater". Each device type has a set of RV-C messages associated with it as defined in section 6 of this specification; e.g. a node supporting the "awning" device type might broadcast/receive RV-C messages "AWNING_COMMAND" and/or "AWNING_STATUS".
&emsp;&emsp;A node may support multiple device types or multiple instances of a device type (e.g. "awning 1" and "awning 2").
&emsp;&emsp;The main meaning of the original text is the set of functions or capabilities possessed by an **RV-C Node** (e.g. "awning", "inverter"). A node may support multiple device types or multiple instances of the same device type (e.g. `awning 1` and `awning 2`). This provides the foundation for implementing "all-in-one" hardware (such as the earlier example of an inverter that also contains a charger). **Device Type = a logically virtual device in software**. Although there is physically only one SA, the MCU inside the board can "split personalities" and logically play multiple Device Types (for example, acting as an inverter and also as a charger).
&emsp;&emsp;The original text mentions `awning 1` and `awning 2`, which is the prototype of the protocol's **`Instance`** concept, used to distinguish different virtual devices under the same type. However, the term **`Instance`** is not explained in the glossary of the protocol; it is explained in later chapters. I will put the explanation of **`Instance`** aside for later, and once I have accumulated more knowledge I will also discuss the various examples of **`Instance`** together.

### 2.4 **DSA (Default Source Address)** —— The "Temporary Badge Number" for Diagnostics
&emsp;&emsp;Original: An identifier associated with each device type as defined in Table 7.2. The DSA is included when a node broadcasts its diagnostic message (DM_RV); a node supporting multiple device types broadcasts the DM_RV for each of its device types with the correct DSA in each DM_RV message.
&emsp;&emsp;The original text mentions that **DSA** is the identifier associated with each Device Type, specifically defined in Table 7.2. It must be carried when broadcasting the `DM_RV` diagnostic message. A node supporting multiple device types must send the corresponding DM_RV for each device type (the DM_RV mentioned here is a fairly important packet of the RV-C protocol and will be parsed and explained in detail later). **DSA** mainly solves the problem of **diagnostic confusion** when one **RV-C Node** contains multiple **Device Types**.

> #### **Since one **RV-C Node** can only have one **SA**, when a single **RV-C Node** has both `Device Type A` and `Device Type B` reporting faults at the same time, how do we tell which one is reporting?**
> This is exactly why **DSA** exists. When sending `DM_RV`, the protocol fills the message with the DSA corresponding to the specific Device Type, so it becomes clear whether it is `Device Type A` or `Device Type B` inside the **RV-C Node** that is reporting the fault.
> #### Then a new problem arises: since one **RV-C Node** can only have one **SA**, when a single **RV-C Node** has both `Device Type A1` and `Device Type A2` reporting faults at the same time, the DSA would also be the same, wouldn't it? So how do we tell which one is reporting?**
>
> This is where the **Bank Select** in the DM_RV comes in. There is very little explanation about **Bank** throughout the whole protocol. In my personal opinion, when the **SA** is the same and the **DSA** is also the same, it becomes necessary to use BANK to distinguish the **Device Type**. I will explain this in detail later when parsing the DM_RV.

## 3 The Raw 29-bit CAN ID Structure of the RV-C **Message on the Bus** and the **Data Field**
&emsp;&emsp;The original table of the CAN message is as follows
| DLL fields | | RV-C name (abbreviation) | Description |
| ----------- | --------- | ------------------------ | ------------------------------------------------------------ |
| Name | Bit | | |
| Base ID | 28 to 26 | Priority | 111b – Lowest priority 000b – Highest priority |
| Base ID | 25 | Reserved | Always 0b |
| Base ID | 24 to 18 | Data Group Number - High | Identifies how the data packet should be parsed, possibly in combination with the DGN-Low |
| Extended ID | 17 and 16 | Data Group Number - High | Identifies how the data packet should be parsed, possibly in combination with the DGN-Low |
| Extended ID | 15 to 8 | Data Group Number - Low | Either determines the target node for the message, or with the DGN-High determines how the data packet should be parsed. |
| Extended ID | 7 to 0 | Source address (SA) | Shall be unique for each node |
| Data | 64 to 0 | Data | Defined in detail in the RV-C application profile specification |
------

### **3.1 The Underlying Identity of a Message: The Three Elements of the 29-bit CAN ID**
&emsp;&emsp;RV-C is designed based on the CAN 2.0b extended frame protocol, and its 29-bit ID (`Base ID` + `Extended ID`) is strictly divided into three parts, each with its own responsibility:
#### **3.1.1 Priority —— Determines Who Sends First**
- **Position**: Bit 28 ~ 26 (3 bits)
- **Function**: This is the bus's "**VIP lane**". When multiple devices send messages on the bus at the same time, the smaller the number, the higher the priority (`000b` highest, `111b` lowest).
- **Practical application**: For example, the default priority of `DM_RV` (diagnostic alarm) is `6` (`110b`), while the priority of a regular status frame may be `7`. This means that when an inverter reports a fault, even if the bus is congested, the alarm information can still squeeze to the front and be sent first, preventing it from being drowned out by normal data.
#### **3.1.2 Data Group Number (DGN) —— Determines "What the Content Is"**
- **Position**: Bit 24 ~ 8 (17 bits), combined from DGN-High (24-18) and DGN-Low (15-8).
- **Function**: This is equivalent to marking the "**content type**" of the message.
- **Practical application**: All DGN content definitions of the protocol are in Table 7.7. For example, the meaning of DGN=1FFFF in the table is DATE_TIME_STATUS; when the parsing side sees it, it knows this is DATE_TIME_STATUS data.
#### **3.1.3 Source Address (SA) —— Determines "Who I Am"**
- **Position**: Bit 7 ~ 0 (8 bits)
- **Function**: This is the "**ID card number**" of the node. The protocol requires that the SA of each node must be unique.
- **The fallback mechanism for bus arbitration**: The protocol specifically emphasizes: "when priorities are equal, SA acts as the final tiebreaker in arbitration." The arbitration of SA here is also important content in RV-C and will be explained in detail later.
------
### **3.2 The Actual Payload of a Message: The Data Field**
- **Position**: `Data`, 64 to 0 bits in total (i.e., the 8-byte data of a standard CAN frame).
- **Function**: It carries the actual data packet; the meaning of the data for each DGN is explained in the protocol.

------
# **Summary**
&emsp;&emsp;OK, that is about all I want to write for the preface. In the next chapter I will focus on **instance** — it is actually very important, yet the protocol does not say much about it.
&emsp;&emsp;**All of the above is personal study notes; if there are any errors, your corrections are welcome.**
&emsp;&emsp;**Exploration is not just about results; the process matters too. Knowledge is wealth. Accumulate a little each time, and eventually there will be a qualitative leap.**
&emsp;&emsp;**Personal studio: https://site.demmo.top/**
&emsp;&emsp;**Contact [wlc@demmo.top](mailto:wlc@demmo.top), [wlcwjy@163.com](mailto:wlcwjy@163.com), your guidance is welcome.**
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;**Ghost**