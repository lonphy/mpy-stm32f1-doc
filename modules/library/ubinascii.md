---
layout: post
title:  "MicroPython ubinascii - 二进制/ASCII转换"
date:   2019-08-18 0:0:0 +0000
category: library
---

此模块实现二进制数据与ASCII格式的各种编码之间的双向转换


函数
=========

###### `ubinascii.hexlify(data[, sep])`{:class="func"}

将二进制数据转换为十六进制表示, 返回字节字符串.

> 与 _CPython_ 差异: 如果提供了 `sep` 参数, 会被当成分割符 分割十六进制值


###### `ubinascii.unhexlify(data)`{:class="func"}

将十六进制数据转换为二进制表示, 返回字节字符串(与`hexlify`相反)


###### `ubinascii.a2b_base64(data)`{:class="func"}

解码通过 base64编码的数据 `data`, 丢弃数据中的非法字符. 符合RFC 2045 s.6.8。返回一个bytes对象。


###### `ubinascii.b2a_base64(data)`{:class="func"}

以base64格式编码二进制数据(RFC 3548). 以 `bytes`对象的形式返回编码后的数据+换行字符

