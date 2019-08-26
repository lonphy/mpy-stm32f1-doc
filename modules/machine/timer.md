---
layout: post
title:  "machine.Timer 类"
date:   2019-08-18 0:0:0 +0000
image: /assets/images/twoscreen.jpg
category: machine
---
`machine.Timer` 可以用于各种任务, 目前只实现了最简单的功能: 周期性调用一个函数

每个`Timer`由一个计数器组成， 以一定速度计数, 计数频率 = ( __外设时钟频率(Hz)__ / __`Timer`预分频__ )

当计数器达到 `period` (定时器周期) 时，会触发一个事件，同时把计数器重置为零

如果指定了 `callback` 方法, `Timer` 事件触发时会主动调用

`Timer` 固定频率翻转LED:
```py
import machine

tim = machine.Timer(4)   # 使用TIM4创建一个 Timer 对象

tim.init(freq=2)         # 触发频率2Hz

tim.callback(lambda t:machine.LED(1).toggle())
```

使用命名函数做为回调:
```py
def tick(timer):                # 被调用时, 会传入一个 Timer 对象
    print(timer.counter())      # 打印当前 Timer对象 的计数器值

tim = machine.Timer(4, freq=1)  # 使用TIM4创建一个Timer, 触发频率1Hz

tim.callback(tick)              # 设置回调为`tick`函数
```

更多示例:
```py
tim = machine.Timer(4, freq=100)    # 直接指定运行频率(Hz)
tim = machine.Timer(4, prescaler=0, period=99)

tim.counter()                   # 获取计数器值(也可设置)

tim.prescaler(2)                # 设置预分频 (也可获取)

tim.period(199)                 # 设置计数周期(也可获取)

tim.callback(lambda t: ...)     # 设置中断触发回调(t是 Timer 对象)
tim.callback(None)              # 禁用回调
```

> `Timer(5)` 被伺服电机驱动(pyb.Servo)使用, `Timer(6)` 用于 `ADC`/`DAC` 定时读写

> __注意__: 在回调函数(__中断__)里, 不能分配内存, 因此在回调函数里引发的异常不能提供更多的有用信息  
> 如何避免此限制，参考`micropython`.`alloc_emergency_exception_buf`()

#### 构造方法

###### `machine.Timer(id, ...)`{:class="method"}

使用指定`id`构造一个新的Timer对象

如果提供了附加参数, 则会调用`Timer.init(...)`初始化.

`id` 取值范围[1, 14]
- `Flash` <= 512KB, `TIM1` ~ `TIM8`
- `Flash` > 512KB,  `TIM1` ~ `TIM14`

#### 实例方法

###### `machine.Timer.init(*, freq, prescaler, period)`{:class="method"}

初始化`Timer`对象, 要么使用`frequecy`(Hz)参数, 要么使用 `prescaler` + `period`参数 初始化:
```py
tim.init(freq=100)                  # 设置 Timer 触发频率 100Hz

tim.init(prescaler=83, period=999)  # 直接设置预分频和重载周期
```

__关键字参数__:

- `freq` — 指定`Timer`触发频率

- `prescaler` - `[0-0xffff]` - 指定`Timer`预分频器(PSC)值  
    ( __`Timer`时钟频率 = `Timer`时钟源频率 / (`prescaler + 1`)__ )

    所有`Timer`都可工作在 __72MHz__

- `period` [0-0xffff] 该参数值被写入`Timer`自动重载寄存器(`ARR`), 用于确定`Timer`计数循环周期, `Timer`计数器在`period+1`个`Timer`时钟周期后重置

- `mode` __计数模式__ 可以是以下常量之一:
    - `Timer.UP` - 从0计数到ARR(默认)
    - `Timer.DOWN` - 从ARR计数到0
    - `Timer.CENTER` - 从0计数到ARR然后再计数到0

- `div` 可以是 [1, 2, 4] 的一个, 用于确定`Timer`分频系数, 分频后供 __数字过滤器__ 采样时钟 使用

- `callback` - 与 `Timer.callback()`一样

- `deadtime` - 指定`Timer`在互补通道上转换期间的 __死区时间__ 或 __非活动时间__ (此时两个通道都将是非活动的)
    __死区时间__ 是一个[0,1008]的整数, 有以下限制:
    - [0-128] 使用公式1
    - [128-256] 使用公式2步长为2
    - [256-512] 使用公式3步长为8
    - [512-1008] 使用公式4, 步长为16

    > `deadtime` measures ticks of `source_freq` divided by `div` clock ticks.

`deadtime` 仅在`Timer(1)` 和 `Timer(8)`上有效, 


###### `machine.Timer.deinit()`{:class="method"}

清理定时器

禁用`callback`及关联中断，禁用所有通道回调及关联中断， 停止`Timer`, 禁用`Timer`外设

###### `machine.Timer.callback(fun)`{:class="method"}

设置用于`Timer`触发回调的函数. `fun`接受1一个参数(Timer对象), 若`fun=None`则禁用回调

###### `machine.Timer.channel(channel, mode, ...)`{:class="method"}

如果仅传入通道号(`channel`), 则前一次已初始化的通道对象会被返回(如果没有已初始化的管道对象, 则返回`None`)

其他情况, 一个`TimerChannel`对象会被初始化，然后返回

