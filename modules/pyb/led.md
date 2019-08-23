---
layout: post
title:  "pyb.LED 类"
date:   2019-08-18 0:0:0 +0000
image: /assets/images/twoscreen.jpg
category: pyb
---


`pyb.LED`类 用于驱动单个LED(发光二极管)

综合示例:

```py
import pyb
import time

led1 = pyb.LED(1)

led1.on()
led1.off()
for x in range(10):
    time.sleep(100)
    led1.toggle()

```
#### 构造方法

###### `pyb.Led(id)`{:class="method"}

说明: 创建一个LED对象, `id` 范围[1~4]

> 受板子定义的LED数量限制


#### 实例方法

###### `pyb.LED.off()`{:class="method"}

熄灭LED

###### `pyb.LED.on()`{:class="method"}

点亮LED, 达到最大亮度


###### `pyb.LED.toggle()`{:class="method"}

在点亮(最大亮度)与熄灭之前切换, 如果LED是非熄灭状态, 则认为是打开, 会切换到熄灭


###### `pyb.LED.intensity([value])`{:class="method"}

获取/设置 LED亮度。 亮度范围[0(熄灭), 255(最大亮度)]

如果未提供`value`, 则返回当前亮度

> 这里需要板子上的LED接在`Timer`[1,3]的输出通道管脚上, 否则无效

> 如果LED在支持PWM的TIMx的CHn上，则通过开发板定义文件指定`MICROPY_HW_LED{x}_PWM`宏来开启PWM特性

在文件(`boards/{开发板名称}/mpconfigboard.h`)中， 示例如下:
```c
// led1 接在PA1上
#define MICROPY_HW_LED1             (pin_A1)

/**
    PA1 是 TIM2_CH2的输出管脚(PWM), 则这样定义 {TIMx, N, TIM_CHANNEL_2, 0}
    TIMx - 定义使用TIM几
    N - 标识TIM的id, TIM1=1， TIM2=2, 依次类推
    TIM_CHANNEL_2 - 指定使用第二通道
    最后一个参数填0即可
*/
#define MICROPY_HW_LED1_PWM         { TIM2, 2, TIM_CHANNEL_2, 0 }

// 如果你的LED是低电平点亮，则需要翻转极性，否则指定的亮度值与实际亮度是相反的
#define MICROPY_HW_LED_INVERTED     (1)

```
> 当然也可以直接用`machine.Timer.channel`实现:

```py
from machine import Pin, Timer

t2 = Timer(2)
t2.init(freq=1000000, mode=Timer.UP, div=1)
pwmLed = t2.channel(2, Timer.PWM_INVERTED, pin=Pin.cpu.A1, pulse_width=0)

# LED亮度设为100%
pwmLed.pulse_width_percent(100)

# LED亮度设为50%
pwmLed.pulse_width_percent(50)

```