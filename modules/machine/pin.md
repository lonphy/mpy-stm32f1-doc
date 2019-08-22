---
layout: post
title:  "machine.Pin 类"
date:   2019-08-18 0:0:0 +0000
image: /assets/images/twoscreen.jpg
category: machine
---


管脚(machine.Pin)
    

#### 类方法

###### `Pin.debug([state=False])`{:class="method"}
    
打开或关闭Pin调试模式, 并返回当前模式
> 调试模式只在查找Pin使用`mapper`/`dict`时有效

参数: `state`
- `False` : 关闭调试模式
- `True`: 打开调试模式

###### `Pin.mapper([fun])`{:class="method"}

设置或获取管脚名映射函数

```python
import machine

def pin_mapper(pin_name):
    if name == "led1":
        return machine.Pin.cpu.A1

machine.Pin.mapper(pin_mapper) # set mapper
machine.Pin.mapper()           # get mapper
```

###### `Pin.dict([dict])`{:class="method"}

设置或获取管脚映射字典
    
```python
import machine

m = {'led1': machine.Pin.cpu.A1}
machine.Pin.mapper(m) # set dict
machine.Pin.mapper()  # get dict
```

#### 类属性

###### `board`
板上定义的所有管脚 例如: `Pin.board.PA0`

###### `cpu`
MCU所有管脚定义 例如: `Pin.cpu.A0`

###### 常量定义
- 工作模式(所有引脚均有)
    - `IN`{:class="const"} - 浮空输入
    - `ANALOG`{:class="const"} - 模拟输入
    - `OUT`{:class="const"} - 推挽输出
    - `OPEN_DRAIN`{:class="const"} - 开漏输出
    - `ALT`{:class="const"} - 复用推挽输出
    - `ALT_OPEN_DRAIN`{:class="const"} - 复用开漏输出

<br>
- 上拉下拉(输入有效)
    - `PULL_UP`{:class="const"} - 上拉
    - `PULL_DOWN`{:class="const"} - 下拉

<br>
- 复用功能 (暂时只用于校检引脚是否可以用于指定外设)
    - `AFx_XXX`

#### 实例方法

###### `Pin.init(mode, pull=None, af=-1, value, alt=-1)`{:class="method"}

初始化Pin

参数:  
* `mode` - 运行模式 (参考 类属性->常量定义->工作模式)
* `pull` - 输入上/下拉电阻, None表示不设置 (参考 类属性->常量定义->上拉下拉)
* `af`   - 外设复用组, 参考 ports/stm32f1/boards/stm32f103xx_af.csv
* `value` - 默认电平 0 or 1
* `alt`   - 同 `af`, 优先级更高

###### `Pin.name()`{:class="method"}
     
返回Pin名称

###### `Pin.names()`{:class="method"}

返回Pin名称列表

###### `Pin.__str__()`{:class="method"}

返回Pin对象描述

###### `Pin.value([value])`{:class="method"}

设置Pin管脚电平(ODR)或获取Pin管脚电平(IDR)

###### `off()`{:class="method deprecated"}
使用 `Pin.value(0)` 替代

###### `on()`{:class="method deprecated"}
使用 `Pin.value(1)` 替代

###### `low()`{:class="method deprecated"}
使用 `Pin.value(0)` 替代

###### `high()`{:class="method deprecated"}
使用 `Pin.value(1)` 替代

###### `irq()`{:class="method deprecated"}
使用 [machine.ExtInt]({% link modules/machine/extint.md %}) 替代


###### `Pin.port()`{:class="method"}

返回Pin对应GPIO的索引 0=GPIOA, 1=GPIOB,以此类推

###### `Pin.gpio()`{:class="method"}

返回Pin对应GPIO绝对地址

因为底层使用`small_int`, 即实际地址左移后或1, 有效范围31bit, 已不能直接表示物理内存地址

> **考虑删掉, 节省Flash**

###### `Pin.mode()`{:class="method"}

返回Pin的工作模式, 参考Pin类常量

###### `Pin.pull()`{:class="method"}

返回Pin 上/下拉配置, 参考Pin类常量, 0-无上拉下拉

###### `Pin.af_list()`{:class="method"}

返回Pin可用的复用外设列表

###### `Pin.af()`{:class="method"}
    
返回Pin配置的复用外设 AFx_XXX

> 这里始终返回`0`, 因STM32F1中, 管脚配置复用后， 最后一个启用的外设有效, 且初始化传入的af仅用于参数检查