---
layout: post
title:  "pyb.Servo 类"
date:   2019-08-18 0:0:0 +0000
image: /assets/images/twoscreen.jpg
category: pyb
---

`pyb.Servo`类 用于驱动 hobby-3线伺服电机


`Servo` 可控制标准hobby 3线(VDD,GND,1根信号线)伺服电机. 

示例:
```python
import pyb

s1 = pyb.Servo(1)
s2 = pyb.Servo(2)

s1.angle(45) # 电机1 转到 45 deg
s2.angle(0) # 电机2 转到 0 deg

s1.angle(-60, 1500) # 电机1 使用1.5s 转到-60 deg
s2.angle(30, 1500)  # 电机2 使用1.5s 转到30 deg
```

> `Servo`使用`Timer(5)` 输出PWM. 当不用于`Servo`时，可做他用

#### 构造方法

###### `pyb.Switch(id)`{:class="method"}

说明: 创建一个servo对象, `id` 范围1~4, 对应`TIM5` 的 ch1 到 ch4


#### 实例方法

###### `pyb.Servo.angle([angle, time=0])`{:class="method"}

如果无参调用, 则返回当前角度.

否则设置电机角度:

- `angle` 是需要转动的角度
- `time` 是转到给定角度需要的时间(ms), 如未指定则以最快时间转动


###### `pyb.Servo.speed([speed, time=0])`{:class="method"}
     

无参数 - 返回电机当前转速

提供参数则用于设置电机速度:

- `speed` 指定需要的目标速度, 范围[-100,100]
- `time`  指定达到给定速度需要的时间(ms), 如未指定则以最快的时间变速


###### `pyb.Servo.pulse_width([value])`{:class="method"}

设置/获取电机PWM脉宽


###### `pyb.Servo.calibration([pulse_min, pulse_max, pulse_centre[, pulse_angle_90, pulse_speed_100]])`{:class="method"}



无参数 - 返回当前校准数据 (长度=5的元组)

否则 用于设置延时校准:

- `pulse_min` 最小脉宽
- `pulse_max` 最大脉宽
- `pulse_centre` 零点/中心脉宽
- `pulse_angle_90` 90度脉宽
- `pulse_speed_100` 速度100时的脉宽
