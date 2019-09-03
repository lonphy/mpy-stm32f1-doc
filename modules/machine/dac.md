---
layout: post
title:  "machine.DAC 类"
date:   2019-08-18 0:0:0 +0000
image: /assets/images/twoscreen.jpg
category: machine
---

`machine.DAC` - 数模转换器

`DAC`用于在`PA4`,`PA5`输出模拟量值(特定电压值), 电压范围[0,3.3V]

示例:
```py
from machine import DAC

dac = DAC(1)   # 在PA4创建 DAC1
dac.write(128) # 输出模拟量(电压值, 1.65V)
```

输出连续的正弦波:
```py
import math
from machine import DAC

# 创建一个保存正弦波的缓冲
buf = bytearray(100)
for i in range(len(buf)):
buf[i] = 128 + int(127 * math.sin(2 * math.pi * i / len(buf)))

# 以400Hz频率输出正弦波
dac = DAC(1)
dac.write_timed(buf, 400 * len(buf), mode=DAC.CIRCULAR)
```

输出连续的12-bit分辨率正弦波:
```py
import math
from array import array
from machine import DAC

buf = array('H', 2048 + int(2047 * math.sin(2 * math.pi * i / 128)) for i in range(128))

# 以400Hz频率输出正弦波
dac = DAC(1, bits=12)
dac.write_timed(buf, 400 * len(buf), mode=DAC.CIRCULAR)
```


#### 类常量

- `DAC.NORMAL`{:class="const"} - 正常模式

- `UART.NORMAL`{:class="const"} - 循环模式

#### 构造方法

###### `machine.DAC(port, bits=8, *, buffering=None)`{:class="method"}

创建`DAC`对象, 支持`DAC1`(`PA4`) 或 `DAC2`(`PA5`)

- `port` 是一个`Pin`对象, 或`数字`(`1` 或 `2`)

- `bits` 分辨率, `8` 或 `12`, `DAC.write` 和 `DAC.write_timed` 的最大输入值 <b>2<sup>bits</sup> - 1</b>

- `buffering` 选择`DAC`运放输出行为, 控制输出阻抗, 可取以下值:
    - `None` 默认 (`DAC.noise()`, `DAC.triangle()` and `DAC.write_timed()` 开启, `DAC.write()`关闭), 
    
    - `False` 禁用缓冲
    
    - `True` 开启缓冲

> 开启时, `DAC`输出管脚可驱动低至5KΩ的负载  
> 否则输出阻抗`最大15KΩ`: 因此获得1%精确度，无缓冲情况, 负载必须小于1.5MΩ  
> 使用缓冲会导致精度下降, 尤其在极值附近时


#### 实例方法

###### `machine.DAC.init(bits=8, *, buffering=None)`{:class="method"}

初始化`DAC`外设, 具体参数参考构造函数


###### `machine.DAC.deinit()`{:class="method"}

关闭`DAC`外设


###### `machine.DAC.noise(freq)`{:class="method"}

以给定频率`freq`生成随机噪声信号

###### `machine.DAC.triangle(freq)`{:class="method"}

以给定频率生成三角波


###### `machine.DAC.write_timed(data, freq, *, mode=DAC.NORMAL)`{:class="method"}

启动`RAM`到`DAC`的突发数据传输(使用`DMA`)

- `data` 字节数组(8 bit数据)

- `freq` 输出频率
    - 数字 指定`DAC`的输出频率(固定使用`TIM6`)

    - 已配置的`Timer`对象, 用于触发`DAC`采样, 有效的`Timer`ID可以是 (`2`, `4`, `5`, `6`, `7`, `8`)

- `mode` 模式, 可选择 `DAC.NORMAL` 或 `DAC.CIRCULAR`.

同时使用2个`DAC`:
```py
dac1 = DAC(1)
dac2 = DAC(2)
dac1.write_timed(buf1, pyb.Timer(6, freq=100), mode=DAC.CIRCULAR)
dac2.write_timed(buf2, pyb.Timer(7, freq=200), mode=DAC.CIRCULAR)
```

###### `machine.DAC.write(value)`{:class="method"}

直接访问`DAC`输出

> `DAC`在硬件实现固定输出12位, 底层实现为左对齐方式(`value << (12 - bits)`)
