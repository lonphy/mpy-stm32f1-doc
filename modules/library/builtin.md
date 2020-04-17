---
layout: post
title:  "MicroPython 内置方法及异常"
date:   2019-08-18 0:0:0 +0000
category: library
---

内置函数和异常
---

这里描述所有的内置函数和异常。也可用`builtins`包来使用


函数及类型
===================

###### `abs()`{:class="func"}

返回数字的绝对值


###### `all(iterable)`{:class="func"}

判断给定的可迭代参数 *iterable* 中的所有元素是否都为 `True`, 如果是返回 `True`, 否则返回 `False`

元素除了是 `0`, `''`, `None`, `False` 其他算`True`


###### `any(iterable)`{:class="func"}

判断给定的可迭代参数 *iterable* 中的所有元素是否都为 `False`, 如果是返回 `False`, 否则返回 `True`

元素除了是 `0`, `''`, `None`, `False` 其他算`True`


###### `bin()`{:class="func"}

返回一个整数 `int` 或者长整数 `long int` 的二进制表示

###### `callable(obj)`{:class="func"}

检查一个对象是否是可调用. 如果返回 `True`, obj 可能调用失败；但如果返回 `False`，调用对象 obj 绝对不会成功

对于 *函数*, *方法*, *lambda表达式*, *类* 以及实现了 **call** 方法的类实例, 它都返回 `True` 

###### `chr(val)`{:class="func"}

返回 *val* 表示的 assic 字符, *val* 有效值[0, 255]

###### `classmethod()`{:class="func"}
装饰器, 指示类方法


###### `compile()`{:class="func"}
将一个字符串编译为字节码

###### `dir([obj])`{:class="func"}
- 不带参数: 返回当前作用域内的变量、方法和定义的类型列表
- 带参数时: 返回参数的属性、方法列表, 如果obj提供了`**dir**()`方法, 也会被调用

###### `divmod(a, b)`{:class="func"}

把除数和余数运算结果结合起来，返回一个包含商和余数的元组`(a // b, a % b)`

###### `enumerate()`{:class="func"}
将一个可遍历的数据对象(如`list`, `tuple`或`string`)组合为一个索引序列，同时列出数据和数据下标，一般用在 **for** 循环中


###### `eval()`{:class="func"}
来执行一个字符串表达式，并返回表达式的值

###### `exec()`{:class="func"}
来执行一个字符串表达式，并返回表达式的值

###### `execfile()`{:class="func"}
来执行一个文件，并返回表达式的值

###### `filter(fn, iter)`{:class="func"}
过滤序列，过滤掉不符合条件的元素( *fn* 返回`False`)，返回由符合条件元素组成的新列表的迭代器

- `fn` -- 过滤函数
- `iter` -- 可迭代对象

###### `globals()`{:class="func"}
以字典类型返回全部全局变量

###### `hash(obj)`{:class="func"}
获取取一个对象(`obj`)（字符串或者数值等）的哈希值

###### `hex(x)`{:class="func"}
将10进制整数`x`转换成16进制，以字符串形式表示

###### `id(obj)`{:class="func"}
返回对象`obj`的内存地址

###### `input([prompt])`{:class="func"}
接受一个标准输入数据，返回 string 类型

###### `isinstance(obj, cls)`{:class="func"}
判断对象`obj`是否是`cls`的实例

###### `issubclass(cls, pCls)`{:class="func"}
判断对象`cls`是否是`pCls`的子类

###### `iter(obj)`{:class="func"}
生成`obj`的迭代器

###### `len(obj)`{:class="func"}
返回对象`obj`（字符、列表、元组等）长度或元素个数


###### `locals()`{:class="func"}
以字典类型返回当前作用域的全部局部变量

###### `map(fn, iter,...)`{:class="func"}
根据提供的函数对指定序列做映射

以`iter`中的每个元素都调用`fn`, 收集`fn`处理后的结果， 并返回迭代器

