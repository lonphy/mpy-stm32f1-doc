---
layout: post
title:  "MicroPython usocket - 套接字库"
date:   2019-08-18 0:0:0 +0000
category: library
---

`usocket` -- 套接字模块
======================

*usocket* 模块提供访问 BSD套接字的接口.

> 与 _CPython_ 差异:   
> - 为了性能与一致性, __socket__ 在 __MicroPython__ 中直接实现为一个 `流` (类文件)接口. 
> - 在 _CPython_ 中 通常需要用 `makefile()`方法 转换socket 对象 为 类文件对象, __MicroPython__ 中仍然支持这个方法(实际什么都没做), 为了与 _CPython_ 兼容，尽量使用它


socket 地址格式
========================

*usocket* 模块的本地地址格式是由 `getaddrinfo`函数返回的不透明数据类型. 它必须用于解析文本地址(包括IP地址):

```py
# 解析域名到地址
sockaddr = usocket.getaddrinfo('www.micropython.org', 80)[0][-1]
# 即使是IP地址, 也必须使用 getaddrinfo()
sockaddr = usocket.getaddrinfo('127.0.0.1', 80)[0][-1]
# 使用解析后的地址
sock.connect(addr)
```

* 在编写可移植的应用程序时，总是使用`getaddrinfo`。

* 下面描述的元组地址可以作为使用的快捷方式. 

`socket` 模块的 _地址元组_ 格式:

* IPv4: `(ipv4_address, port)`:
    - *ipv4_address* - 点分数字格式的IPv4地址(字符串)，例如 *8.8.8.8*
    
    - *port* - 端口号, 范围 1-65535. 

* IPv6: `(ipv6_address, port, flowinfo, scopeid)`:
    - *ipv6_address* - 冒号分割的十六进制数字IPv6地址(字符串), 例如 ""2001:db8::1""

    - *port* - 端口号, 范围 1-65535. 
    
    - *flowinfo* - 必须是 0.
    
    - *scopeid* - 本地链接地址的接口范围标识符.


模块方法
=========
###### `usocket.socket(af=AF_INET, type=SOCK_STREAM, proto=IPPROTO_TCP)`{:class="class"}

使用给定的地址类型、socket类型创建新的 *socket*. 

```py
from usocket import *
# 创建 TCP socket
socket(AF_INET, SOCK_STREAM)
# 创建 UDP socket
socket(AF_INET, SOCK_DGRAM)
```

###### `usocket.getaddrinfo(host, port, af=0, type=0, proto=0, flags=0)`{:class="func"}

将 主机/端口 参数转换为包含创建 连接到该服务的 *socket* 所需的所有参数的5元组.


*af*, *type*, 与 *proto* (和`socket()`构造参数相同) 用于过滤指定类型地址(当前实现固定返回 `AF_INET` + `SOCK_STREAM`).

结果列表的 `5-tuples` 结构如下:

`(family, type, proto, canonname, sockaddr)`

下面演示如何连接到给定的url:

```py
s = usocket.socket()
s.connect(usocket.getaddrinfo('www.micropython.org', 80)[0][-1])
```


常量
=========

- `usocket.AF_INET`{:class="const"}
- `usocket.AF_INET6`{:class="const"}

地址类型, `AF_INET` 对应 *IPv4*, `AF_INET6` 对应 *IPv6*. 

- `usocket.SOCK_STREAM`{:class="const"}
- `usocket.SOCK_DGRAM`{:class="const"}
   
Socket 类型. 

- `usocket.IPPROTO_UDP`{:class="const"}
- `usocket.IPPROTO_TCP`{:class="const"}

IP 协议类型, 无需调用 `usocket.socket()` 时传入, 因为 `SOCK_STREAM` 自动选择 `IPPROTO_TCP`, `SOCK_DGRAM` 自动选择 `IPPROTO_UDP`, 

- `usocket.SOL_*`{:class="const"}

*socket* 选项级别(`setsockopt()`的参数).


- `usocket.SO_*`{:class="const"}

*socket* 选项(`setsockopt()`的参数).


Socket 类方法
=======

##### `socket.close()`{:class="method"}

将 *socket* 标记为关闭并释放所有资源. 
一旦发生这种情况，其上的所有后续操作都将失败.
如果协议支持，远端将接收EOF指示.

当被垃圾收集时，*socket* 会自动关闭，但是建议在完成工作后显式地`close()`掉.

##### `socket.bind(address)`{:class="method"}

将 *socket* 绑定到给定 *address* . 该 *socket* 必须尚未绑定

