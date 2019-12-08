---
layout: post
title:  "machine.ADC 类"
date:   2019-08-18 0:0:0 +0000
image: /assets/images/twoscreen.jpg
category: machine
---

`machine.ADC` – 模数转换, 从pin上读取模拟值

使用示例:
```py
from machine import ADC,ADCAll

# STM32F103xx 最大分辨率12bit
resolution = 12

adc = ADC(pin)                   # 从Pin创建一个ADC对象
val = adc.read()                 # 读取模拟量

adc = ADCAll(resolution)         # 创建 ADCAll 对象
adc = ADCAll(resolution, mask)   # 使用选定模拟通道创建一个ADCAll对象
val = adc.read_channel(channel)  # 读取给定通道
val = adc.read_core_temp()       # 读取MCU温度
val = adc.read_core_vbat()       # 读取MCU电池电压
val = adc.read_core_vref()       # 读取MCU参考电压
val = adc.read_vref()            # 读取MCU供电电压
```

#### 构造方法

###### `machine.ADC(pin)`{:class="method"}

使用给定的Pin创建ADC对象


#### 实例方法

###### `machine.ADC.read()`{:class="method"}

读取模拟值，并返回

返回值区间是[0,4095]

###### `machine.ADC.read_timed(buf, timer)`{:class="method"}

按`timer`给定的采样率， 读取模拟值到`buf`中。

- `buf` 可以是 `bytearray` 或 `array.array`等. 
> 因为`ADC`的值精度是12-bit, 所以对于`buf`的元素类型大小
    - &gt;=16bit,则直接存储
    - = 8bit, (例如bytearray)则精度缩减到8bit，然后存储

- `timer` 是一个`Timer`对象, 每次定时器触发都会进行采样。所以Timer必须是已经初始化的，并且以所需的采样频率运行。


为了支持此函数的预定行为，`timer`也可以是一个整数，指定采样的频率(Hz)。在这种情况下`Timer(6)`将以给定的频率运行。

使用定时器的例子(推荐):

```py
from machine import ADC,Timer,Pin
adc = ADC(Pin.board.PA1)   # 使用PA1创建一个ADC对象
tim = Timer(6, freq=10)    # 创建timer(6), 以10Hz运行
buf = bytearray(100)       # 创建采样值缓冲
adc.read_timed(buf, tim)   # 花费10秒, 采样100个值
```

使用整数的例子:

```py
from machine import ADC,Pin
adc = ADC(Pin.board.PA1)
buf = bytearray(100)
adc.read_timed(buf, 10)  # 注意这里, 第二个参数传10， 表示以10Hz的频率采样
                            # 大概10s后采样结束
for val in buf:          # 打印所有采样到的值
    print(val)
```

> 本函数不会分配任何堆内存

###### `machine.ADC.read_timed_multi((adcx, adcy, ...), (bufx, bufy, ...), timer)`{:class="method"}

> 这是一个类方法

它将模拟值从多个ADC以`timer`设置的速率读入缓冲区。每次定时器触发时，都会依次从每个ADC快速读取一个样本。

`adcx`和`bufx`以`tuple`的形式传入，每个ADC都有一个相关的buf。所有buf的类型和长度必须相同，buf的数量必须等于ADC的数量。


`ADC值`的分辨率为_12-bit_，如果bufn的元素大小为16-bit或更大，则直接存储到相应的缓冲区中，否则样本的分辨率将降低到8-bit。

批量读取3个ADC示例:
```python
from machine import ADC,Pin,Timer
import array

adc0 = ADC(Pin.board.PA1)
adc1 = ADC(Pin.board.PA2)
adc2 = ADC(Pin.board.PA3)
tim = Timer(8, freq=100)
rx0 = array.array('H', (0 for i in range(100))) # 创建 100个 16-bit元素的缓冲
rx1 = array.array('H', (0 for i in range(100)))
rx2 = array.array('H', (0 for i in range(100)))

ADC.read_timed_multi((adc0, adc1, adc2), (rx0, rx1, rx2), tim)
for n in range(len(rx0)):
    print(rx0[n], rx1[n], rx2[n])
```

如果在正确的时间内获得了所有样本，则函数返回True。
在高采样率的情况下，获取一组样本所需的时间可能超过定时器周期。在这种情况下，函数返回False，表示在样本区间内精度的损失。
在极端情况下，可能会漏掉样本。

> 本函数不会分配任何堆内存, 另外函数是阻塞执行的， 意味着缓冲区填满以前不会返回


###### `machine.ADC.deinit()`{:class="method"}
释放ADC对象

<br>

## `machine.ADCAll`

> 这里有些问题， 只是翻译了一下， STM32F103xx 与F4等有区别， 源码还未调整

实例化它会将所有带掩码的ADC引脚更改为模拟输入。
预处理后的MCU温度、VREF和VBAT数据分别可以通过ADC通道16、17和18访问。

根据使用的参考电压(通常为3.3V)进行适当的缩放。

芯片上的温度传感器是出厂校准的，允许读取模具温度到+/- 1摄氏度。虽然这听起来相当精确，但MCU的内部温度是测量出来的。

根据MCU负载和I/O子系统的工作情况，芯片温度很随意的就比环境温度高出几十度。

`ADCAll` 的 `read_core_vbat()`, `read_vref()` 以及 `read_core_vref()` 方法使用供电电压作为参考,

读取 __备用电池电压__，__参考电压__ 等, 所有结果都是以浮点数形式给出直流电压。


`read_core_vbat()` - 返回备用电池的电压。此电压也根据实际供电电压进行调整。  
为了避免模拟输入过载，电池电压通过分压器测量，并根据分压器的值进行缩放。为了防止备用电池负载过大，分压器只在ADC转换时激活。

`read_vref()`通过测量内部电压基准进行测量，并使用内部电压基准的工厂校准值对其进行回调。

在大多数情况下，读数接近3.3V。如果板子由电池供电，则供电电压可能降至3.3V以下。

只要满足操作条件，板子仍然可以正常运行。

通过适当的MCU时钟、flash访问速度和编程模式的设置，可以将pyboard的电压降至2v，并且仍然可以获得有用的ADC转换。

> 确保模拟输入电压不超过实际供电电压非常重要

其他模拟输入通道(0..15)将根据所选精度返回未缩放的整数值。

为了避免不必要的模拟输入开启(通道0..15)，可以指定第二个参数。

这个参数是一个二进制模式，其中每个请求的模拟输入都有相应的位。

默认值是`0xffffffff`，这意味着所有模拟输入都是活动的。

如果只需要内部通道(16..18)，掩码值应该是`0x70000`。

例如:
```python
adcall = ADCAll(12, 0x70000) # 12 bit resolution, internal channels
temp = adcall.read_core_temp()
```
