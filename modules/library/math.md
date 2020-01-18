---
layout: post
title:  "MicroPython math - 数学库"
date:   2019-08-18 0:0:0 +0000
category: library
---

`math` 模块提供了一些基础数学函数处理浮点数

> 注意: 在 _pyboard_ 上, 浮点数是32位精度


方法
=========

###### `math.acos(x)`{:class="func"}

返回 `x`的反余弦值

###### `math.acosh(x)`{:class="func"}

返回 `x` 的 反双曲余弦值

###### `math.asin(x)`{:class="func"}

返回 `x`的反正弦值

###### `math.asinh(x)`{:class="func"}

返回 `x` 的 反双曲正弦值

###### `math.atan(x)`{:class="func"}

返回 `x`的反正切值

###### `math.atan2(y, x)`{:class="func"}

返回 `y/x`的反正切主值

###### `math.atanh(x)`{:class="func"}

返回 `x`的反双曲正切值


###### `math.ceil(x)`{:class="func"}

返回 `x` 的向上舍入整数

###### `math.copysign(x, y)`{:class="func"}

返回 以 `y` 的符号处理后的 `x` 值

###### `math.cos(x)`{:class="func"}

返回 `x` 的余弦值

###### `math.cosh(x)`{:class="func"}

返回 `x` 的双曲余弦值


###### `math.degrees(x)`{:class="func"}

返回 弧度 `x` 的角度值


###### `math.erf(x)`{:class="func"}

返回 `x` 的误差值


###### `math.erfc(x)`{:class="func"}

返回 `x` 的余误差


###### `math.exp(x)`{:class="func"}

返回 `x` 的指数

###### `math.expm1(x)`{:class="func"}

返回 `exp(x) - 1`

###### `math.fabs(x)`{:class="func"}

返回 `x` 的绝对值


###### `math.floor(x)`{:class="func"}

返回 `x` 的向下舍入整数


###### `math.fmod(x, y)`{:class="func"}

返回 `x/y` 的余数


###### `math.frexp(x)`{:class="func"}

将浮点数分解为尾数和指数

返回的值是 `(m, e)`, 解压为: `x == m * 2**e`

如果`x == 0`，则返回 `(0.0, 0)`，否则关系 `0.5 <= abs(m) < 1`成立

###### `math.gamma(x)`{:class="func"}

返回 `x` 的伽玛函数

###### `math.isfinite(x)`{:class="func"}

判断 `x` 是否是有限数


###### `math.isinf(x)`{:class="func"}

判断 `x` 是否是无限数

###### `math.isnan(x)`{:class="func"}

`x` 是 `NaN` 返回 `True`


###### `math.ldexp(x, exp)`{:class="func"}

返回 `x * (2**exp)`

###### `math.lgamma(x)`{:class="func"}

返回 `x` 伽马函数的自然对数

###### `math.log(x)`{:class="func"}

返回 `x` 的自然对数

###### `math.log10(x)`{:class="func"}

返回 `x` 以10为底的对数

###### `math.log2(x)`{:class="func"}

返回 `x` 以2为底的对数


###### `math.modf(x)`{:class="func"}

返回 `x` 的整数和小数部分, 两个浮点数的元组, 2个值得符号与 `x` 一致


###### `math.pow(x, y)`{:class="func"}

返回 `x` 的 `y` 次方

###### `math.radians(x)`{:class="func"}

返回 角度 `x` 对应的弧度

###### `math.sin(x)`{:class="func"}

返回 `x` 的正弦值

###### `math.sinh(x)`{:class="func"}

返回 `x` 的双曲正弦值

###### `math.sqrt(x)`{:class="func"}

返回 `x` 的平方根

###### `math.tan(x)`{:class="func"}

返回 `x` 的正切值

###### `math.tanh(x)`{:class="func"}

返回 `x` 的双曲正切值

###### `math.trunc(x)`{:class="func"}

返回 `x` 的整数部分


常量
=========

- `math.e` - 自然对数的底数

- `math.pi` - 圆周率
