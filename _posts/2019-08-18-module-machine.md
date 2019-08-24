---
layout: post
title:  "machine 模块"
date:   2019-08-18 0:0:0 +0000
image: /assets/images/machine.jpg
---

machine 模块提供GPIO、管脚映射、外设类、MCU频率设置、查看系统信息等功能

#### 模块方法

###### `machine.info()`{:class="method"}

打印MCU信息

```
MCU unique_id
运行频率
段(代码段,堆,栈)起始/结束地址, 内存起始/结束地址
qstr信息
gc 信息
vfs 信息
```

###### `machine.unique_id()`{:class="method"}

返回MCU的96bit(12Byte)唯一标识`bytes`

###### `machine.freq(sysclk, hclk, pclk1, pclk2)`{:class="method"}

返回或设置MCU运行频率
> 在stm32f103中, 返回(sysclk, hclk, pclk1, pclk2)

设置时如果只提供sysclk, 则:
- hclk=sysclk
- pclk1=sysclk/2
- pclk2=sysclk

###### `machine.reset()`{:class="method"}

类似reset管脚触发的方式复位MCU

###### `machine.soft_reset()`{:class="method"}

软件复位MCU

###### `machine.reset_cause()`{:class="method"}

获取MCU复位原因, 返回 [machine.XXX_RESET](#const_reset)

###### `machine.bootloader()`{:class="method"}

__TODO__

###### `machine.idle()`{:class="method"}

MCU进入空闲模式， 任何中断或事件就可唤醒

###### `machine.lightsleep([timeout])`{:class="method"}

MCU进入低功耗stop模式, timeout是RTC唤醒超时时间, 单位ms, 实际处理成秒

###### `machine.deepsleep([timeout])`{:class="method"}

MCU进入低功耗standby模式, timeout是RTC唤醒超时时间, 单位ms, 实际处理成秒

###### `machine.enable_irq(state)`{:class="method"}

重新启用中断请求, state值应该是前一次调用`disable_irq()`的返回值

###### `machine.disable_irq()`{:class="method"}

禁用中断, 并返回禁用前的中断状态, 用于调用`enable_irq(state)`的入参， 恢复禁用前的中断状态

###### `machine.time_pulse_us()`{:class="method"}

__TODO__

#### 模块常量 {#const_reset}

- `machine.PWRON_RESET`{:class="const"} - 上电复位
- `machine.HARD_RESET`{:class="const"} - 硬件复位
- `machine.WDT_RESET`{:class="const"} - 独立看门狗复位
- `machine.DEEPSLEEP_RESET`{:class="const"} - 深度睡眠唤醒复位
- `machine.SOFT_RESET`{:class="const"} - 软件复位


#### 模块包含的类

- [ExtInt]({% link modules/machine/extint.md %}) - 外部中断及事件

- [I2C]({% link modules/machine/i2c.md %}) - I2C通讯总线

- [PinRemap]({% link modules/machine/pin_remap.md %}) - 管脚重映射

- [Pin]({% link modules/machine/pin.md %}) - 管脚

- [RTC]({% link modules/machine/rtc.md %}) - 实时时钟

- [UART]({% link modules/machine/uart.md %}) - 串行通讯收发器

- [WDT]({% link modules/machine/wdt.md %}) - 独立看门狗


> 以下模块未完全测试验证

- [ADC]({% link modules/machine/adc.md %}) - 模数转换

- [CAN]({% link modules/machine/can.md %}) - 控制器局域网

- [DAC]({% link modules/machine/dac.md %}) - 数模转换

- [Signal]({% link modules/machine/signal.md %}) -

- [SPI]({% link modules/machine/spi.md %}) - SPI通讯总线

- [Timer]({% link modules/machine/timer.md %}) - 硬件定时器

- `mem8` -- <8-bit memory>
- `mem16` -- <16-bit memory>
- `mem32` -- <32-bit memory>
