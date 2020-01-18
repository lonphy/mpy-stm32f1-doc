---
layout: post
title:  "network 网络配置库"
date:   2019-12-30 0:0:0 +0000
category: specific
---

network库提供网络驱动和路由配置。要使用此模块，必须使用具有网络功能的MicroPython 固件。

通过配置接口提供的网络服务可以通过`usocket`库使用。

例如:
```py
# 连接， 显示IP配置
import network, utime

nic = network.Driver(...)
if not nic.isconnected():
    nic.connect()
    print("Waiting for connection...")
    while not nic.isconnected():
        utime.sleep(1)
print(nic.ifconfig())

# 在这里就可以正常使用usocket了
import usocket as socket
addr = socket.getaddrinfo('micropython.org', 80)[0][-1]
s = socket.socket()
s.connect(addr)
s.send(b'GET / HTTP/1.1\r\nHost: micropython.org\r\n\r\n')
data = s.recv(1000)
s.close()
```

通用网络适配器接口(NIC)
================================

本节描述一个(隐式)抽象基类，用于规范为MicroPython不同网络硬件实现网络接口类。

这意味着MicroPython实际上并没有提供`AbstractNIC`类，但是任何实际的NIC类都需实现这里所描述的方法:

### class network.AbstractNIC(id=None, ...)

实例化一个网络接口对象。参数依赖于网络接口。如果有多个相同类型的NIC，那么第一个参数应该是`id`


###### `AbstractNIC.active([is_active])`{:class="method"}

如果传入一个`bool`参数， 则使能(`Ture`) 或 失能(`False`) 网络接口。

如果未提供参数, 则查询当前状态。

大多数其他方法需要一个活动的(使能)接口(在非活动(失能)接口上调用它们的行为是未定义的)。


###### `AbstractNIC.connect([service_id, key=None, *, ...])`{:class="method"}

把网络模块连接到网络(大多数情况下: 网络模块连接路由器)。

这个方法时可选实现的, 并且只对那些不“总是连接”的接口可用(例如Wifi)。

如果未指定参数, 则连接到默认的网络(或者唯一的)。如果只传递一个参数, 将被视为要连接的网络唯一标识。

在网络模块是WIFI的情况， 可能还要传递第二个参数(密码).

根据网络模块实际情况或特定设备，还可以有更多的任意关键字参数。

参数可用于: 

* a) 指定可供选择的服务标识类型;
* b) 提供额外的参数用于连接;

对于各种介质类型，有不同的预定义/推荐参数集，其中:

* WiFi: *bssid* 关键字用于连接到特定的BSSIC(可以认为是路由器的MAC地址)

###### `AbstractNIC.disconnect()`{:class="method"}

断开网络


###### `AbstractNIC.isconnected()`{:class="method"}

返回网络连接状态, `True` - 已连接, `False` - 未连接


###### `AbstractNIC.scan(*, ...)`{:class="method"}


扫描可用的网络服务/连接. 返回带有可发现服务参数的元组/列表。

对于各种网络媒体，有各种预定义/推荐的元组格式:

* `WiFi`: (ssid, bssid, channel, RSSI, authmode, hidden). 可能有更多特定于特定设备的字段。

该函数可以接受额外的关键字参数来过滤扫描结果(例如: 针对特定的服务进行、特定的扫描通道，用于特定集合的服务等)
并对其产生影响。可能的话，参数名应该与connect()中的匹配。


###### `AbstractNIC.status([param])`{:class="method"}

查询网络接口的动态状态信息。当不传参调用时返回结果标识连接状态。

其他情况 *param* 是个要查询的 _特定状态参数名称_ 的字符串。

返回类型和值取决于网模块。可能支持的一些参数是:

* `WiFi STA`: 使用`rssi`检索AP信号的强度。
* `WiFi AP`: 使用`stations`检索所有连接到AP的客户端列表, 包含(MAC, RSSI)的元组。

###### `AbstractNIC.ifconfig([(ip, subnet, gateway, dns)])`{:class="method"}

获取/设置 IP层网络接口参数: IP 地址, 子网(网络掩码), 网关 以及 DNS 服务器。

如果未指定任何参数， 调用返回一个4元素的元组包含以上信息。

如果要配置以上参数，则需传入4元素元组, 例如:

```python
nic.ifconfig(('192.168.1.4', '255.255.255.0', '192.168.1.1', '114.114.114.114'))
```

###### `AbstractNIC.config('param')`{:class="method"}
###### `AbstractNIC.config(param=value, ...)`{:class="method"}

获取或设置一般网络接口参数。这些方法允许处理标准IP配置之外的其他参数(如`ifconfig()`)。

这些参数包括特定网络和特定硬件的参数。对于参数设置应该使用 __keyword__ 参数语法，并且可以一次设置多个参数。

对于查询，参数名应该使用引号包裹的字符串，并且一次只能查询一个参数:

```python
# 设置Wifi的AP名称(专业名称是 ESSID ) 和通道
ap.config(essid='My AP', channel=11)

# 一次查询一个参数
print(ap.config('essid'))
print(ap.config('channel'))
```

MicroPython STM32F1内置NIC实现
======================================

下面的类实现了AbstractNIC接口，并提供了一种控制各种网络接口的方法:
* [class network.WLAN]({% link modules/specific/network.wlan.md %}) --- 控制内置 Wifi接口(ESP8266-01/S)
