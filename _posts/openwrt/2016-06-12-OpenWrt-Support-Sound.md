---
layout: post
title: openwrt挂载USB声卡
category: openwrt 
tags: OpenWrt 
keywords: openwrt alsa
description: 
---

##### 一、前言
    背景：让路由器能够播放音乐
    资源：openwrt路由器一枚（mtk7620, kernal 3.10）
          USB声卡一枚（型号：）
          
##### 二、编译

    2.1 make menuconfig:
        kernal module
            - Sound Support 
                - kmod-sound-core   
                - kmod-usb-audio
        Utilities 
            - alsa-utily    
            - alsa ..
        Library
            - alsa..
        CONFIG_PACKAGE_libsndfile=y

    2.2 make kernal_menuconfig:
        Device Drivers
            - Sound Card Support        
                - Advanced Linux Sound Architecture 
                - Sequencer support 
                - OSS Mixer API     
                - OSS PCM (digital audio) API 
                - Verbose procfs contents  
                - ALSA for SoC audio support            
                    - SoC Audio for Ralink SoC
                        - Build WM8960 CODEC drivers
        
            - Charater Driver
                - Ralink PCM Support
                - Raline I2S Support
                    - Audio Code Selection (Select WM8960) (codec和上面一致即可)
                    
    2.3 相关选项打开后，直接编译可能会遇到如下问题：
        sound/soc/mtk/mtk_audio_i2s.c:129:3: error: 'SNDRV_PCM_RATE_12000' undeclared here (not in a function)
        sound/soc/mtk/mtk_audio_i2s.c:129:26: error: 'SNDRV_PCM_RATE_24000' undeclared here (not in a function)
        有人说可能用这两种速率的很少所以没有宏定义，所以我暂且把这两个选项给忽略了，等明天声卡到手后试试能不能用，不能用的话再想办法修改这个问题。
        

##### 三、调试
    现在你手上应该已经有支持声卡的固件了，不要犹豫，直接给路由器更新软件吧～～。
    然后插上声卡～～
