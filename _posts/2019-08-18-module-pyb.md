---
layout: post
title:  "pyb 模块"
date:   2019-08-18 0:0:0 +0000
image: /assets/images/pyb.jpg
---

pyb包提供一些特定外部或内部模块的驱动

#### 模块方法

###### `pyb.fault_debug()`{:class="method"}
    
描述: __TODO__

###### `pyb.repl_info()`{:class="method"}

描述: __TODO__

###### `pyb.irq_stats()`{:class="method"}

描述: __TODO__

###### `pyb.country([code])`{:class="method"}

描述: __TODO__

###### `pyb.elapsed_millis()`{:class="method"}

描述: __TODO__

###### `pyb.elapsed_micros()`{:class="method"}

描述: __TODO__

###### `pyb.main()`{:class="method"}

描述: __TODO__

###### `pyb.dht_readinto()`{:class="method"}

描述: DHT单线驱动

###### `pyb.pwm_set(period, pulse)`{:class="method"}

描述: __TODO__, Servo模块启用时有效

###### `pyb.servo_set(port, value)`{:class="method"}

描述: __TODO__, Servo模块启用时有效

#### 模块包含的类

- [VCP]({% link modules/pyb/vcp.md %}) - USB通讯
- [HID]({% link modules/pyb/hid.md %}) - USB人机对话接口
- [LED]({% link modules/pyb/led.md %}) - LED
- [Accel]({% link modules/pyb/accel.md %}) - MMA7660加速传感器
- [Switch]({% link modules/pyb/switch.md %}) - 按键开关[文档已完成]
- [LCD]({% link modules/pyb/lcd.md %}) - LCD显示屏接口
- [Servo]({% link modules/pyb/servo.md %}) - 私服电机
