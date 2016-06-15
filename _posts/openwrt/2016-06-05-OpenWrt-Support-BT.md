---
layout: post
title: openwrt挂载USB蓝牙dongle
category: openwrt
tags: OpenWrt BT
keywords: openwrt bluetooth usb dongle csr4.0
description: 
---

##### 一、前言
    背景：希望路由器能够支持蓝牙。
    资源：openWRT路由器（Kernal:3.10)
          Bluetooth Dongle: CSR4.0
    
##### 二、让openWRT支持蓝牙
    1、make menuconfig打开相关选项
        - Kernel modules
            - Other modules
                - kmod-bluetooth
            - USB Support
                - kmod-usb-core
                - kmod-usb-ohci
                - kmod-usb-storage
                - kmod-usb-usb2
        
        - Ulities
            - bluez-utils
            - blues-hcidump (O)
            - bluelog (O)
            
        - Libraries
            - bluez-libs
            - usb-
            
    2、打开上述选项后编译，然后升级路由器软件。
    3、将BT dongle连接到路由器,可用lsusb命令辅助查看，当路由器正常识别到适配器后，进行以下步骤。
    4、搜索蓝牙设备
    ```bash
    hciconfig hci0 up #打开蓝牙
    hciconfig hci0 piscan #蓝牙可见
    hciconfig hci0 noscan #蓝牙不可见
    
    hcitool scan #搜索附近的蓝牙设备
    ```
    5、配对一个蓝牙设备
    openWRT默认的配置需要先修改后，才能完成配对/连接。
    5.1、/etc/bluetooth
   
    -1、hcid.conf
    # HCI daemon configuration file.
    # HCId options
    options {
        autoinit yes;
        security auto;
        pairing multi;
        passkey "0000";
        }
        
    # Default settings for HCI devices
    device {          
        name "%h";
        class 0x000100;
        lm accept,master; 
        lp rswitch,hold,sniff,park;
        }

    -2、rfcomm.conf
    
    ``bash
    # RFCOMM configuration file.
    rfcomm0 {
        bind yes;
        device F0:DB:E2:93:40:A3; #需要连接的蓝牙设备
        channel 1;
        comment "gaojing test";
        }
    ```
    
    5.2、/var/lib/bluetooth/xx:xx:xx:xx:xx:xx
    新增pincodes文件，文件内容如下
    F0:DB:E2:93:40:A3 0000
    
    5.3 配对/连接 （1代表chennel）
        rfcomm bind /dev/rfcomm0 F0:DB:E2:93:40:A3 1
        rfcomm connect /dev/rfcomm0 1 F0:DB:E2:93:40:A3
    
    
##### 三、hci相关命令
    3.1 hciconfig
        – Gives info about the bluetooth hci on your pc
        – Ensure the device is up and running and has required scan modes
        – hcitool dev should also give some of this info

    3.2 hcitool inq and hcitool scan
        – Gives info about or rather identifies nearby bluetooth devices

    3.3 hcitool info <BTAddr>
        – Get info about remote bluetooth device

    3.4 l2ping <BTAddr>
        – One way to see if we can communicate with a remote bluetooth device

    3.5 sdptool browse <BTAddr> or sdptool records   <BTAddr>
        – Gives info about the services provided by a remote bluetooth device

    3.6 obexftp –nopath –noconn –uuid none –bluetooth <BTAddr> –channel <OPUSHChannelNo> –put <FileToPut>
        – Allows one to send file without specifying the pin on the remote device side
        – The OPush channel number for device is got from sdptool above

    3.7 passkey-agent –default <Pin>
        – Pin specified here is what the remote BT device should provide or its user enter on that device when requested.

    3.8 obexftp -b <BTAddr> -v -p <FileToPut>
        – Allows one to put a file onto the specified BT device
        – obexftp could also be used to get or list the files on the BT device
        – also allows one to identify a nearby BT device by just giving -b option

    3.9 obexpushd
        – Allows one to recieve files sent from a bluetooth device.
        – Depending on who started it, the recieved files will be stored in the corresponding home directory
        
    3.10 添加通道
        - sdptool add --channel=1 DID SP DUN LAN FAX OPUSH FTP HS HF SAP NAP GN PANU HID CIP CTP A2SRC A2SNK SYNCML NOKID PCSUITE SR1
        后面的参数不一定被支持，但是以防有些服务没有被打开，所以，干脆全部打开了。
        
    3.11 rfkill unblock Bluetooth rfkill list
        - Can't init device hci0: Operation not possible due to RF-kill (132)
　　
　　