###### `max(n, n1, ...)`{:class="func"}
返回所有参数中的最大值

**n, n1, ...** 是数值表达式

###### `min(n, n1, ...)`{:class="func"}
返回所有参数中的最小值

**n, n1, ...** 是数值表达式

###### `next(iter)`{:class="func"}
获取迭代器 *iter* 的下一个元素

###### `oct(n)`{:class="func"}
将一个整数 *n* 转换成8进制字符串

###### `open(name [, mode[, buffering]])`{:class="func"}
打开一个文件，创建一个 `file` 对象

- **name** -- 文件路径/名称
- **mode** -- 打开文件的模式：只读，写入，追加等， 默认只读
- **buffering** -- 缓冲大小:
    - `0` - 无缓冲
    - `1` - 行缓冲
    - `>1` - 指定缓冲大小
    - `-1` - 缓冲大小取系统默认


###### `ord(c)`{:class="func"}
`chr()` 的反向函数, 返回字符`c`的asiic码值

###### `pow(x, y[, z])`{:class="func"}
计算x的y次方，如果z在存在，则再对结果进行取模，其结果等效于pow(x,y) %z

###### `print(*objects, sep=' ', end='\n', file=sys.stdout, flush=False)`{:class="func"}
用于打印输出

- `objects` -- 可以一次输出多个对象。输出多个对象时，需要用 , 分隔
- `sep` -- 用来间隔多个对象，默认值是一个空格
- `end` -- 用来设定以什么结尾。默认值是换行符 `\n`
- `file` -- 要写入的文件对象
- `flush` -- 输出是否被缓存通常决定于 file，但如果 `flush` 关键字参数为 True，流会被强制刷新


###### `property()`{:class="func"}
在新式类中返回属性值

###### `range(start, stop[, step])`{:class="func"}
创建一个整数列表

- `start`: 计数从 `start` 开始。默认:0. 例如range（3）等价于range（0, 3）;
- `stop`: 计数到 `stop` 结束, 不包括`stop`. 例如: range（0, 3） 是[0, 1, 2]没有3
- `step`: 步长, 默认1. 例如：range（0， 3） 等价于 range(0, 5, 1)

###### `repr()`{:class="func"}
将对象转化为供解释器读取的形式(返回一个对象的 string 格式)

###### `reversed()`{:class="func"}
经列表元素按反向顺序排列

###### `round(x)`{:class="func"}
对数值`x`进行四舍五入


###### `delattr(obj, name)`{:class="func"}
删除*obj*的*name*属性, 相当于 `del obj.name`.

###### `getattr(obj, name [, default])`{:class="func"}
返回一个对象(`obj`)的属性(`name`)值
> `default`, 指定默认返回值，如果不提供，当`obj`没有属性`name`时，将触发 `AttributeError`

###### `hasattr(obj, name)`{:class="func"}
判断对象(`obj`)是否包含属性`name`

###### `setattr(obj, name, value)`{:class="func"}
设置对象`obj`属性`name`的值为`value`

###### `sorted(iter, cmp=None, key=None, reverse=False)`{:class="func"}
对所有可迭代的对象进行排序操作

- `iter` -- 可迭代对象
- `cmp` -- 比较的函数，两个入参，参数的值都是从可迭代对象中取出，此函数必须遵守的规则为，大于则返回1，小于则返回-1，等于则返回0
- `key` -- 用于进行比较的元素，只有一个参数，具体的函数的参数就是取自于可迭代对象中，指定可迭代对象中的一个元素来进行排序
- `reverse` -- 排序规则, `True` 降序, `False` 升序（默认）


###### `staticmethod()`{:class="func"}
装饰器, 返回函数的静态方法

###### `sum(iter[, start])`{:class="func"}
对可迭代对象`iter`进行求和计算, `start` 指定初始值， 默认为0

