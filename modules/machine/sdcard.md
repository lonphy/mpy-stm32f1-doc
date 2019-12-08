---
layout: post
title:  "machine.SDCard 类"
date:   2019-08-18 0:0:0 +0000
image: /assets/images/twoscreen.jpg
category: machine
---

`machine.SDCard` 是一个单例类。

- `SD卡`
    是常见的小型移动存储介质。 通常有各种大小和物理形式。

- `MMC卡`
    类似可移动存储设备

- `eMMC设备`
    通常是设计成嵌入到其他系统中的类似存储设备。

这三种设备都共享一个公共协议与主机通信，并且高层次支持对它们来说都是类似的。

在 MicroPython 中是用 一个单例类 `machine.SDCard` 来实现的。


`SD`和`MMC`接口 都支持使用各种总线宽度进行访问。当使用`1-bit`的接口进行访问时，可以使用SPI协议进行访问。

一般来说，构造一个`SDCard对象`不传递任何参数, 因为在STM32F103xx中， _SDIO_ 是固定管脚(MPY-ST103 使用4-bit宽度总线)

#### 构造方法

###### `machine.SDCard()`{:class="method"}

无参数、返回底层SDCard单例
该类提供使用专用`SD/MMC`接口硬件或`SPI`通道访问 `SD` 或 `MMC` 存储卡的能力

该类实现了 `uos.AbstractBlockDev` 定义的块设备协议. 这使得`SD card`安装非常简单:

```py
import uos, machine
uos.mount(machine.SDCard(), "/sd")
```

#### 实例方法

###### `machine.SDCard.present()`{:class="method"} {#sdcard_present}

返回 bool

- `True` : 表示SD卡已插入
- `False` : 则未检测到SD卡


###### `machine.SDCard.power(state)`{:class="method"} {#sdcard_power}

设置SDCard供电状态
- `state` : bool值
    - `True` : 开机供电，初始化SD卡驱动(如果已经初始化， 则不会执行任何操作)
    - `False` : 清理SD卡底层资源， 掉电

返回bool值, 成功-`True`, 失败-`False`


###### `machine.SDCard.info()`{:class="method"} {#sdcard_info}

返回SD卡信息: 3个元素的tuple `(字节容量, 块大小, 卡类型)`
```python
import machine
sd = machine.SDCard()
print(sd.info())

# 打印类似下面：
# (15523119104, 512, 1)
```


#### `os.AbstractBlockDev`接口实现

###### `machine.SDCard.readblocks(block_num, buf)`{:class="method"} {#sdcard_readblocks}
###### `machine.SDCard.readblocks(block_num, buf, offset)`{:class="method"} {#sdcard_readblocks}
###### `machine.SDCard.writeblocks(block_num, buf)`{:class="method"} {#sdcard_writeblocks}
###### `machine.SDCard.writeblocks(block_num, buf, offset)`{:class="method"} {#sdcard_writeblocks}
###### `machine.SDCard.ioctl(op, arg)`{:class="method"} {#sdcard_ioctl}
