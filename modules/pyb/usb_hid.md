---
layout: post
title:  "pyb.HID 类"
date:   2019-08-18 0:0:0 +0000
image: /assets/images/twoscreen.jpg
category: pyb
---


`pyb.USB_HID`类 可供创建一个人机交互接口的对象, 它可用来模拟USB-HID设备，例如一个鼠标或键盘

在使用`pyb.USB_HID`类之前， 需要调用 `pyb.usb_mode()` 设置USB为包含HID接口的模式


#### 构造方法

###### `pyb.USB_HID()`{:class="method"}

创建一个`USB_HID`对象

#### 实例方法

###### `pyb.USB_HID.recv(data, *, timeout=5000)`{:class="method"}

接收数据

`data` 可以是一个数字, 指定要接收的字节数， 或者一个可变缓冲，用于填充收到的数据

`timeout` 指定接收超时时间(ms)

返回值: 如果`data`是一个数字，则返回一个新的buffer对象, 否则返回接收到的字节数

###### `pyb.USB_HID.send(data)`{:class="method"}

发送数据

`data` 是要发送的数据 (一个元组/列表类型的数字序列, 或一个`bytearray`).

<br>