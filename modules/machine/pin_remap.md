---
layout: post
title:  "machine.PinRemap 类"
date:   2019-08-18 0:0:0 +0000
image: /assets/images/twoscreen.jpg
category: machine
---

管脚重映射(machine.PinRemap)


#### 类方法

###### `PinRemap.remap(phriph [, value])`{:class="method"}

获取或配置管脚映射

参数:
- `phriph`, 参考 [Pin重映射外设](#const_phriph)
- `value`, 可选, 参考 [Pin重映射值](#const_values)

#### 类常量

###### Pin重映射外设 {#const_phriph}
- `PinRemap.REMAP_PIN_SPI1`{:class="const"}
- `PinRemap.REMAP_PIN_I2C1`{:class="const"}
- `PinRemap.REMAP_PIN_USART2`{:class="const"}
- `PinRemap.REMAP_PIN_USART1`{:class="const"}
- `PinRemap.REMAP_PIN_USART3`{:class="const"}
- `PinRemap.REMAP_PIN_TIM1`{:class="const"}
- `PinRemap.REMAP_PIN_TIM2`{:class="const"}
- `PinRemap.REMAP_PIN_TIM3`{:class="const"}
- `PinRemap.REMAP_PIN_TIM4`{:class="const"}
- `PinRemap.REMAP_PIN_CAN`{:class="const"}
- `PinRemap.REMAP_PIN_PD01`{:class="const"}
- `PinRemap.REMAP_PIN_TIM5CH_4`{:class="const"}
- `PinRemap.REMAP_PIN_ADC1_ETRGINJ`{:class="const"}
- `PinRemap.REMAP_PIN_ADC1_ETRGREG`{:class="const"}
- `PinRemap.REMAP_PIN_ADC2_ETRGINJ`{:class="const"}
- `PinRemap.REMAP_PIN_ADC2_ETRGREG`{:class="const"}

以下外设在 `STM32F103xG` 型号中可用
- `PinRemap.REMAP_PIN_TIM9`{:class="const"}
- `PinRemap.REMAP_PIN_TIM10`{:class="const"}
- `PinRemap.REMAP_PIN_TIM11`{:class="const"}
- `PinRemap.REMAP_PIN_TIM13`{:class="const"}
- `PinRemap.REMAP_PIN_TIM14`{:class="const"}
- `PinRemap.REMAP_PIN_FSMC_NADV`{:class="const"}

###### Pin重映射值 {#const_values}
- `REMAP_VAL_NONE`{:class="const"} - 无重映射
- `REMAP_VAL_PART1`{:class="const"} - 部分映射1
- `REMAP_VAL_PART2`{:class="const"} - 部分映射2
- `REMAP_VAL_FULL`{:class="const"} - 全部映射