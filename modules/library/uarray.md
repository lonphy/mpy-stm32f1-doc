---
layout: post
title:  "MicroPython uarray - 数字数组"
date:   2019-08-18 0:0:0 +0000
category: library
---

支持的格式编码:

|编码 |python类型 | 对应C类型              | 大小(字节) | 说明
|--  |--        |--                     |--         |--
|`b` | `int`    | `signed char`         | 1
|`B` | `int`    | `unsigned char`       | 1
|`h` | `int`    | `signed short`        | 2
|`H` | `int`    | `unsigned short`      | 2
|`i` | `int`    | `signed int`          | 4
|`I` | `int`    | `unsigned int`        | 4
|`l` | `int`    | `signed long`         | 4
|`L` | `int`    | `unsigned long`       | 4
|`q` | `float`  | `signed long long`    | 8
|`Q` | `float`  | `unsigned long long`  | 8
|`f` | `float`  | `float`               | 4
|`d` | `float`  | `double`              | 8         | stm32f103不支持


#### array 构造方法

###### `uarray.array(typecode[, iterable])`{:class="method"}

使用给定的`typecode`(元素类型)， 创建数组。

初始数组内容由 *iterable* 提供, 如果为空， 则会创建一个空数组

#### array 类实例方法

###### `array.append(val)`{:class="method"}

追加 `val` 到 数组末尾

###### `array.extend(iterable)`{:class="method"}

将 `iterable` 包含的数据 追加到数组末尾


示例:

```python
from uarray import array

# 无符号字符数组
array.array('B', [1, 2, 3, 4])

# 无符号整型数组
array.array('I', [1, 2, 3])

```

<br>
<br>