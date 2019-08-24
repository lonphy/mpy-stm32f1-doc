---
layout: post
title:  "machine.I2C 类"
date:   2019-08-18 0:0:0 +0000
image: /assets/images/twoscreen.jpg
category: machine
---

`machine.I2C`是一种用于设备之间通信的两线协议

在物理层由两根线组成: `SCL`和`SDA`，即时钟线和数据线

`I2C`对象被创建到一个特定的总线上， 可以在创建时初始化，或者之后初始化

> 打印`I2C`对象, 可以输出其配置的信息

综合示例:

```py
from machine import I2C

i2c = I2C(freq=400000)          # 以400kHz的频率创建I2C外设
                                # 根据实现的不同，可能需要额外的参数用于选择要使用的外设和/或引脚

i2c.scan()                      # 扫描从设备, 返回 7-bit的地址列表

i2c.writeto(42, b'123')         # 写3个字节到从设备(地址=42)
i2c.readfrom(42, 4)             # 从从设备(地址=42) 读取4字节

i2c.readfrom_mem(42, 8, 3)      # 从从设备(地址=42)的内存中地址为8的地方读取3个字节
i2c.writeto_mem(42, 2, b'\x10') # 写1字节到从设备(地址=42)的内存地址2
```

#### 构造方法

###### `machine.I2C(id=-1, *, scl, sda, freq=400000)`{:class="method"}

使用以下参数创建一个`I2C`对象返回:

- `id` 唯一标识一个特定`I2C`外设.
    默认值 `-1`选择软件`I2C`, 大多数情况可以接在任意管脚上(`SCL` + `SDA`)

    如果 `id`=`-1`, 那么 `scl` and `sda` 必须指定.

    其他允许的值依赖于开发板的实际情况与board文件定义, 在这种情况下, `scl`与`sda`可能必填， 也可能无需指定(有硬件I2C分配的情况)

    通用方法```I2C.init(scl, sda, *, freq=400000)```使用给定的参数初始化一个I2C总线(注意id一定是`-1`，否则板子复位)
- `scl` - `SCL` 使用的管脚(管脚对象)
- `sda` - `SDA` 使用的管脚(管脚对象)
- `freq` - `SCL` 最大时钟频率


#### 实例方法

###### `machine.I2C.scan()`{:class="method"}

扫描 `0x08~0x77` 之间的所有`I2C`地址, 并返回有响应的地址列表

从机地址(包含写入位)发送到总线后, 如果从机拉低`SDA`, 则认为从机做出响应(即`ACK`回复)


#### I2C 原始操作

以下方法实现了原始的`I2C`主模式总线操作, 如需对总线有更多的控制， 组合使用这些方法实现I2C事物即可, 其他情况还是使用 [标准方法](#standard) 吧

> __这些方法仅在`软I2C模式`可用__

###### `machine.I2C.start()`{:class="method"}

在总线上产生一个 __开始条件__ (`SCL`保持为高电平, `SDA`拉低(高->低))


###### `machine.I2C.stop()`{:class="method"}

在总线上产生一个 __停止条件__ (`SCL`保持为高电平, `SDA`拉高(低->高))


###### `machine.I2C.readinto(buf, nack=True)`{:class="method"}

从总线读取数据, 并存储到buf. 读取的数据长度由`len(buf)`指定

除最后一字节在接收所有数据时都会在总线上发送`ACK`

最后一字节收到后, 如果`nack=True`, 则回复`NACK`, 其他情况则回复`ACK`(这种情况下, __从设备__ 会认为需要读取更多的数据)


###### `machine.I2C.write(buf)`{:class="method"}

写`buf`到总线

每个字节发送后， 都会检查是否收到`ACK`，若收到`NACK`，则停止传输剩余数据

返回 成功写入数量(即收到的`ACK`数量)

#### 标准总线操作 {#standard}

以下方法实现了标准I2C主设备的读写操作, (需要指定从设备地址)

###### `machine.I2C.readfrom(addr, nbytes, stop=True)`{:class="method"}

从`addr`指定的从设备读取 `nbytes`数据.

如果`stop=True`，在传输结束时发送`停止条件`

返回读到的数据, `bytes`对象


###### `machine.I2C.readfrom_into(addr, buf, stop=True)`{:class="method"}

从`addr`指定的从设备读取数据并转存`buf`. 读取的字节数以`buf`长度为准.

如果`stop=True`，在传输结束时发送`停止条件`

返回`None`


###### `machine.I2C.writeto(addr, buf, stop=True)`{:class="method"}

向`addr`指定的从设备写入数据`buf`, 如果发送期间收到`NACK`, `buf`中剩余的数据不会发送

如果`stop=True`，(即使收到`NACK`) 在传输结束时发送`停止条件`

返回成功写入的长度(收到的`ACK`数量)


###### `machine.I2C.writevto(addr, vector, stop=True)`{:class="method"}

向`addr`指定的从设备写入 vector中的 `bytes`对象

`vector` 是 __缓冲协议__ 对象的元组或列表, `addr` 发送一次, 然后顺序写入每个对象, 对象的缓冲长度可能是0, 这种情况下不会发送

在任意对象的缓冲发送期间, 如果收到`NACK`, 剩余的部分不会发送(包括未发完的缓冲及对象)

如果`stop=True`，(即使收到`NACK`) 在传输结束时发送`停止条件`

返回成功写入的长度(收到的`ACK`数量)


#### 内存操作

一些I2C设备表现为一种内存设备(或寄存器集合)以供读写。这种情况下，I2C事务关联2个地址: __从设备地址__ 及 __从设备内存地址__.

为此类设备通信的方便， 提供以下函数:

###### `machine.I2C.readfrom_mem(addr, memaddr, nbytes, *, addrsize=8)`{:class="method"}

从`addr`指定的从设备的内存地址`memaddr`处, 读取`nbytes`字节

`addrsize` 指定以bit为单位的地址大小

返回读到的数据(`bytes`对象)


###### `machine.I2C.readfrom_mem_into(addr, memaddr, buf, *, addrsize=8)`{:class="method"}

从`addr`指定的从设备的内存地址`memaddr`处, 读取数据到`buf`. 读取长度以`buf`长度为准.

`addrsize` 指定以bit为单位的地址大小

返回`None`


###### `machine.I2C.writeto_mem(addr, memaddr, buf, *, addrsize=8)`{:class="method"}

向地址是`addr`的 __从设备__ 的 `memaddr`处 写入 `buf`

`addrsize` 指定以bit为单位的地址大小

返回`None`
