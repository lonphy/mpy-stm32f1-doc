---
layout: post
title:  "MicroPython ustruct - 打/解包基本数据类型"
date:   2019-08-18 0:0:0 +0000
category: library
---

Supported size/byte order prefixes: "@", "<", ">", "!".

Supported format codes: "b", "B", "h", "H", "i", "I", "l", "L", "q",
"Q", "s", "P", "f", "d" (the latter 2 depending on the floating-point
support).


#### 库方法

###### `ustruct.calcsize(fmt)`{:class="method"}

Return the number of bytes needed to store the given *fmt*.

###### `ustruct.pack(fmt, v1, v2, ...)`{:class="method"}

Pack the values *v1*, *v2*, ... according to the format string *fmt*. 

The return value is a bytes object encoding the values.


###### `ustruct.pack_into(fmt, buffer, offset, v1, v2, ...)`{:class="method"}

Pack the values *v1*, *v2*, ... according to the format string
*fmt* into a *buffer* starting at *offset*. 

*offset* may be negative to count from the end of *buffer*.


###### `ustruct.unpack(fmt, data)`{:class="method"}

Unpack from the *data* according to the format string *fmt*. 

The return value is a tuple of the unpacked values.


###### `ustruct.unpack_from(fmt, data, offset=0)`{:class="method"}

Unpack from the *data* starting at *offset* according to the format
string *fmt*. 

*offset* may be negative to count from the end of *buffer*. 

The return value is a tuple of the unpacked values.
