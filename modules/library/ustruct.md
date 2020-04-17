---
layout: post
title:  "MicroPython ustruct - 打/解包基本数据类型"
date:   2019-08-18 0:0:0 +0000
category: library
---

该模块实现了 *CPython* *struct* 模块的一个子集，如下所述. 更多信息参考 [struct](https://docs.python.org/3.5/library/struct.html#module-struct)

支持的 大小/字节序 前缀: `@`, `<`, `>`, `!`.

支持的格式编码: `b`, `B`, `h`, `H`, `i`, `I`, `l`, `L`, `q`, `Q`, `s`, `P`, `f`, `d` (后2个取决于浮点支持, f103是软件实现, 只支持`f`).


#### 模块函数

###### `ustruct.calcsize(fmt)`{:class="func"}

返回存储给定格式 *fmt* 所需的字节数.


###### `ustruct.pack(fmt, v1, v2, ...)`{:class="func"}
根据 格式 *fmt* 打包 值 *v1*, *v2*, ... .

返回 打包结果: `bytes` 对象.


###### `ustruct.pack_into(fmt, buffer, offset, v1, v2, ...)`{:class="func"}

根据 格式 *fmt* 打包 值 *v1*, *v2*, ... 到 *buffer* 缓冲 *offset* 开始的位置.

*offset* 可为负值, 表示到 *buffer* 结束位置的偏移量.


###### `ustruct.unpack(fmt, data)`{:class="func"}

根据格式字符串 *fmt* 从 *data* 解包数据.

返回打包前各值组成的元组.


###### `ustruct.unpack_from(fmt, data, offset=0)`{:class="func"}

根据格式字符串 *fmt* 从 *data* 的 偏移量 *offset* 处开始解包数据.

*offset* 可为负值, 表示到 *data* 结束位置的偏移量.

返回值是打包前各值组成的元组.
