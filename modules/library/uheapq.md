---
layout: post
title:  "MicroPython uheapq - 堆队列算法"
date:   2019-08-18 0:0:0 +0000
category: library
---

此模块实现了堆队列算法.

堆队列 是仅仅是个列表, 以特定方式存储其元素(后进先出FIFO)


函数
=========

###### `uheapq.heappush(heap, item)`{:class="func"}

把`item` 压入 `heap`中.

###### `uheapq.heappop(heap)`{:class="func"}

从`heap`中弹出第一个元素, 空堆将引发 `IndexError`.

###### `uheapq.heapify(x)`{:class="func"}

转换 列表 `x` 为一个堆. 这是个就地操作.