每个通道都可以配置为 __调制宽度脉冲__(`PWM`)、__输出比较__(`OC`) 或 __输入捕获__(`IC`)

所有通道共享相同的底层`Timer`，意味着它们共享相同的`Timer`时钟

__关键字参数__:

- `mode` 工作模式是下列常量之一:
    - `Timer.PWM` — PWM模式(高电平有效)
    - `Timer.PWM_INVERTED` — PWM模式(低电平有效)
    - `Timer.OC_TIMING` — 输出比较模式, 无引脚被驱动
    - `Timer.OC_ACTIVE` — 输出比较模式, 当匹配时，引脚将被设为指定电平(电平由`polarty`决定)
    - `Timer.OC_INACTIVE` — 输出比较模式, 当匹配时, 引脚将被设为电平反值(电平由`polarty`决定)
    - `Timer.OC_TOGGLE` — 输出比较模式, 当匹配时, 翻转引脚当前电平
    - `Timer.OC_FORCED_ACTIVE` — 输出比较模式, 强制设为固定电平(电平由`polarty`决定)
    - `Timer.OC_FORCED_INACTIVE` — 输出比较模式, 强制设为固定电平反值(电平由`polarty`决定)
    - `Timer.IC` — 输入捕获模式
    - `Timer.ENC_A` — 编码模式, 计数器只在`CH1`变化时才变化
    - `Timer.ENC_B` — 编码模式, 计数器只在`CH2`变化时才变化
    - `Timer.ENC_AB` — 编码模式, 计数器在`CH1`或`CH2`变化时都变化

- `callback` - 与 `TimerChannel`.`callback`() 一样

- `pin`  - `None` (默认) 或 一个 `Pin` 对象, 如果提供了`pin`参数(非`None`), 对应的管脚会被配置为`Timer`通道
    > 如果`pin`不支持`Timer`对应的通道, 则会报错

- __`Timer.PWM`模式下的关键字参数__:
    - `pulse_width` - 确定脉冲宽度初始值
    - `pulse_width_percent` - 确定脉冲宽度百分比初始值

- __`Timer.OC` 模式下的关键字参数__:
    - `compare` - 确定比较寄存器初始值
    - `polarity` 极性可以是以下值:
        - `Timer.HIGH` - 输出高电平
        - `Timer.LOW`- 输出低电平

- __`Timer.IC`模式下的关键字参数__ :
    - `polarity` 极性可以是以下值:
        - `Timer.RISING` - 上升沿捕获
        - `Timer.FALLING` - 下降沿捕获
        - `Timer.BOTH` - 上升/下降沿都会捕获
    > 注意, 捕获只在主通道有效

__`Timer`.`ENC` 模式说明__:

需要2个管脚, 使用`Timer.counter()`读取编码值, 仅工作在`CH1`和`CH2`(不能是`CH1N`或`CH2N`)

当配置为 `Timer.ENC_xx` 模式时, `channel`参数将被忽略

__PWM__ 示例:
```py
timer = machine.Timer(2, freq=1000)

ch2 = timer.channel(2, machine.Timer.PWM, pin=machine.Pin.board.X2, pulse_width=8000)

ch3 = timer.channel(3, machine.Timer.PWM, pin=machine.Pin.board.X3, pulse_width=16000)
```

###### `machine.Timer.counter([value])`{:class="method"}

获取或设置定时器计数值

###### `machine.Timer.freq([value])`{:class="method"}

设置或获取计Timer运行频率(如果设置了预分频和重载周期则会被修改)


###### `machine.Timer.period([value])`{:class="method"}

设置或获取计数周期


###### `machine.Timer.prescaler([value])`{:class="method"}

设置或获取预分频


###### `machine.Timer.source_freq()`{:class="method"}

获取定时器时钟源频率

<br>


#### `TimerChannel` 类

该类 为`Timer`装置一个通道。 通道用于生成/捕获一个信号, 使用`Timer`.`channel`()方法创建

#### 类实例方法


###### `TimerChannel.callback(fun)`{:class="method"}

设置用于`Timer`通道触发回调的函数. `fun`接受1一个参数(`Timer`对象), 若`fun=None`则禁用回调


###### `TimerChannel.capture([value])`{:class="method"}

获取或设置与通道关联的捕获值

> `capture`、`compare`和`pulse_width`都是同一个函数

`capture` 供`TimerChannel`处于 __输入捕获模式__ 时使用


###### `TimerChannel.compare([value])`{:class="method"}

获取或设置与通道关联的比较值

> `capture`、`compare`和`pulse_width`都是同一个函数

`compare` 供`TimerChannel`处于 __输出比较模式__ 时使用


###### `TimerChannel.pulse_width([value])`{:class="method"}

获取或设置与通道关联的脉宽值

> `capture`、`compare`和`pulse_width`都是同一个函数

`pulse_width` 供`TimerChannel`处于 __PWM__ 时使用

- 边缘对齐模式下，占空比 与 `period+1` 一致
- 中心对齐模式下, 占空比 与 `period` 一致


###### `TimerChannel.pulse_width_percent([value])`{:class="method"}

获取或设置与通道关联的脉宽百分比

`value`是一个数字, 取值[0,100], 指定脉冲处于有效电平的百分比(一个周期)

`value` 也可以是浮点数(获得更高精度)。例如`value=25`表示占空比为25%