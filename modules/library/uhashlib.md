---
layout: post
title:  "MicroPython uhashlib - 散列算法库"
date:   2019-08-18 0:0:0 +0000
category: library
---

该模块实现了二进制数据哈希算法. 可用算法依赖于实现平台, 可实现的有:

- __SHA256__ - 现代哈希算法(SHA2系列), 适用于加密安全的情况. 基本所有 _MicroPython_ 都实现了此算法, 除非 Flash 大小受限情况下不提供

- __SHA1__ - 上一代算法。新应用不推荐使用，但是 _SHA1_ 是许多Internet标准和现有应用程序的一部分，因此尝试提供这一功能

- __MD5__ - 一种遗留算法，在密码上不安全。基本上不提供


构造函数
============

##### `class uhashlib.sha256([data])`{:class="class"}

创建一个 _SHA256_ 散列对象，`data` 可选


##### `class uhashlib.sha1([data])`{:class="class"}

创建一个 _SHA1_ 散列对象，`data` 可选


##### `class uhashlib.md5([data])`{:class="class"}

创建一个 _MD5_ 散列对象，`data` 可选


方法
=======

###### `hash.update(data)`{:class="method"}

向散列中输入更多的二进制数据

###### `hash.digest()`{:class="method"}

返回bytes对象 作为所有数据的散列值. 此方法调用后，不能再向散列中输入更多数据

###### `hash.hexdigest()`{:class="method"}

未实现, 用 `ubinascii.hexlify(hash.digest())` 可以达到同样的效果
