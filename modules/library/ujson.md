---
layout: post
title:  "MicroPython ujson - JSON库"
date:   2019-08-18 0:0:0 +0000
category: library
---

`ujson`库用于在 Python对象和 JSON 数据格式之间转换


函数
=========

###### `ujson.dump(obj, stream)`{:class="func"}

将`obj`序列化成 _JSON_ 字符串, 并写入给定的`stream`.


###### `ujson.dumps(obj)`{:class="func"}

返回`obj`对应的 _JSON_ 字符串


###### `ujson.load(stream)`{:class="func"}

按 _JSON_ 字符串 解析给定 `stream`, 反序列化成 _Python_ 对象， 并返回

解析持续到文件结束. 如果 `stream` 中的数据不是正确的 _JSON_ 格式 将引发一个 `ValueError`.


###### `ujson.loads(str)`{:class="func"}

解析 _JSON_ 格式字符串 `str` 并返回一个对象. 如果字符串格式不正确 将引发一个 `ValueError`.

