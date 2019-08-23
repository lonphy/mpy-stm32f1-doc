---
layout: post
title:  "machine.UART 类"
date:   2019-08-18 0:0:0 +0000
image: /assets/images/twoscreen.jpg
category: machine
---

`machine.UART` - 全双工串行通讯总线

`UART` 实现了标准的UART/USART全双工串行通信协议, 在物理层有两根线: `RX` 和 `TX`. 通讯单位是一个8/9bit 字符.


`UART`可以用下面的代码创建于初始化:

```py
from machine import UART

uart1 = UART(1, 9600)                         # 使用给定的波特率创建
uart1.init(9600, bits=8, parity=None, stop=1) # 使用给定参数初始化
```

- `bits` - 字符串宽度, 可以是 `8` 或 `9`.
- `parity` - 奇偶校检位, 可以是 `None`, `0` (even) or `1` (odd).
- `stop` - 停止位, 可以是 `1` 或 `2`

`UART`对象实现了流接口, 可以使用标准流方法读写:

```py
uart1.read(10)       # 读取10个字符, 返回 bytes 对象
uart1.read()         # 读取所有可读字符
uart1.readline()     # 读取一行
uart1.readinto(buf)  # 读取并存储到传入的缓冲
uart1.write('abc')   # 写3个字符
```

 单字符读写可以使用:

```py
uart1.readchar()     # 读一个字符, 返回对应的ASCII码(数字)
uart1.writechar(42)  # 写入1个字符
```

 检查是否有任意字符可供读取:

```py
uart1.any() # 返回True表示有数据待读
```

#### 流控

只有`UART(2)`, `UART(3)`支持流控

若调用`UART.init()`时，`flow`参数设置为 `RTS`, `CTS` 或两者组合, 则启用流控

`RTS`是输出低电平有效, `CTS`是低电平输入有效(上拉使能)

为了实现流控，开发板的`CTS`引脚应该连接到目标设备`RTS`引脚，并将开发板`RTS`引脚连接到目标设备的`CTS`引脚

###### `CTS` 流控模式下, 发送类方法行为如下:

- `UART.write(buf)`:
    当`nCTS`是`False`时, 发送暂缓. 如果超时之前未将`buf`全部发送完成，则会触发超时  
    同时返回已经发送的字节数, 以便用户按需发送剩余的数据

    超时的情况下, 会有一个字符保留在`UART`中，等待`nCTS`为`True`。返回包含此字符的未发送字节数

- `UART.writechar()`:
    当`nCTS`是`False`时, 除非目标设备及时变换`nCTS`为`True`, 否则超时

    如果超时, 会引发`OSError`(116)异常, 一旦目标设备置`nCTS`为`True`, 字符会尽快被传输

###### `RTS` 流控模式下, 接收类方法的行为如下:

- 如果启用缓冲输入（`read_buf_len` > 0），接收到的字符将被缓冲
    - 如果缓冲区将满，下一个字符的到来将导致`nRTS`被设为`False`(目标设备应停止后续传输)
    - 当从缓冲区读取字符后，`nRTS`将会被设为`True`

> 注意: `UART.any()`方法返回缓冲区中的字节数  
> 假定缓冲区为N字节. 如果缓冲区将满，另一个字符到达，`nRTS`会被设置为`False`，同时`UART.any()`返回N
>
> 当读取字符时，附加字符将被放置在缓冲区中，并将被包含在下次`UART.any()`调用的结果中

- 如果禁用输入缓冲（`read_buf_len` == 0), 收到字符后就会将`nRTS`置为`False`, 直到字符被读走


#### 类常量

- `UART.RTS`{:class="const"} - 流控类型`RTS`

- `UART.CTS`{:class="const"} - 流控类型`CTS`

- `UART.IRQ_RXIDLE`{:class="const"} - 用于中断入参

#### 构造方法

###### `machine.UART(bus)`{:class="method"}

创建一个UART对象, bus 取值范围[1,5]分别对应底层(`USART1`,`USART2`,`USART3`,`UART4`,`UART5`)

如果没有其他参数, 构造方法只是创建了一个`UART`对象, 但没有初始化(实际持有上次初始化的参数)

