---
layout: post
title:  "MicroPython gc - 垃圾回收控制"
date:   2019-08-18 0:0:0 +0000
category: library
---


模块方法
=========

###### `gc.enable()`{:class="func"}

开启自动垃圾回收


###### `gc.disable()`{:class="func"}

禁用自动垃圾回收

堆内存仍然可以分配, 垃圾回收可以用`gc.collect()`手动执行


###### `gc.collect()`{:class="func"}

执行垃圾回收


###### `gc.mem_alloc()`{:class="func"}

返回堆内存已分配的字节数。

> `MicroPython` 特有扩展


###### `gc.mem_free()`{:class="func"}

返回可用堆内存字节数, `-1` 表示未知

> `MicroPython` 特有扩展

###### `gc.threshold([amount])`{:class="func"}

设置或查询附加的GC分配阈值。正常情况下， 垃圾回收只有在无法满足新的分配时会被触发, 例如, 内存溢出的情况。

如果调用本函数设置了阈值，每次分配超过阈值之后都会触发一个回收(已分配的总内存), `OOM`情况不会触发(因为不会实际分配内存)

`amount` 通常设置的要比总的可用堆内存小, 目的是在堆内存耗尽前触发一个垃圾回收。这是一个启发式度量, `amount`最优值因应用而异。

无参调用时返回当前阈值. `-1` 表示阈值禁用

> 与`CPython`不同, 这是`MicroPython` 特有扩展
