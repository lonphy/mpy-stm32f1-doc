---
layout: post
title:  "MicroPython uzlib - zlib库(解压)"
date:   2019-08-18 0:0:0 +0000
category: library
---

`uzlib` 库可用于解压由 `DEFLATE` 算法压缩的二进制数据。

> 注意: 压缩功能未实现


#### 库方法

###### `uzlib.decompress(data, wbits=0, bufsize=0)`{:class="method"}

返回解压后的数据(`bytes`)

`wbits` 是 `DEFLATE` 压缩时的字典窗口大小(8-15, 字典大小是窗口大小的2的整次幂)。

若`wbits`值为正, `data`按`zlib`数据流(带zlib头部)处理. 

否则, `data` 按`原始 DEFLATE`流处理

- `bufsize` 为了兼容CPython而存在, 内部处理时忽略

###### `uzlib.DecompIO(stream, wbits=0)`{:class="method"}

创建一个 __流包装__, 用于透明解压另一个流中的压缩数据。

这允许处理数据大于可用堆大小的压缩流时十分有效.

`wbits`除了可以取`decompress()`中描述的值外，还可以取`24..31(16 + 8..15)`，意味着输入流有gzip头
<br><br><br>