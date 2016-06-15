---
layout: post
title: 蓝牙核心协议
category: openwrt
tags: OpenWrt BT
keywords: openwrt bt core spec 
description: 
---

##### 一、Building blocks of BT device
     BT core spec defines the technology building blocks that developes use to create the interoperable devices that make up the thriving BT ecosystem.
     - Two flavors of BT
     Each implementation has different use cases and each implementation uses a different chipset to meet essential hardware requirements.
     Dual-mode chipsets are also avaiable for applications that include both use cases.
        - BT BR/ER: version 2.0/2.1
            establishs a relatively short-range,continuous wireless connection, which makes it ideal for use cases such as streaming audio.
        - BT LE: version 4.0/4.1/4.2
            alllows for short bursts of long-range radio connection,makeing it ideal for IOT applications that don't require continuous connection but depend on long battery life.
        - Dual-Mode
            Dual-mode chipsets are available to support single devices such as smartphones or tablets that need to connect to both BR/EDR devices(such as audio headsets) and LE devices(such as wearables or retail beacons).
            
##### 二、Core System Architecture
    The system includes an RF transceiver, baseband and protocol stacks that enable devices to connect and exchange a variety of classed of data.
    BT devices exchange protocol signaling according to the BT spec.
    Core system protocols are the 
        - L2CAP(logical link control and adaptation
        - LM(link manager) protocol,protocol).
        - LC(link control) protocol,
        - RF(radio) protocol, 
        all of which are fully defined in the BT spec. 
        The lowest three(RF,LC,LM) are often grouped into a subsystem known as the BT controller.This is a common implentation that uses an optional standard interface-HCI(Host Controller Interface)-that enables two-way communication with the remainder of the BT system, called the BT host.
    
    The primary controller may be one of the following configurations, depending on use case:
    - BR/EDR controller including the radio, baseband, Link Manager and optionally HCI
    - LE controller including the LE PHY, Link Layer and optionally HCI
    - Combined BR/EDR controller and LE controller, with one BT device address shared by the combined controller
    
    The BT spec enables interoperability between systems by defining the protocol messages that are exchanged between equivalent layers. it also enables interoperability between independent BT sub-systems by defining the common interface between BT controllers and BT hosts.
    
    
![BT_Core_spec](http://i4.buimg.com/de7a767bba9e9aa9.png)
    
    - PHY （Physical Layer）
        Controls transmission/receiving of the 2.4GHz radio with BT communication channels.
        BR/EDR provides more channels with narrower bandwidth, while LT uses fewer channels but broader bandwith.
        
    - LL (Link Layer)
        Defines packet structure/channels, discovery/connection procedure and sends/receives data.
        
    - DTM (Direct Test Mode)
        Allows testers to instruct the PHY layer to transmit or receive a given sequence of packets, submitting commands to it either via the HCI or via a 2-write UART interface.
        
    - HCI (Host to Controller Interface)
        Optional standard interface between the BT controller subsystem (bottom three layers) and the BT host.
    
    - L2CAP (Logic Link Control and Adaptation Protocol Layer)
        A packet-based protocol that transmits packet to the HCI or directly to the LM (Link Manager) in a hostless system. 
        Supports higher-level protocol multiplexing, packet segmentation and reassembly, and the conveying of quality of service information to higher layers.
        
    - ATT (Attribute Protocol)
        Defines the client/server protocol for data exchange once a connection is established.
        ATT(Attributes) are grouped together into meaningful service using the GATT(Generic Attribute Profile).
        ATT is used in LE implementations and occasionally in BR/EDR implementations.
        
    - Security Manager
        Defines the protocol and behavior that manages pairing integrity, authentication and encryption between BT devices, 
        and provides a toolbox of security functions that other components use to support almost any level of security needed by diverse applications.
        
    - GATT (Generic Attribute Profile)
        https://www.bluetooth.com/specifications/gatt
        Using the ATT protocol, GATT grooups services that encapsulate the behavior of part of a device and describes a use case, roles and general behaviors based on the GATT functionality.
        Its service framework defines procedures and formats of services and their characteristics, including discovering, reading, writing, notifying and indicating characteristics, as well as configuring the broadcast of characteristics.
        GATT is uesd only in BT LE implementations.
    
    - GAP (Generic Access Profile)
        Works in conjunction with GATT in BT LE implementations to define the procedures and roles related to the discovery of BT devices and sharing information, and link management aspects of connecting to BT devices.
    
    
    
    
    
    
    
    
    
    
    
##### 三、
