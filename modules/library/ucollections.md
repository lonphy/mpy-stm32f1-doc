---
layout: post
title:  "MicroPython ucollections - 集合及容器类型"
date:   2019-08-18 0:0:0 +0000
category: library
---

这个模块实现了高级的集合和容器类型来保存/聚集各种对象


##### `ucollections.deque(iterable, maxlen[, flags])`{:class="class"}
Deques(双端队列)是一个类似列表的容器，它支持以O(1)复杂度从队列的任意一端入队和出队, 使用以下参数创建:

- `iterable` 必须是一个空`tuple`, 用于创建新队列

- `maxlen` 必须提供， 指定队列的最大容量. 一旦队列满了，任何添加的新元素都会从另一端丢弃.

- `flags` 可选参数, 设为1启用在添加元素时进行溢出检查

支持以下方法:

###### `deque.append(x)`{:class="method"}
添加 `x` 到队列右侧. 如果打开了溢出检查, 当队列满后引发`IndexError`

###### `deque.popleft()`{:class="method"}
从队列左侧删除并返回一个元素. 如果对列为空引发`IndexError`


##### `ucollections.namedtuple(name, fields)`{:class="class"}

这是一个工厂函数，用于创建具有特定名称和字段的新 `namedtuple` 类型.

`namedtuple` 是 `tuple` 的子类, 它可以不仅使用数字索引访问其字段，还可以用字段名符号访问. 字段是字符串序列指定的名称. 

为了与 _CPython_ 兼容, 它也可以是空格分隔的命名字段(效率低一些). 使用示例:
```python
from ucollections import namedtuple

MyTuple = namedtuple("MyTuple", ("id", "name"))
t1 = MyTuple(1, "foo")
t2 = MyTuple(2, "bar")
print(t1.name)
assert t2.name == t2[1]
```

##### `ucollections.OrderedDict(...)`{:class="class"}

`dict` 类型的子类, 它可以记住添加的键顺序. 当遍历有序 _dict_ 时, 会按添加的顺序依次返回:

```python
from ucollections import OrderedDict

# 为了使用有序字典，OrderedDict应该用(键、值)对序列来初始化
d = OrderedDict([("z", 1), ("a", 2)])

# 更多的元素可以正常添加
d["w"] = 5
d["b"] = 3
for k, v in d.items():
    print(k, v)
```

输出:

    z 1
    a 2
    w 5
    b 3