##### `socket.listen([backlog])`{:class="method"}

【TCP】使能服务接收连接. `backlog` 无用

##### `socket.accept()`{:class="method"}

【TCP】接收一个连接. *socket* 必须是已绑定且在监听连接. 返回一个 `(conn, address)`对:

- `conn` 是个 新的 *socket* 对象, 可调用 `TCP` 及 通用的 方法
- `address` 是被接收连接的 *socket* 地址

##### `socket.connect(address)`{:class="method"}

【TCP】*socket* 连接到 *address*


##### `socket.send(bytes)`{:class="method"}

【TCP】发送数据到 *socket* . *socket* 必须已连接到远端. 返回已发送的字节数(可能小于 `bytes` 长度, 即 __短写__ 的情况)


##### `socket.recv(bufsize)`{:class="method"}

【TCP】从 *socket* 接收数据. 返回接收到的数据 - `bytes` 对象. 接收长度由 `bufsize` 指定.

##### `socket.sendto(bytes, address)`{:class="method"}

【UDP】发送数据到 *socket* . 不要在 `TCP` socket 上执行, 发送目标由 `address` 指定.


##### `socket.recvfrom(bufsize)`{:class="method"}

【UDP】从 *socket* 上读取数据. 返回值是 *(bytes, address)*:

- `bytes` - 是收到的数据
- `address` - 是发送端的 *socket* 地址

##### `socket.setsockopt(level, optname, value)`{:class="method"}

为 *socket* 设定选项值. 所需符号常量 在 *socket* 模块定义(`SO_xxx`等). 
`value` 是个数字或 类 `bytes` 对象提供的缓冲.


##### `socket.settimeout(value)`{:class="method"}

设置 *socket* 阻塞操作的超时时间. `value` 可以是非负浮点数(单位 秒) 或 `None`. 
- `value` 是 *非0值* , 且则随后的 *socket* 操作完成前耗费的实际大于该值则引发`OSError`异常.

- `value` 是 *0* , 则 *socket* 操作处于 **非阻塞** 模式

- `value` 是 `None`, 则 *socket* 完全出入 **阻塞** 模式


更通用的做法是 使用 `uselect.poll` 对象, 可同时在多个对象上等待(不限于 *socket* , 在支持 `流` 的对象上都可以 ), 示例:

```python
# 设超时方式
s.settimeout(1.0)  # 设置超时1s
s.read(10)  # 读取可能会引发超时

# 轮询方式:
poller = uselect.poll()
poller.register(s, uselect.POLLIN)
res = poller.poll(1000)  # 设置轮询间隔1000ms
if not res:
    # s 还未收到数据, 例如读取超时
```

> 与 _CPython_ 差异:   
> _CPython_ 在超时情况下引发`socket.timeout`异常(`OSError`的子类), _MicroPython_ 则直接引发`OSError`.


##### `socket.setblocking(flag)`{:class="method"}

设置 *socket* 为 `True`-**阻塞** 或 `False`-**非阻塞** 模式.

该方法是 `settimeout` 的快捷调用:

* `sock.setblocking(True)` 等价于 `sock.settimeout(None)`

* `sock.setblocking(False)` 等价于 `sock.settimeout(0)`


##### `socket.makefile(mode='rb', buffering=0)`{:class="method"}

返回 *socket* 关联的文件对象. `mode` 限定 二进制操作(*rb*, *wb*, *rwb*). `buffering`参数无效


##### `socket.read([size])`{:class="method"}

从 *socket* 读取至多`size`字节. 返回 `bytes` 对象. 如果未指定`size`则读取所有可用数据,除非碰到`EOF`; 
因此 *socket* 关闭前不会返回. 该函数尝试读取尽可能多的数据(无短读). 

但对于 *非阻塞socket* 不现实, 会返回已就绪的数据.


##### `socket.readinto(buf[, nbytes])`{:class="method"}

读取数据到 `buf` 中, 如果指定了 `nbytes` 则最多读取这么多. 其他情况读取 `len(buf)` 字节.
与`read()`一样，该方法遵循 *无短读* 策略.

返回 存储到 `buf` 中的已读取数据长度


##### `socket.readline()`{:class="method"}

读取一行, 以换行符结束.

##### `socket.write(buf)`{:class="method"}

写`buf`持有的数据到 *socket* . 该方法尝试写入所有的数据(无 *短写* )。 

对于 *非阻塞 socket* 是不现实的， 返回值可能小于 `buf` 的数据长度.

返回已写入数据字节长度
