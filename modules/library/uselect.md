---
layout: post
title:  "MicroPython uselect - 在一组流上等待事件"
date:   2019-08-18 0:0:0 +0000
category: library
---

*uselect* 模块提供的函数可以有效地等待多个 `流` 上的事件(选择已就绪的流).


模块方法
=========

###### `uselect.poll()`{:class="class"}

创建一个 `Poll类` 的实例

###### `uselect.select(rlist, wlist, xlist[, timeout])`{:class="class"}

等待一组对象上的活动. 这个函数是为了兼容，但是效率不高。建议使用`Poll`代替。


类 `Poll` 实例方法
=================

##### `poll.register(obj[, eventmask])`{:class="method"}

注册 `流` 对象 `obj` 到 轮询对象中. `eventmask` 可以逻辑或组合以下事件:

* `uselect.POLLIN`  - 数据读就绪

* `uselect.POLLOUT` - 数据写就绪

> 注意: 事件 `uselect.POLLHUP` 与 `uselect.POLLERR` 在此处不可用.

*eventmask* 默认值 `uselect.POLLIN | uselect.POLLOUT`.

对于同一个 `obj`，多次调用该函数都会成功, 同时更新 `eventmask` (与 `modify()`表现类似)

##### `poll.unregister(obj)`{:class="method"}

从轮询对象注销 `obj`


##### `poll.modify(obj, eventmask)`{:class="method"}

修改 `obj` 的 `eventmask`, 如果 `obj` 未注册, 则引发 `ENOENT`(`OSError`).

##### `poll.poll(timeout=-1)`{:class="method"}

等待至少一个已注册的对象准备就绪或触发异常条件. 超时时间可选, 单位:毫秒.（如果 未指定 或是 -1，则不会超时)

返回 一个 `("obj", "event", ...)` 元组列表. (元组中可能包含多余2个元素，取决于实现平台).

`event` - 指定在流`obj`上发生的事件，它是上述`uselect.POLL*`的常量组合.

注意: 事件`uselect.POLLHUP`和`uselect.POLLERR`可能随时（即使不要求）返回, 且必须采取相应措施(注销obj并适当地close), 因为后续调用`poll()`都会立即返回, 且仍然是这些事件.

超时的情况下，返回空列表.

> 与 _CPython_差异: 可能返回多余2个元素的元组

##### `poll.ipoll(timeout=-1, flags=0)`{:class="method"}


类似`poll.poll()`, 但返回一个产生 *被调用函数所有元组* 的迭代器, 该方法提供了一种高效无分配的 `流` 轮询方式.

如果`flags`置 `1`, 将使用事件的一次性行为:   

    发生事件后, 流的事件掩码将自动重置 (与 poll.modify(obj, 0) 等价), 因此，在未设置新掩码前，不会处理此流的任何事件.

此行为对于异步I/O调度器非常有用

>   该方法是 *MicroPython* 特有扩展

