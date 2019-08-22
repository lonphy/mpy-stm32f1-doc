---
layout: post
title:  "pyb.LED 类"
date:   2019-08-18 0:0:0 +0000
image: /assets/images/twoscreen.jpg
category: pyb
---


`pyb.LED`类 用于驱动单个LED(发光二极管)


#### 构造方法

###### `pyb.Led(id)`{:class="method"}

说明: 创建一个LED对象, `id` 范围[1~4]

> 受板子定义的LED数量限制


#### 实例方法

###### `pyb.LED.intensity([value])`{:class="method"}

获取/设置 LED亮度。 亮度范围[0(熄灭), 255(最大亮度)]

如果未提供`value`, 则返回当前亮度

> 这里需要板子上的LED接在某Timer的输出通道管脚上, 同时修改`led.c`使用的Timer, 否则无效

> 默认 LED(3) 使用 Timer2, LED(4)使用Timer3

###### `pyb.LED.off()`{:class="method"}
     
熄灭LED

###### `pyb.LED.on()`{:class="method"}

点亮LED, 达到最大亮度


###### `pyb.LED.toggle()`{:class="method"}

在点亮(最大亮度)与熄灭之前切换, 如果LED是非熄灭状态, 则认为是打开, 会切换到熄灭