如果传入其他参数, 该总线会被初始化. 参考[UART.init方法](#uart_init)查看初始化参数.

各串口对应管脚如下:

- `UART(1)` : `(TX, RX) = (PA9,PA10)`
- `UART(2)` : `(TX, RX, CTS, RTS) = (PA2, PA3, PA0, PA1)`
- `UART(3)` : `(TX, RX, CTS, RTS) = (PB10, PB11, PB13, PB14)`
- `UART(4)` : `(TX, RX) = (PC10, PC11)`
- `UART(5)` : `(TX, RX) = (PC12, PD2)`

> 以上`UARTx`对应管脚可通过`machine.PinRemap`调整

#### 实例方法

###### `machine.UART.init(baudrate, bits=8, parity=None, stop=1, *, timeout=1000, timeout_char=0, flow=0, read_buf_len=64)`{:class="method"}    {#uart_init}

用给定的参数初始化`UART`:
- `baudrate`  - 波特率
- `bits` - 一个字符的bit位 8 或 9.
- `parity` - 奇偶校检: `None`, `0` (even) 或 `1` (odd).
- `stop` - 停止位: 1 或 2.
- `timeout` - 读取第一个字符的超时时间(ms), 同时也是写超时时间, 默认`1000ms`
- `timeout_char` - 读取的字符间隔超时时间(ms)
- `flow` - 流控: `RTS|CTS`,  `RTS` == 256, `CTS` == 512
- `read_buf_len` - 读缓冲大小(0，禁用读缓冲)

###### `machine.UART.deinit()`{:class="method"}

关闭UARTx总线

###### `machine.UART.any()`{:class="method"}

有任意字符待读返回`True`, 否则返回 `False`.


###### `machine.UART.readchar()`{:class="method"}

读取单个字符

返回读到的字符， 超时返回`-1`


###### `machine.UART.sendbreak()`{:class="method"}

发送一个中断帧


###### `machine.UART.irq(handler, trigger, hard)`{:class="method"}

设置或返回字符中断回调

- `handler` - 回调处理器, 入参为`UART`实例`self`
- `trigger` - CR1 对应的中断位, 可用的目前只有`UART.IDLEIE`(空闲帧)
- `hard` - 布尔值, `True` - 表示硬件回调(Handler模式，会禁用GC及内存分配), `False` - 则添加回调函数到调度器, 建议填`False`

```py
def rxIdleCb(n):
    print(n.readline())

uart1.irq(rxIdleCb, UART.IRQ_RXIDLE, False)
```


###### `machine.UART.read([nbytes])`{:class="method"}

读取最多 `nbytes`字节数据， 返回 `bytes`对象。如果待读数据大于等于`nbytes`, 则立即返回, 否则等到足够长度的数据或超时才返回

如果`nbytes`未指定，则读取底层所有待读数据(已经收到的数据), 并且等到超时为止

`UART`流实现为非阻塞模式， 所以如果读缓冲没有数据, 会立即返回`None`(前提是把`timeout`设为0)

> 如果配置的是9bit位宽， 则每个字符占用2字节, `nbytes`必须是偶数, 实际要读取的长度是 `nbytes/2`

返回读到的字节数, 超时则返回`None`


###### `machine.UART.readline()`{:class="method"}

读取尽可能多的单行文本列表, 直到达到`timeout`

返回一个字符串(包含换行符) 或未读到任何数据返回`None`


###### `machine.UART.readinto(buf[, nbytes])`{:class="method"}

读取并存储到`buf`对象中， 通常len(buf)是需要读取的数据大小.

如果同时提供`maxlen`参数, 则最大读取 `min(maxlen, len(buf))` 字节的数据

返回 读到的数据长度, 或者`None` 表示超时

###### `machine.UART.writechar(char)`{:class="method"}

写入单个字符, `char` 是需要写入的字符


###### `machine.UART.write(buf)`{:class="method"}

发送缓冲数据,  如果配置为8bit， 每个字符占1字节. 如果配置为9bit, 每个字符占2字节(小端), 此时`buf`的长度必须为偶数

返回 写入字节数, 如果超时且没有数据被发送则返回`None`

<br>
