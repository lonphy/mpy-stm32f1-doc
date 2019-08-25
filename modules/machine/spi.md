---
layout: post
title:  "machine.SPI 类"
date:   2019-08-18 0:0:0 +0000
image: /assets/images/twoscreen.jpg
category: machine
---


`machine.SPI`是一个主模式的同步串行协议实现, 在物理层面, 总线包含3条信号线: `SCK`, `MOSI`, `MISO`

多个设备可共享同一总线, 对于每个从设备都应该有独立的`SS`信号线, 用来选择要通讯的唯一从设备

`SS`信号线应该在用户代码层面管理, 使用[machine.Pin类]({% link modules/machine/pin.md %})


#### 类常量

- `SPI.MSB`{:class="const"} - 以`MSB`顺序交换数据(高bit先发)

- `SPI.LSB`{:class="const"} - 以`LSB`顺序交换数据(低bit先发)

#### 构造方法

###### `machine.SPI(id, ...)`{:class="method"}

使用给定的`id`构造`SPI`对象, `id`取值依赖于板文件定义(对于`F103`, `硬SPI`最大取值[1,3])

`id=1,2,3` 分别选择 `SPI1,2,3`

`id=-1` 可用于构造一个`软SPI`

不传入其他参数的情况, `SPI`对象会被创建但不会初始化(实际是上一次初始化配置值)

传入其他附加参数的话， `SPI`总线会被顺带初始, 参数参考[SPI.init()](#spi_init)


#### 实例方法

###### `machine.SPI.init(baudrate=1000000, *, polarity=0, phase=0, bits=8, firstbit=SPI.MSB [, sck=None, mosi=None, miso=None])`{:class="method"} {#spi_init}

使用给定的参数初始化`SPI`总线:
- `baudrate` - `SCK` 时钟频率

- `polarity` - `SCK`极性,规定空闲时`SCK`线的电平高低,  取值`0`或`1`

- `phase` - 时钟相位, 指定在 第一或第二时钟沿采样, 取值`0`或`1`

- `bits` - 每次传输的数据bit宽度, `8` 或 `16`(所有硬件都保证支持8bit)

- `firstbit` - 字节内bit交换顺序, 取值`SPI.MSB`或`SPI.LSB`

- `sck`, `mosi`, `miso` 用于总线信号的 [管脚(`machine.Pin`)]({% link modules/machine/pin.md %}) 对象  
> 对于硬件`SPI`(通过`id`参数选择), 管脚是固定的, 不支持修改  
> 对于`F103`系列, 只有`SPI1`支持管脚重映射(通过板定义文件 或`machine.PinRemap` 来修改)  
> `SPI1` 可重映射到 (`NSS`/`PA15`, `SCK`/`PB3`, `MISO`/`PB4`, `MOSI`/`PB5`)  
> `软SPI`支持任意的管脚组合 (`id` = `-1`)

在`硬SPI`的情况下, 实际时钟频率可能低于配置的`baudrate`, 可打印`SPI对象`查看实际时钟频率

###### `machine.SPI.deinit()`{:class="method"}

关闭SPI总线

###### `machine.SPI.read(nbytes, write=0x00)`{:class="method"}

读取`nbytes`长度的数据, 同时写入单字节数据`write`

返回 读到的数据(`bytes`对象)


###### `machine.SPI.readinto(buf, write=0x00)`{:class="method"}

读取数据到`buf`指定的缓冲, 同时写入单字节数据`write`

返回`None`

###### `machine.SPI.write(buf)`{:class="method"}

写`buf`中的字节数据, 返回`None`


###### `machine.SPI.write_readinto(write_buf, read_buf)`{:class="method"}

写`write_buf`的同时，读取数据到`read_buf`

两个缓冲可以相同或不同, 但长度必须相等

返回`None`
