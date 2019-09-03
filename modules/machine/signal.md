---
layout: post
title:  "machine.Signal 类"
date:   2019-08-18 0:0:0 +0000
image: /assets/images/twoscreen.jpg
category: machine
---

`machine.Signal` – 逻辑信号控制类

`machine.Signal`类是`machine.Pin`的简单扩展类. 

与`machine.Pin`不同, 它只能在 `绝对` 0和1状态，一个`Signal`可以在逻辑开(高)或关(低)状态

换句话说，它为`Pin`功能曾加了逻辑反转支持

虽然看起来是一个简单的附加功能，但确实是支持多种简单数字设备所必需的，以一种可跨不同板移植的方式支持这些设备，这是`MicroPython`的主要目标之一

不管的用户使用的是高点亮LED还是低点亮LED，是常开还是常闭的继电器，你都可以开发一个外观漂亮的应用程序，它可以与每个用户一起工作，并且在应用程序的配置文件中用几行代码就可以捕捉到硬件配置的差异。

示例:
```py
from machine import Pin, Signal

# 假设在pin0上有一个高电平点亮的LED
led1_pin = Pin(0, Pin.OUT)
# ... pin1上有个低电平点亮的LED
led2_pin = Pin(1, Pin.OUT)

# 现在，要使用类Pin同时点亮它们，需要设置不同的值
led1_pin.value(1)
led2_pin.value(0)

# 而类Signal允许抽象出高低电平差异
led1 = Signal(led1_pin, invert=False)
led2 = Signal(led2_pin, invert=True)

# 现在点亮它们看起来是一样的
led1.value(1)
led2.value(1)

# 更好的是:
led1.on()
led2.on()
```

以下是何时使用`machine.Signal`与`machine.Pin`的指南:

__使用 `Signal`:__

如果你想控制一个简单的开关(包括软PWM!)设备，如led，多段显示器，继电器，蜂鸣器，或读取简单的二进制传感器，如常开/闭按钮，拉高/低，舌簧开关，湿度/火焰传感器，等等。总之，如果您有一个真正的物理设备/传感器需要GPIO访问，应该使用`Signal`

__使用 `Pin`:__

如需实现更高级别的协议或总线来与复杂的设备通信


`Pin`和`Signal`的使用界限来自上面的示例及`MicroPython`架构:

`Pin`提供最低的开销，这对bit类协议可能非常重要, 但`Signal`在`Pin`的基础上以较小的代价(这比在Python中手工实现偏差要小的多)增加了额外的灵活性。

此外`Pin`是一个低层次对象, `Signal`是对`Pin`的封装。

如果还不能分清、可以尝试一下`Signal`. `Signal`可被提供给开发者从而避免处理无聊的差异(例如高/低有效信号)
并允许其他用户分享和喜欢你的应用程序，而不会因led或继电器的接线稍微不同就让他们认为不合适而喷你

#### 构造方法

###### `machine.Signal(pin_obj, invert=False)`{:class="method"}
###### `machine.Signal(pin_arguments..., *, invert=False)`{:class="method"}

创建一个`Signal`对象, 有两种途径创建:

- 通过包装现有的`Pin`对象

- 通过将所需的`Pin`参数直接传递给`Signal`构造函数, 跳过创建`Pin`对象的过程

参数说明:

- `pin_obj` - 已存在的`Pin`对象

- `pin_arguments` - 与传递给`Pin`构造函数的参数一致

- `invert` - 如果是`True`, 信号会被反向处理(低电平有效)


#### 实例方法

###### `machine.Signal.value([x])`{:class="method"}

设置或获取信号值，这取决于是否提供参数`x`

如果不传，则该方法得到信号级别，1表示信号逻辑搞，0表示信号逻辑低

如果提供了参数，则设置信号级别。参数`x`是任何可以转换成布尔值的表达式或值。如果转换为`True`，则信号被设为逻辑高， 否则设为逻辑低

在实际引脚上的电平高低与信号级别的关系取决于配置的`invert`参数, 

对于非反转信号, 逻辑1 对应 高电平, 逻辑0对应低电平

对于反转信号, 逻辑1 对应 低电平, 逻辑0对应 高电平


###### `machine.Signal.on()`{:class="method"}

使信号有效

###### `machine.Signal.off()`{:class="method"}

使信号无效