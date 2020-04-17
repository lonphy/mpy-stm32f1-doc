---
layout: post
title:  "MicroPython sys - 系统特定功能库"
date:   2019-08-18 0:0:0 +0000
category: library
---

###### `sys.exit(retval=0)`{:class="func"}

使用给定退出代码终止当前程序. 基本上，这个函数会引发`SystemExi`异常。如果提供参数，它将作为“SystemExit”的参数


###### `sys.atexit(func)`{:class="func"}

注册在终止时的会掉函数。`func` 必须是不带参的，或者 `None`来禁用调用。

`atexit`将返回此函数设置的前一个值，初始值是 `None`.

> 与 *CPython* 差异: 这是一个 *MicroPython* 扩展，旨在提供类似于 *CPython* 中的 `atexit` 模块功能


###### `sys.print_exception(exc, file=sys.stdout)`{:class="func"}

打印异常并携带栈信息到一个类文件对象 `file`(默认是`sys.stdout`)

> 与 *CPython* 差异: 
> 该函数是 *CPython* 的 `traceback` 模块中同名函数简化实现, 与 `traceback.print_exception()` 不同:
> - 它只接受异常值, 而不是异常类型/值/回溯对象
> - `file` 参数是位置参数
> - 不支持*further* 参数
>
> 与 *CPython* 兼容的 `traceback` 模块可以在 **micropython-lib** 中找到.


库常量
=========

- `sys.argv`

    当前程序启动时使用的可变参数列表, 一般是空列表

- `sys.byteorder`

    系统字节序(`little` 或 `big`)

- `sys.implementation`

    包含 *Python* 实现信息的对象, 对于 *MicroPython*, 包含以下属性:

    * *name* - 字符串 *micropython*

    * *version* - 版本: *tuple (major, minor, micro)*, 例如 *(1, 12, 0)*

    这个对象是区分 *MicroPython* 与 *其他Python* 实现的推荐方法(注意，可能在最小平台端口种不提供)

    > 与 *CPython* 差异: *CPython* 在这个对象上提供了很多属性, 但 *MicroPython* 仅实现了最有用的部分.

- `sys.maxsize`

    当前平台可使用的原生最大数值, 或 *MicroPython* `int` 类型可提供的最大值 <small>(当它小于原生最大数值时， 意味着 *MicroPython* 不支持 `long int`)</small>

    这个属性对于检测平台字长非常有用(例如: 32位/64位). 建议不要直接将此属性与某个值进行比较，而是计算其位数量:
    ```python
    bits = 0
    v = sys.maxsize
    while v:
        bits += 1
        v >>= 1
    if bits > 32:
        # 64位平台(或更大)
        ...
    else:
        # 32位平台(或更少)
        # 注意在32位平台上, 由于上述特性 sys.maxsize 的bit数也许小于32(例如31), 所以 使用 > 16, > 32, > 64 格式的比较
    ```

- `sys.modules`

    库导入路径. 在一些硬件平台上可能不包含内置路径

- `sys.path`

    一个可变的目录列表，用于搜索导入的模块.

- `sys.platform`

    *MicroPython* 运行平台, 对于 *OS/RTOS* 环境, 通常是一个 *OS* 的标识符, 例如 *linux*.
    对于裸机板(不包含操作系统)通常是板子的标识, 例如 *pyboard* 来标识原生参考板.
    因此它可用于区分不同的板子. 如需检查程序运行在MicroPython的哪种板子上, 请使用 `sys.implementation`.

- `sys.stderr`

    标准错误流

- `sys.stdin`

    标准输入流

- `sys.stdout`

    标准输出流

- `sys.version`

    实现遵循的 *Python* 语言版本， 一个字符串 *"3.4.0"*

- `sys.version_info`

    实现遵循的 *Python* 语言版本， 一个整数元组。

    > 与 *CPython* 差异: 仅返回前三部分版本(**major**, **minor**, **micro**), 使用数字索引
