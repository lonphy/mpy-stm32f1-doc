---
layout: post
title:  "MicroPython uos - 基础操作系统服务"
date:   2019-08-18 0:0:0 +0000
category: library
---

`uos`模块包含文件系统访问/挂载、终端重定向/复制以及`uname`和`urandom`函数

通用函数
=================

###### `uos.uname()`{:class="func"}
返回一个`tuple`(可能是一个命名`tuple`)，其中包含关于底层机器和/或其操作系统的信息, 有五个字段，它们的顺序如下:

- `sysname` -- 底层系统的名称

- `nodename` -- 网络名称 (可与 `sysname` 相同)

- `release` -- 底层系统版本

- `version` -- _MicroPython_ 版本和构建日期

- `machine` -- 底层硬件标识(例如 板名, CPU)

###### `uos.urandom(n)`{:class="func"}

返回一个具有 `n`字节长度的 随机内容bytes对象。如果可能，会使用硬件随机数生成器生成


文件系统访问
=================

###### `uos.chdir(path)`{:class="func"}
改变当前目录

###### `uos.getcwd()`{:class="func"}
获取当前目录

###### `uos.ilistdir([dir])`{:class="func"}

该函数返回一个迭代器，会生成与它所列出的目录中的条目对应的元组.
无参调用列出当前目录, 否则列出`dir`目录.

元组形式是 `(name, type, inode[, size])` :

- `name` 字符串(当`dir`是`bytes`对象时则是`bytes`) 文件/目录名称

- `type` 是一个标识条目类型的整数，目录: `0x4000`，常规文件: `0x8000`

- `inode` 是与文件的inode对应的整数, 对于无`inode`概念的文件系统是0

- 一些平台上会返回4元素`tuple`, 最后一位是 `size`, 对于 _文件_, `size`表示文件大小, 对于 _目录_ 则是未定义


###### `uos.listdir([dir])`{:class="func"}

无参调用列出当前目录， 其他情况列出`dir`对应目录

###### `uos.mkdir(path)`{:class="func"}

新建目录

###### `uos.remove(path)`{:class="func"}
删除一个文件

###### `uos.rmdir(path)`{:class="func"}
删除一个目录

###### `uos.rename(old_path, new_path)`{:class="func"}
重命名一个文件

###### `uos.stat(path)`{:class="func"}

获取文件/目录的状态

###### `uos.statvfs(path)`{:class="func"}
获取一个文件系统的状态, 按以下顺序, 用`tuple`返回文件系统信息:

* `f_bsize` -- 文件系统块大小

* `f_frsize` -- 片段大小

* `f_blocks` -- 以`f_frsize`为单位的文件系统块数量

* `f_bfree` -- 可用块大小

* `f_bavail` -- 为非特权用户提供的空闲块数量

* `f_files` -- inodes 数量

* `f_ffree` -- 可用 inodes 数量

* `f_favail` -- 非特权用户空闲 inodes 数量

* `f_flag` -- 挂载标志

* `f_namemax` -- 最大文件名长度

inodes 相关参数: 如果具体平台不支持, `f_files`, `f_ffree`, `f_avail` 和 `f_flags` 参数可能返回 `0`

###### `uos.sync()`{:class="func"}

同步所有文件系统


终端重定向和复制
====================================

###### `uos.dupterm(stream_object, index=0)`{:class="func"}

