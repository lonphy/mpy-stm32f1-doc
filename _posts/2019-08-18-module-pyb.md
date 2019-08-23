---
layout: post
title:  "pyb 模块"
date:   2019-08-18 0:0:0 +0000
image: /assets/images/pyb.jpg
---

pyb包提供一些特定外部或内部模块的驱动

#### 模块方法

###### `pyb.fault_debug()`{:class="method"}

启用/禁用硬件故障调试。

硬故障是指底层系统中存在致命错误，例如无效的内存访问。 参数值如下:
- `False` - 当发生致命错误时， 板子(MCU)将自动复位(默认值)
- `True` - 当发生致命错误时, 将打印寄存器和栈跟踪信息， 然后无限循环闪烁LED
> 打印信息包含 栈: R0, R1, R2, R3, R12, LR, PC, XPSR

###### `pyb.repl_info(level)`{:class="method"}

开启/关闭repl运行信息输出
- `0` - 关闭
- `1` - 打开
> 打开后， 可在每次repl执行后， 输出以下信息:

```
    执行耗时(ms)
    qstr 信息
    GC 信息(如果已开启)
```

###### `pyb.irq_stats()`{:class="method"}

返回每个中断被调用的计数信息

> 若编译时打开 宏 `IRQ_ENABLE_STATS`, 则该函数可用

###### `pyb.country([code])`{:class="method"}

设置/获取 国家代码2字节
> 用于设置wifi模块(cyw43)国别码(暂时无用)

###### `pyb.elapsed_millis(start)`{:class="method"}

返回自`start`(单位ms)以来, 到调用时刻, 经过的毫秒数
> 函数会强制返回正数(有效位30bit, 意味着能正确表示最多12.4天)

###### `pyb.elapsed_micros()`{:class="method"}

返回自`start`(单位us)以来, 到调用时刻, 经过的微秒数
> 函数会强制返回正数(有效位30bit, 意味着能正确表示最多17.8分钟)

###### `pyb.main()`{:class="method"}

设置主脚本的文件名, 以便在`boot.py`执行完后执行, 如果未调用本函数，则默认的`main.py`将被执行

仅在`boot.py`中调用才有意义

###### `pyb.dht_readinto()`{:class="method"}

DHT单线驱动

###### `pyb.pwm_set(period, pulse)`{:class="method"}

__TODO__, Servo模块启用时有效

###### `pyb.servo_set(port, value)`{:class="method"}

__TODO__, Servo模块启用时有效

#### 模块包含的类

- [Accel]({% link modules/pyb/accel.md %}) - MMA7660加速传感器

- [LED]({% link modules/pyb/led.md %}) - LED

- [Servo]({% link modules/pyb/servo.md %}) - 私服电机驱动

- [Switch]({% link modules/pyb/switch.md %}) - 按键开关

- [USB_VCP]({% link modules/pyb/usb_vcp.md %}) - USB虚拟串口

- [USB_HID]({% link modules/pyb/usb_hid.md %}) - USB HID设备

<br>
