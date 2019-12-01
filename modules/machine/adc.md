---
layout: post
title:  "machine.ADC 类"
date:   2019-08-18 0:0:0 +0000
image: /assets/images/twoscreen.jpg
category: machine
---

`machine.ADC` – 模数转换, 从pin上读取模拟值

使用示例:
```py
from machine import ADC
from machine import ADCAll

adc = ADC(pin)                   # create an analog object from a pin
val = adc.read()                 # read an analog value

adc = ADCAll(resolution)         # create an ADCAll object
adc = ADCAll(resolution, mask)   # create an ADCAll object for selected analog channels
val = adc.read_channel(channel)  # read the given channel
val = adc.read_core_temp()       # read MCU temperature
val = adc.read_core_vbat()       # read MCU VBAT
val = adc.read_core_vref()       # read MCU VREF
val = adc.read_vref()            # read MCU supply voltage
```

#### 构造方法

###### `machine.ADC(pin)`{:class="method"}

Create an ADC object associated with the given pin. 

This allows you to then read analog values on that pin.


#### 实例方法

###### `machine.ADC.read()`{:class="method"}

Read the value on the analog pin and return it. 

The returned value will be between 0 and 4095.



###### `machine.ADC.read_timed(buf, timer)`{:class="method"}

Read analog values into `buf` at a rate set by the `timer` object.

- `buf` can be bytearray or array.array for example.  The ADC values have
12-bit resolution and are stored directly into `buf` if its element size is
16 bits or greater.  If `buf` has only 8-bit elements (eg a bytearray) then
the sample resolution will be reduced to 8 bits.

- `timer` should be a Timer object, and a sample is read each time the timer
triggers.  The timer must already be initialised and running at the desired
sampling frequency.

To support previous behaviour of this function, `timer` can also be an
integer which specifies the frequency (in Hz) to sample at.  In this case
Timer(6) will be automatically configured to run at the given frequency.

Example using a Timer object (preferred way):

```py
    adc = pyb.ADC(pyb.Pin.board.X19)    # create an ADC on pin X19
    tim = pyb.Timer(6, freq=10)         # create a timer running at 10Hz
    buf = bytearray(100)                # creat a buffer to store the samples
    adc.read_timed(buf, tim)            # sample 100 values, taking 10s
```

Example using an integer for the frequency:

```py
    adc = pyb.ADC(pyb.Pin.board.X19)    # create an ADC on pin X19
    buf = bytearray(100)                # create a buffer of 100 bytes
    adc.read_timed(buf, 10)             # read analog values into buf at 10Hz
                                        #   this will take 10 seconds to finish
    for val in buf:                     # loop over all values
        print(val)                      # print the value out
```

This function does not allocate any memory.

###### `machine.ADC.read_timed_multi((adcx, adcy, ...), (bufx, bufy, ...), timer)`{:class="method"}

Read analog values from multiple ADC's into buffers at a rate set by the
timer.  The ADC values have 12-bit resolution and are stored directly into
the corresponding buffer if its element size is 16 bits or greater, otherwise
the sample resolution will be reduced to 8 bits.

This function should not allocate any heap memory.








###### `machine.ADC.deinit()`{:class="method"}
Disable the ADC block.


<br />


`machine.ADCAll`

###### `machine.ADCAll.read_channel()`{:class="method"}

Fast method to read the channel value.

###### `machine.ADCChannel.value()`{:class="method"}
Read the channel value.

###### `machine.ADCChannel.init()`{:class="method"}
Re-init (and effectively enable) the ADC channel.

###### `machine.ADCChannel.deinit()`{:class="method"}
Disable the ADC channel.