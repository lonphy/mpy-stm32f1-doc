---
layout: post
title:  "MicroPython cmath - 复数数学库"
date:   2019-08-18 0:0:0 +0000
category: library
---

`cmath` 库 为复数提供一些基本数学函数.

方法
=========

###### `cmath.cos(z)`{:class="func"}

返回 `z` 的余弦值

###### `cmath.exp(z)`{:class="func"}

返回 `z` 的指数


###### `cmath.log(z)`{:class="func"}

返回 `z` 的自然对数, 沿负实轴分支切割

###### `cmath.log10(z)`{:class="func"}

返回 `z` 以10为底的对数, 沿负实轴分支切割

###### `cmath.phase(z)`{:class="func"}

返回 `z` 在范围(-pi， +pi)内的相位


###### `cmath.polar(z)`{:class="func"}

以 `tuple` 返回 `z` 的极坐标形式


###### `cmath.rect(r, phi)`{:class="func"}

返回 模为`r`，相位为 `phi` 的复数


###### `cmath.sin(z)`{:class="func"}

返回 `z` 的正弦值

###### `cmath.sqrt(z)`{:class="func"}

返回 `z` 的平方根


常量
=========

- `cmath.e`

    自然对数的底数

- `cmath.pi`

    圆周率
