---
layout: post
title:  "pyb.USB_VCP类"
date:   2019-08-18 0:0:0 +0000
image: /assets/images/twoscreen.jpg
category: pyb
---

`pyb.USB_VCP`类 用于创建一个类似流对象的USB虚拟串口, 用于与USB连接的主机进行数据收发

#### 构造方法

###### `pyb.USB_VCP()`{:class="method"}

创建一个VCP对象

#### 实例方法

###### `pyb.USB_VCP.setinterrupt(chr)`{:class="method"}

设置执行Python代码时产生中断的字符

默认是 `3`(CTRL-C), 当从USB VCP 收到CTRL-C字符时，会引发一个`KeyboardInterrupt`异常

设为`-1`禁用该中断功能. 这对于使用USB VCP 发送原始数据是比较有用


###### `pyb.USB_VCP.isconnected()`{:class="method"}

返回USB是否连接, `True`-已连接, 否则返回`False`

###### `pyb.USB_VCP.any()`{:class="method"}

返回 `True`表示有字符等待被读取， 否则返回`False`

###### `pyb.USB_VCP.close()`{:class="method"}

This method does nothing. It exists so the USB_VCP object can act as a file.

###### `pyb.USB_VCP.read([nbytes])`{:class="method"}

读取最多 `nbytes`字节数据， 返回 `bytes`对象。

如果`nbytes`未指定，则读取底层所有可读数据(已经收到的数据), 

`USB_VCP`流实现为非阻塞模式， 所以如果底层缓冲没有数据, 会立即返回`None`

###### `pyb.USB_VCP.readinto(buf[, maxlen])`{:class="method"}

读取字节数组存储到`buf`对象中， 通常len(buf)是需要读取的数据大小. 

如果同时提供`maxlen`参数, 那么最大可读取 `min(maxlen, len(buf))` 字节的数据

返回 转存的数据大小, 或者`None` 表示无数据可读

###### `pyb.USB_VCP.readline()`{:class="method"}

读取一整行数据

返回一个持有收到数据的`bytes`对象, 包含换行符, 或者返回`None`表示没有待读数据

###### `pyb.USB_VCP.readlines()`{:class="method"}

从USB VCP 读取尽可能多的单行文本列表

返回一个列表对象, 每个对象存储一行数据(包含换行符)

###### `pyb.USB_VCP.write(buf)`{:class="method"}

发送缓冲数据, 返回写入字节数

###### `pyb.USB_VCP.recv(data, *, timeout=5000)`{:class="method"}

从USB接受数据, 

`data`参数可以是一个数字, 表示需要接收的字节数, 或者一个可变缓冲用于填充接收到的数据

`timeout` 指定接收超时时间(ms)

返回值: 如果`data`是一个数字， 返回一个新的buffer, 其他情况返回接收到了多少字节

###### `pyb.USB_VCP.send(data, *, timeout=5000)`{:class="method"}

发送数据

`data` 是要发送的数据(数字或者一个buffer对象)

`timeout`是发送超时时间(ms)

返回发送的字节数