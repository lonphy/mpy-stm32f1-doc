---
layout: post
title:  "pyb.Accel 类"
date:   2019-08-18 0:0:0 +0000
image: /assets/images/twoscreen.jpg
category: pyb
---

`pyb.Accel` 类 提供加速传感器(MMA7660)驱动

> 也就是重力传感器

示例:

```python
import pyb

accel = pyb.Accel()
for i in range(10):
    print(accel.x(), accel.y(), accel.z())

```
每个轴的值范围是[-32,31]


#### 构造方法

###### `pyb.Accel()`{:class="method"}

创建并返回`Accel`对象

> 注意: 如果创建`Accel`对象后立即读取加速器值，将会返回0, 因为加速器第一次采样大约需要20ms才能就绪,
> 所以, 在创建与获取值之间要么有其他代码， 要么插入延时

例如:
```python
import pyb

accel = pyb.Accel()
pyb.delay(20)
print(accel.x())
```

#### 实例方法

###### `pyb.Accel.filtered_xyz()`{:class="method"}

获取过滤后的3元组(x,y,z)

> 注意: 方法当前实现是4次采样求和，即前三次调用`filtered_xzy()`采样的值与本次采样的结果求和, 因此返回值是原始值的 4倍大小

###### `pyb.Accel.tilt()`{:class="method"}

获取`MMA7660`加速器的`tilt`寄存器值(即倾斜度)

###### `pyb.Accel.x()`{:class="method"}

直接获取x轴的值

###### `pyb.Accel.y()`{:class="method"}

直接获取y轴的值

###### `pyb.Accel.z()`{:class="method"}

直接获取z轴的值