###### `super()`{:class="func"}
用于调用父类(超类)的一个方法


###### `type(obj)`{:class="func"}
返回`obj`的类型

###### `type(name, bases, dict)`{:class="func"}
创建一个新的类型
- `name` -- 类的名称
- `bases` -- 基类的元组
- `dict` -- 类内定义的命名空间变量


###### `zip([iter,...])`{:class="func"}
将可迭代对象序列中的每个迭代对象的元素打包成一个个元组，然后返回由这些元组组成的列表对象, 列表长度是可迭代对象序列中最小的那个.
```python
t1 = [1,2,3]
t2 = [4,5,6,7]
print(list(zip(t1, t2)))
# 以上输出
# [(1, 4), (2, 5), (3, 6)]
```

##### `bool([x])`{:class="class"}
将给定参数`x`转换为布尔类型，如果没有参数，返回`False`

##### `bytearray([source[, encoding[, errors]]])`{:class="class"}
创建一个可变的字节数组

分以下情况:

- 如果 `source` 是 整数，则返回一个长度为 `source` 的 `bytearray`
- 如果 `source` 是 字符串，则按照指定的 `encoding` 将字符串转换为字节序列
- 如果 `source` 是 可迭代类型，则元素必须为[0 ,255] 中的整数
- 如果 `source` 是 具有buffer接口的对象，则此对象也可以被用于初始化 `bytearray`
- 如果没有输入任何参数，返回长度是0的`bytearray`


##### `bytes([source[, encoding[, errors]]])`{:class="class"}
创建一个`bytes`对象, 参数同`bytearray`, 但内容创建后不可变

##### `complex(real[, imag])`{:class="class"}
创建一个值为 `real + imag * j` 的复数或者转化一个字符串或数为复数


##### `dict()`{:class="class"}
创建一个字典

##### `float()`{:class="class"}
将整数和字符串转换成浮点数

##### `frozenset([iter])`{:class="class"}
返回一个冻结的集合，冻结后集合不能再添加或删除任何元素

##### `int(x[,base])`{:class="class"}
将一个字符串或数字转换为整型, 当`x`是字符串时, 可指定`base`, 默认十进制

###### `from_bytes(bytes, byteorder)`{:class="method"}

返回由给定字节数组`bytes`表示的整数。 `byteorder` 可以是`big`或`little`(与CPython不兼容)

###### `to_bytes(size, byteorder)`{:class="method"}
返回整数的字节数组表示,  `byteorder` 可以是`big`或`little`(与CPython不兼容)

##### `list()`{:class="class"}
将元组转换为列表

##### `memoryview()`{:class="class"}
返回给定参数的内存查看对象

##### `object()`{:class="class"}

##### `set([iter])`{:class="class"}
创建一个无序不重复元素集

##### `slice(start, stop[, step])`{:class="class"}
返回一个切片对象

##### `str(obj)`{:class="class"}
返回`obj`的`string`格式

##### `tuple()`{:class="class"}
将 列表转换为元组

异常及错误
==========

- `AssertionError` - 断言语句失败

- `AttributeError` - 属性错误

- `Exception` - 常规错误的基类

- `ImportError` - 导入模块/对象失败

- `IndexError` - 序列中没有此索引(index)

- `KeyboardInterrupt` - 用户中断执行(通常是输入^C)

- `KeyError` - 映射中没有这个键

- `MemoryError` - 内存溢出错误

- `NameError` - 未声明/初始化对象 (没有属性)

- `NotImplementedError` - 尚未实现错误

- `OSError` - 操作系统错误

- `RuntimeError` - 常规运行时错误

- `StopIteration` - 迭代器没有更多的值

- `SyntaxError` - Python 语法错误

- `SystemExit` - 解释器请求退出

- `TypeError` - 对类型无效的操作

- `ValueError` - 传入无效的参数

- `ZeroDivisionError` - 除(或取模)零 (所有数据类型)
