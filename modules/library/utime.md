---
layout: post
title:  "MicroPython utime - 日期时间库"
date:   2019-08-18 0:0:0 +0000
category: library
---

该模块实现了 *CPython* *time* 模块的一个子集，如下所述. 更多信息参考 [time](https://docs.python.org/3.5/library/time.html#module-time)

*utime* 模块提供获取当前时间和日期、测量时间间隔和延迟的功能.


**时基/时间纪元**:  嵌入式中 从 2000-01-01 00:00:00 UTC 开始.

**维护实际的日历日期/时间**: 需要实时时钟(`RTC`)支持. 依赖于 `machine.RTC()` 对象. 
当前日历时间可以使用`machine.RTC().datetime(tuple)`函数设置，并通过以下方式维护:

* 通过备用电池(依赖于具体电路支持)

* 使用网络时间协议.

* 由用户在每次开机时手动设置

如果没有使用 *MicroPython* `RTC`维护实际的日历时间，那么下面需要参考当前绝对时间的函数行为可能与预期有所出入.


模块函数
=========

###### `utime.localtime([secs])`{:class="func"}

将以秒为单位的时间(从 *时间纪元开始*) 转换为8元组，其中包含:(年、月、日、时、分、秒、周、一年中第几天)

如果 *secs* 没有提供 或 为 `None`，则使用RTC的当前时间.

* 年份包括世纪 (例如 2014).

* 月: 1-12

* 日: 1-31

* 时: 0-23

* 分: 0-59

* 秒: 0-59

* 周:  0-6 表示 星期一到星期天

* 一年中第几天:  1-366

###### `utime.mktime()`{:class="func"}

这是 `localtime` 的逆函数. 它的参数是一个完整的8元组，按照`localtime`表示时间.
返回一个整数，该整数是自 2000年1月1日以来的秒数.

###### `utime.sleep(seconds)`{:class="func"}

休眠给定的秒数.
有些板子可能接受*seconds*作为浮点数，以休眠小数秒. 为了兼容性及准确性 请使用 `sleep_ms()` 及 `sleep_us()`

###### `utime.sleep_ms(ms)`{:class="func"}

延时 *ms* 毫秒, ms 值应该为正或0.

###### `utime.sleep_us(us)`{:class="func"}

延时 *us* 微秒, us 值应该为正或0.

###### `utime.ticks_ms()`{:class="func"}

返回一个带有任意参考点的递增毫秒计数器，该计数器在某个值之后绕回到0, (本页面内, 称之为环绕)

重置值没有显式公开, 但是为了简化说明，将其称为 `TICKS_MAX`.
那么周期是 `TICKS_PERIOD = TICKS_MAX + 1`. `TICKS_PERIOD` 是2的整次幂.

`ticks_ms()`, `ticks_us()`, `ticks_cpu()` 等函数都使用相同的周期值(为简单起见).

因此，这些函数将返回 [`0`, `TICKS_MAX`] 内的值，共 `TICKS_PERIOD` 个值.

> 注意，这里只使用非负值.

在大多数情况下，应该将这些函数的返回值视为不透明的. 它们唯一可用的操作是下面描述的`ticks_diff()`和`ticks_add()`函数.

> 注意: 直接对这些值进行标准的数学运算(+、-)或关系运算(<、<=、>、>=), 其结果无效.
执行数学运算，然后将结果作为参数传给 `ticks_diff()`或`ticks_add()`, 其结果也无意义.

###### `utime.ticks_us()`{:class="func"}

与上述`ticks_ms()`相同，单位: 微秒

###### `utime.ticks_cpu()`{:class="func"}

类似`ticks_ms()`和`ticks_us()`，但可能具有系统中最高的分辨率.

这通常是CPU时钟，所以本函数就这么命名. 但也不一定非得是CPU时钟，可以使用系统中其他可用的时钟源(如高分辨率计时器).【f103中就别想了】.

此函数准确的定时单元(分辨率)没有在`utime`模块级别指定.此函数用于非常精细的基准测试或非常紧密的实时循环.避免在可移植性代码中使用.


###### `utime.ticks_add(ticks, delta)`{:class="func"}

使用 给定值 *delta* 补偿 *ticks*, *delta* 可正可负.

给定一个 *ticks* 值，这个函数用于计算 向前/后偏移 *delta* 后的值. 结果同样遵循 *tick* 的 算术定义(参见`ticks_ms()`).

*ticks* 参数必须是调用`ticks_ms()`, `ticks_us()`, 或 `ticks_cpu()` 函数的结果(或者前一个`ticks_add()`的返回值).
但 *delta* 可以是任意整数或数字表达式.

`ticks_add()` 用于计算事件/任务的死线很有用. (注意:必须使用`ticks_diff()`函数来处理死线).

示例:
```py
# 找出100毫秒前的刻度值
print(time.ticks_add(time.ticks_ms(), -100))

# 计算死线时间, 并测试
deadline = time.ticks_add(time.ticks_ms(), 200)
while time.ticks_diff(deadline, time.ticks_ms()) > 0:
    do_a_little_of_something()

# 打印当前板子的 TICKS_MAX 值
print(time.ticks_add(0, -1))
```

###### `utime.ticks_diff(ticks1, ticks2)`{:class="func"}

测量从`ticks_ms()`,`ticks_us()`或`ticks_cpu()`返回的值之间的刻度差(一个可能环绕的带符号值).

参数的顺序与减法运算符相同: `ticks_diff(ticks1, ticks2)` 等效于 `ticks1 - ticks2`. 但`ticks_ms()`等函数可能返回环绕值，因此直接对它们使用减法将产生不正确的结果.

所以需要`ticks_diff()`，它实现了模块化(或者具体点，环)算术运算来产生正确结果，即使是环绕值(只要它们之间的距离不太遥远，见下面).

函数返回 [`-TICKS_PERIOD/2`, `TICKS_PERIOD/2-1`] 范围内的带符号数(这是带符号二进制整数的典型范围定义).

如果结果为负，则意味着 *ticks1* 出现的时间早于 *ticks2*. 否则，表示 *ticks1* 发生在 *ticks2* 之后,
只有当 *ticks1* 和 *ticks2* 相互之间的间隔不超过`TICKS_PERIOD/2-1`时成立， 否则结果不正确.

具体来说, 如果两个 *tick* 值相距 `TICKS_PERIOD/2-1`, 那么函数将返回`TICKS_PERIOD/2-1`. 但是，如果它们相距`TICKS_PERIOD/2`的话， 将返回`-TICKS_PERIOD/2`，即结果值将环绕到可能值的负区间.

上述限制的非正式理由:

假设你被锁在一个房间里，除了一个标准的12刻度时钟外，没有其他方法来监控时间的流逝. 然后, 如果你现在看一下转盘，13小时后再看(例如睡了一觉), 你可能会觉得只过去了一小时.
为了避免这个错误，只要定期看表就可以了。您的应用程序也应该这样做: 不要让应用程序长时间运行单个任务. 应分步运行任务, 且在步骤之间计时.

`ticks_diff()` 设计用于以下的各种模式:

* **带超时轮询**. 在这种情况下，事件的顺序是已知的，您将只处理`ticks_diff()`的正结果:

```py
# Wait for GPIO pin to be asserted, but at most 500us
start = time.ticks_us()
while pin.value() == 0:
    if time.ticks_diff(time.ticks_us(), start) > 500:
        raise TimeoutError
```

* **调度事件**. 在这种情况下，`ticks_diff()`的结果可能是负数，例如一个过期事件:

```py
# This code snippet is not optimized
now = time.ticks_ms()
scheduled_time = task.scheduled_time()
if ticks_diff(scheduled_time, now) > 0:
    print("Too early, let's nap")
    sleep_ms(ticks_diff(scheduled_time, now))
    task.run()
elif ticks_diff(scheduled_time, now) == 0:
    print("Right at time!")
    task.run()
elif ticks_diff(scheduled_time, now) < 0:
    print("Oops, running late, tell task to run faster!")
    task.run(run_faster=true)
```

> 注意: 不要将`time()`值传递给`ticks_diff()`，应该对`time()`值使用常规的数学运算.  但是请注意，`time()` 可能(也会)溢出[参考](https://en.wikipedia.org/wiki/Year_2038_problem)

###### `utime.time()`{:class="func"}

返回从纪元开始的秒数，假设底层的RTC已按上面的方式设置和维护.

如果未设置RTC，此函数将返回自特定参考点(对于没有电池支持的RTC的嵌入式板，通常是在电源启动或复位后)以来的秒数.

如果需要更高的精度，可以使用`ticks_ms()`和`ticks_us()`函数，如果需要日历时间，则不带参数的`localtime()`是更好的选择.

> 与 *CPython* 差异: 在 *CPython* 里, 这个方法返回 自 *1970-01-01 00:00 UTC* 以来的秒数即 Unix 时间戳(浮点数), 通常有微秒级精度.
但在 *MicroPython* 里, 仅 *Unix* 下使用同样的纪元, 同时如果支持浮点数, 则返回次秒级精度. 
嵌入式硬件通常没有浮点精度来表示长时间范围和次秒级精度，因此使用的是具有秒级精度的整数值.
一些嵌入式硬件也缺乏电池供电的RTC，因此返回上次开机后的秒数或从其他硬件相关的特定点(如复位)返回的秒数.
