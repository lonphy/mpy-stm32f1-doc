---
layout: download
title: 固件下载
permalink: /download/
---

固件下载
---

> `dfu 版本` 可用dfu直刷, 或板子本身的bl写入

> `elf 版本` 与 `bin 版本` 可用STLINK 刷入

烧写地址说明:

| bootloader存在 | APP Flash类型 | 烧写地址 |
|:-- |:-- |:-- |
| N  | 内部 |  `0x08000000` |
| Y  | 内部 |  `0x08020000` |
| Y  | 外部 |  `0x64000000` |


#### MPY-ST103

不带bootloader v1.0
- dfu版本: [下载](https://www.lonphy.com/mpy/mpy-st103/bl-no-firmware-v1.0.dfu)
- bin版本: [下载](https://www.lonphy.com/mpy/mpy-st103/bl-no-firmware-v1.0.bin)
- elf版本: [下载](https://www.lonphy.com/mpy/mpy-st103/bl-no-firmware-v1.0.zip)

带bootloader(内部Flash) v1.0
- dfu版本: [下载](https://www.lonphy.com/mpy/mpy-st103/bl-internal-firmware-v1.0.dfu)
- bin版本: [下载](https://www.lonphy.com/mpy/mpy-st103/bl-internal-firmware-v1.0.bin)
- elf版本: [下载](https://www.lonphy.com/mpy/mpy-st103/bl-internal-firmware-v1.0.zip)

带bootloader(外扩Flash) v1.0
- dfu版本: [下载](https://www.lonphy.com/mpy/mpy-st103/bl-ext-firmware-v1.0.dfu)
- bin版本: [下载](https://www.lonphy.com/mpy/mpy-st103/bl-ext-firmware-v1.0.bin)
- elf版本: [下载](https://www.lonphy.com/mpy/mpy-st103/bl-ext-firmware-v1.0.zip)

#### 启明欣欣 F103RC

不带bootloader v1.0
- dfu版本: [下载](https://www.lonphy.com/mpy/QiMingF103RC/bl-no-firmware-v1.0.dfu)
- bin版本: [下载](https://www.lonphy.com/mpy/QiMingF103RC/bl-no-firmware-v1.0.bin)
- elf版本: [下载](https://www.lonphy.com/mpy/QiMingF103RC/bl-no-firmware-v1.0.zip)

#### SR139

不带bootloader v1.0
- dfu版本: [下载](https://www.lonphy.com/mpy/SR139/bl-no-firmware-v1.0.dfu)
- bin版本: [下载](https://www.lonphy.com/mpy/SR139/bl-no-firmware-v1.0.bin)
- elf版本: [下载](https://www.lonphy.com/mpy/SR139/bl-no-firmware-v1.0.zip)