---
layout: post
title:  "pyb.Switch 类"
date:   2019-08-18 0:0:0 +0000
image: /assets/images/twoscreen.jpg
category: pyb
---

按键开关(pyb.Switch)

Switch单例 用于控制一个按钮开关
示例:

```python
import pyb

sw = pyb.Switch()       # 创建开关实例
sw()                    # 获取状态(True -按下, False -其他)
sw.callback(f)          # 注册一个回调, 当开关按下时会被调用
sw.callback(None)       # 移除回调

pyb.Switch().callback(lambda: pyb.LED(1).toggle())
```

#### 构造方法

###### `pyb.Switch()`{:class="method"}

说明: 返回一个预定义的Switch实例


#### 实例方法

###### `pyb.Switch.__call__()`{:class="method"}

直接调用Switch对象,获取开关状态：True-按下, False-未按下


###### `pyb.Switch.value()`{:class="method"}
     
获取开关状态：True-按下, False-未按下


###### `pyb.Switch.callback([callback])`{:class="method"}

设置Switch回调方法

如果`callback=None`, 则禁用回调
