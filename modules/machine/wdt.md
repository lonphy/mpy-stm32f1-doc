---
layout: post
title:  "machine.WDT 类"
date:   2019-08-18 0:0:0 +0000
image: /assets/images/twoscreen.jpg
category: machine
---


独立看门狗(pyb.WDT)

#### 方法

###### `WDT(id, timeout)`{:class="method"}
    
构造方法, 创建独立看门狗

参数:
- `id`: 看门狗ID， 只能是0
- `timeout`: 超时时间, 单位ms

| 喂狗周期ms    |	预分频    |	计数器频率    |	重载值             |
|:--          |:--          |:--:         |:--                |
| <512	      |0 = /4	    | 10000	      | timeout * 10      |
| >=512	      |1 = /8	    | 5000	      | timeout/2 * 10    |
| >=1024	  |2 = /16      | 2500	      | timeout/4 * 10    |
| >=2048	  |3 = /32      | 1250	      | timeout/8 * 10    |
| >=4096	  |4 = /64      | 625	      | timeout/16 * 10   |
| >=8192	  |5 = /128     | 312	      | timeout/32 * 10   |
| >= 16384	  |6 = /256     | 156	      | timeout/64 * 10   |

###### `WDT.feed()`{:class="method"}

喂狗


#### 综合示例

```python
import pyb

# pyb.WDT
wdt = pyb.WDT(0, 1000)

for True:
    pass # do something
    wdt.feed()
    pass # do another
```
<br>