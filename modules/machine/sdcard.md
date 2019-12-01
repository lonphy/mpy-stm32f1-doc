---
layout: post
title:  "machine.SDCard 类"
date:   2019-08-18 0:0:0 +0000
image: /assets/images/twoscreen.jpg
category: machine
---

`machine.SDCard` 是一个单例类

SD cards are one of the most common small form factor removable storage media. SD cards come in a variety of sizes and phsyical form factors. 

MMC cards are similar removable storage devices while eMMC devices are electically similar storage devices designed to be embedded into other systems. 

All three form share a common protocol for communication with their host system and high-level support looks the same for them all. 

As such in MicroPython they are implemented in a single class called `machine.SDCard`.


Both `SD` and `MMC` interfaces support being accessed with a variety of bus widths. 

When being accessed with a 1-bit wide interface they can be accessed using the SPI protocol. 

Different MicroPython hardware platforms support different widths and pin configurations but for most platforms there is a standard configuation for any given hardware. 

In general constructing an SDCard` object with without passing any parameters will initialise the interface to the default card slot for the current hardware. 

The arguments listed below represent the common arguments that might need to be set in order to use either a non-stanard slot or a non-standard pin assignment. 

The exact subset of arguments suported will vary from platform to platform.

#### 构造方法

###### `machine.SDCard(slot=1, width=1, cd=None, wp=None [, sck=None, miso=None, mosi=None, cs=None])`{:class="method"}

该类提供使用专用`SD/MMC`接口硬件或`SPI`通道访问 `SD` 或 `MMC` 存储卡的能力

该类实现了 `uos.AbstractBlockDev` 定义的块设备协议. 这使得`SD card`安装非常简单:

```py
uos.mount(machine.SDCard(), "/sd")
```

构造方法包含以下参数:

- `slot` - selects which of the available interfaces to use. Leaving this unset will select the default interface.
- `width` - selects the bus width for the SD/MMC interface.
- `cd` - can be used to specify a card-detect pin.
- `wp` - can be used to specify a write-protect pin.

- `sck` - can be used to specify an SPI clock pin.
- `miso` - can be used to specify an SPI miso pin.
- `mosi` - can be used to specify an SPI mosi pin.
- `cs` - can be used to specify an SPI chip select pin.