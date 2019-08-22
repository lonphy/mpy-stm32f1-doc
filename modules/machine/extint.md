---
layout: post
title:  "machine.ExtInt 类"
date:   2019-08-18 0:0:0 +0000
image: /assets/images/twoscreen.jpg
category: machine
---

外部事件与中断(machine.ExtInt)


当前有18个中断线(USB_Weakup未实现), 其中16个来自GPIO Pin, 其他的2个来自内部

对于0-15, 一个给定的中断线可以映射到任意GPIO的对应Pin, 例如 中断线0 可以映射到 Px0(x可以是A,B,C,...)

> `ExtInt`构造会自动配置传入Pin为输入模式

```python
from machine import ExtInt,Pin

def callback(line):
    print("line =", line)

extint = ExtInt(pin, ExtInt.IRQ_FALLING, Pin.PULL_UP, callback)
```

> 尝试注册2个回调到同一中断线会抛出一个异常

如果pin参数传入一个`integer`, 将会被认为是一个内部中断线, 则必须是 16,17  
所有其他情况则必须传入一个`Pin`对象

有效的`mode`(触发模式)可以参考 [类常量](#class-const)

其中只有IRQ_xxx模式已经通过测试, EVT_xxx(用于快速启动ADC/DAC转换)因为与低功耗模式/WFE指令有关, 没测试

有效的`pull`可以是`Pin.PULL_UP`, `Pin.PULL_DOWN`, `None`

#### 类方法

###### `ExtInt.regs()`{:class="method"}
    
打印6个寄存器的值


#### 类常量 {#class-const}

- `ExtInt.IRQ_RISING`{:class="const"} - 中断, 上升沿触发
- `ExtInt.IRQ_FALLING`{:class="const"} - 中断, 下降沿触发
- `ExtInt.IRQ_RISING_FALLING`{:class="const"} - 中断, 上升+下降沿触发
- `ExtInt.EVT_RISING`{:class="const"} - 事件, 上升沿触发
- `ExtInt.EVT_FALLING`{:class="const"} - 事件, 下降沿触发
- `ExtInt.EVT_RISING_FALLING`{:class="const"} - 事件, 上升+下降沿触发



#### 构造方法

###### `ExtInt(pin, mode, pull, callback)`{:class="method"}
- `pin` 使能中断的引脚（可以是引脚对象或任何有效的引脚名称）
- `mode`
    - ExtInt.IRQ_XXX 中断模式
    - ExtInt.EVT_XXX 事件模式
- `pull`
    - None 无上拉或下拉电阻
    - `Pin.PULL_UP`{:class="const"} - 使能上拉电阻
    - `Pin.PULL_DOWN`{:class="const"} - 使能下拉电阻

- `callback` 中断触发时调用的函数。回调函数必须接受1个参数(触发中断/事件的中断线)
    > 不能在同一个中断线定义不同的回调函数, 若需要则需要传入None清理一次

> 使用构造函数, 会自动使传入引脚被配置为输入模式, 根据pull不同值被配置为浮空/上拉/下拉

```python
from machine import ExtInt,Pin

def callback(line):
    print("%s pressed"%(line))

k1 = ExtInt(Pin.cpu.A1, ExtInt.IRQ_FALLING, Pin.PULL_UP, callback)
```

#### 实例方法

###### `ExtInt.line()`{:class="method"}
    
返回引脚所映射的中断线(0~17)

###### `ExtInt.enable()`{:class="method"}
    
启用中断线

###### `ExtInt.disable()`{:class="method"}
    
禁用中断线

###### `ExtInt.swint()`{:class="method"}
    
软件触发中断线
