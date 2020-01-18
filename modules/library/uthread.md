---
layout: post
title:  "_thread 模块"
date:   2019-12-29 0:0:0 +0000
category: library
---

`_thread` 包可以提供多个任务同时运行的能力

```python
import _thread, utime

def runner1():
    print("runner id is %d"%(_thread.get_ident()))
    utime.sleep_ms(5000)

_thread.start_new_thread(runner1, ())

```


#### 模块方法

###### `_thread.start_new_thread(callable, (args...))`{:class="method"}

启动一个新的线程执行`callable`, 可指定参数元组

返回值: `None`


###### `_thread.stack_size([size])`{:class="method"}

设置线程堆栈默认大小, 并返回原有线程堆栈的大小
> 如果 `size` 未指定或值0， 则由系统决定(默认4096字节)
> 如果  `size` 小于默认大小(2048字节), 则按2048字节计算

返回值类型: `int`

###### `_thread.get_ident()`{:class="method"}

获取调用者所在线程ID

返回值类型: `int`


###### `_thread.exit()`{:class="method"}

退出当前线程, 主线程(`main`)调用无效

返回值: `None`


###### `_thread.allocate_lock()`{:class="method"}


#### `LockType`类

> 注意 `_thread.LockType` 实例 只能调用`_thread.allocate_lock()`获得

```python
import _thread

# 创建锁
lock = _thread.allocate_lock()

# 输出False, 表示未被锁定
print(lock.locked()) 

# 获取锁
# 正常情况， 返回True, 若已经被获取， 则返回False
print(lock.acquire())

# 输出True, 表示处于锁定状态
print(lock.locked())

# 一些需要用锁保护的逻辑

# 释放锁
lock.release()
```

###### `_thread.LockType.acquire([wait=0])`{:class="method"}

获取锁, 通过`wait`指示锁被占用时是否等待

- `wait`=`False` 时， 若锁被占用， 返回`False`
- 若获取成功， 返回`True`

###### `_thread.LockType.release()`{:class="method"}

释放已获取的锁, 若锁处于非锁定状态， 则`RuntimeError`会被`raise`


###### `_thread.LockType.locked()`{:class="method"}
返回锁当前的锁定状态
- `True`: 锁定
- `False`: 未被锁定


###### `_thread.LockType.__enter__()`{:class="method"}
`_thread.LockType.acquire` 的别名

###### `_thread.LockType.__exit__()`{:class="method"}
`_thread.LockType.release` 的别名

<br><br><br><br>