在给定的 _类流_ 对象上复制或切换 _MicroPython_ 终端(`REPL)`.
`stream_object`参数必须是一个原生流对象, 或派生自`uio.IOBase` 实现了 `readinto()`和`write()`方法.
流应该是非阻塞式，如果无可读数据，`readinto()` 应该返回 `None`.

调用此函数后，所有终端输出将在此流上重复，流上任何可用输入都将传递到终端输入.

`index` 参数是一个非负整数，指定复制槽. 一个具体实现平台可以实现多个槽(0槽总是可用的), 这种情况下，终端的输入和输出在所有设置的槽上都起作用.

如果 `stream_object` 传入 `None`, 那么 `index` 指定的复制槽将被取消

函数返回给定槽上上一个指定的 _类流_ 对象


文件系统挂载
===================

一些实现平台提供了一个虚拟文件系统(VFS)、允许在VFS中挂载多个 __真实__ 文件系统的能力.

文件系统对象可以挂载到VFS的根目录, 也可以挂载到任一子目录.

这使得动态及灵活的文件系统配置堆Python程序可见, 具有此功能的实现平台提供`mount()`和`umount()`函数, 以及由VFS表示的各种文件系统实现
类.

###### `uos.mount(fsobj, mount_point, *, readonly)`{:class="func"}

将文件系统对象 `fsobj` 挂载到 VFS 中 由 `mount_point` 给出的位置. `fsobj` 是个有`mount()`方法的VFS 对象, 或一个块设备.

如果是个 块设备, 将会自动检测文件系统类型(无法识别文件系统时将引发一个异常). 

如果 `readonly` 是 `True` 会被挂载为只读文件系统

在挂载过程中，会调用文件系统对象的`mount()`方法

如果 `mount_point` 已被挂载，将引发 `OSError(EPERM)`

###### `uos.umount(mount_point)`{:class="func"}

卸载文件系统。`mount_point` 可以是挂载目录名称, 也可以是已挂载的文件系统对象. 在卸载过程中，调用文件系统对象的 `umount()` 方法

如果`mount_point` 未找到，将引发 `OSError(EINVAL)`


##### `uos.VfsFat(block_dev)`{:class="class"}

创建一个 _FAT格式_ 的文件系统对象. 
FAT文件系统的存储由 `block_dev` 提供. 这个构造函数创建的对象可以使用`mount()`进行挂载

###### `static mkfs(block_dev)`{:class="method"}
在块设备 `block_dev` 上创建 _FAT_ 文件系统


块设备
=============

一个 __块设备__ 是实现了 __块协议__ 的对象, 即`AbstractBlockDev`类描述的方法集.
此类的具体实现通常允许访问类似内存功能的硬件(如闪存).
一个特定文件系统驱动可以使用一个块设备来存储其文件系统的数据.

##### `uos.AbstractBlockDev(...)`{:class="class"}
构造一个块设备对象. 构造函数的参数依赖于特定的块设备

###### `readblocks(block_num, buf)`{:class="method"}

从 `block_num` 指定的块开始, 将数据从设备读入 `buf` (字节数组). 读取的块数量由 `buf` 的长度决定, 它是块大小的整数倍

###### `writeblocks(block_num, buf)`{:class="method"}

从 `block_num` 指定的块开始, 将 `buf`(字节数组) 写入设备. 写入的块的数量由 `buf` 的长度决定，它是块大小的整数倍


###### `ioctl(op, arg)`{:class="method"}

控制块设备及参数查询. 操作执行由以下`op`中的一个指定:

- `1` -- 初始化设备 (`arg`未用)

- `2` -- 关闭设备 (`arg`未用)

- `3` -- 同步设备 (`arg`未用)

- `4` -- 获取块数量, 需要返回一个整数 (`arg`未用)

- `5` -- 获取单块的字节大小, 需要返回一个整数, 或 `None` 表示默认值 __512__ (`arg`未用)

作为示例，下面的类实现了一个块设备，该设备使用 `bytearray` 将数据存储在RAM中:

```python
class RAMBlockDev:
    def __init__(self, block_size, num_blocks):
        self.block_size = block_size
        self.data = bytearray(block_size * num_blocks)

    def readblocks(self, block_num, buf):
        for i in range(len(buf)):
            buf[i] = self.data[block_num * self.block_size + i]

    def writeblocks(self, block_num, buf):
        for i in range(len(buf)):
            self.data[block_num * self.block_size + i] = buf[i]

    def ioctl(self, op, arg):
        if op == 4: # get number of blocks
            return len(self.data) // self.block_size
        if op == 5: # get block size
            return self.block_size
```

可以这样使用:

```python
import uos

bdev = RAMBlockDev(512, 50)
uos.VfsFat.mkfs(bdev)
vfs = uos.VfsFat(bdev)
uos.mount(vfs, '/ramdisk')
```
