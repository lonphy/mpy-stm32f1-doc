---
layout: post
title:  "MPY ST103快速上手"
date:   2019-12-07 0:0:0 +0000
image: /assets/images/twoscreen.jpg
category: pyb
---

简介
---

- MCU 
    - STM32F103ZGT6 ARM Cortex M3 / 72MHz, <b class='r'>1024KB</b> Flash, 96KB SRAM
- 存储
    - 外扩 128Mbit SPI Flash : __W25Q128__
    - 外扩 1M Byte SRAM: __IS62WV51216__
    - 外扩 128Mbit NorFlash: __S29GL128P10/S29GL128S90__
    - MicroSD 9P卡座(SDIO-4bit)
- 接口
    - LCD液晶屏接口(FSMC 8/16bit)
    - OLED(I2C)接口
    - SWD 3Pin 调试接口
    - 两路 __DAC__ 输出
    - 4路 __PWM__ 输出
    - ESP8266-01 / ESP8266-01S 插座
    - MicroUSB 母座 * 2 (一路USB2.0全速, 一路USB转串口)
- 其他
    - RTC 电池座(CR1220)
    - 无源蜂鸣器(PWM/GPIO驱动)
    - 5个高亮LED(4色)支持PWM控制, 一个电源指示灯(常亮)
    - USB转串口CH340C(USART1)
    - 其他未用到GPIO全量引出
    - 1个唤醒按键 + 1个复位按键

图例
---
<img width="1000" src="mpy-st103.png">



第一次启动
---

> 出厂自带最新版固件

所需工具:

- 带USB接口 电脑/笔记本 一台(TypeC接口 需转接器)
- MicroUSB 线一根

开机步骤:
1. 第一步
    - 确保使用跳线帽按上图位置， 选择 __启动模式__
2. 第二步
    - 使用USB线连接 __MPY-ST103__ 与 电脑 ( __D5__ 点亮表示供电正常), 确保连接良好
3. 第三步
    - __Windows__: 
        - 会提示安装驱动, 打开 "计算机"(win10), 在显示的"移动硬盘"中提供了驱动程序， 手动安装即可(设备管理器-属性-更新驱动-在选择)
        - 使用串口工具, 打开COM(n), 波特率选择115200

    - __MAC__:
        - 打开终端 
        ```sh
            # 查看串口设备名
            $ ls /dev/tty.*
            /dev/tty.Bluetooth-Incoming-Port /dev/tty.STW-9030-SerialPort     /dev/tty.usbmodem43EF367936362
            
            # 使用自带工具screen 打开串口
            $ screen /dev/tty.usbmodem43EF367936362 115200

            # 显示如下，表示连接成功
            MicroPython v1.11-523-gfdccf3bff-dirty on 2019-12-07; MPY ST103 with STM32F103ZG
            Type "help()" for more information.
            >>>
        ```
    - __Linux__:
        - 打开Bash
        ```bash
            # 若已安装，则跳过
            $ sudo apt-get install picocom

            # 确保已经识别
            $ ls /dev/ttyACM0

            # 连接串口, 退出使用Ctrl+z 然后按Ctrl+q
            $ sudo picocom -b115200 /dev/ttyACM0
        ```
4. 第四步
    ```py
    # 导入LED类
    from pyb import LED
    led1 = LED(1)
    led1.on()   # 点亮LED
    led1.off()  # 熄灭LED
    ```
    