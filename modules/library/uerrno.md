---
layout: post
title:  "MicroPython uerrno - 系统错误码"
date:   2019-08-18 0:0:0 +0000
category: library
---

此模块提供了 `OSError` 的错误描述访问

常量
=========

- `EEXIST`, `EAGAIN`, ...

    错误码, 基于 _ANSI C/POSIX_ 标准. 所有错误码以 __E__开头.
    
    如上所述，错误码集合取决于 _MicroPython_ 平台, 错误通常可以通过 `exc.args[0]`来访问,
    其中`exc`是`OSError`的实例, 示例:

    ```python
    try:
        uos.mkdir("my_dir")
    except OSError as exc:
        if exc.args[0] == uerrno.EEXIST:
            print("Directory already exists")
    ```

- `uerrno.errorcode`
    错误码字符串字典, 键是错误码(参考上面):

    ```
    >>> print(uerrno.errorcode[uerrno.EEXIST])
    EEXIST
    ```
