

# HydraBus


**DISCLAIMER**:

  * This is one-stop document how to prepare, download, build, flash, and use `hydrabus` device in `gnulinux` environment.
  * This is not official documentation but a straight-forward compilation of HOWTOs from official project, documents and other related articles from the Internet.
  * All mentioned trademarks, tools, projects, content, and information belong to their legal creators & copyright holders.
  * This documentation is written & tested with the following versions of hardware/software:

|Component | Revision/Version |
|:---------|:----------------:|
| hydrabus | `1_3`            | 
| hydranfc | `XX`             |
| gnulinux |`Ubuntu 18.04 LTS`|


![HydraBus pinout](../master/resources/hydrabus/pinout_hb.png)  




## Contents


  * [Pinouts](#pinouts)
    * [By connectors](#by-connectors)
      * [PA-J2](#pa-j2)
      * [PB-J1](#pb-j1)
      * [PC-J3](#pc-j3)
      * [SWD_DEBUG](#swd_debug)
    * [By protocols](#by-protocols)
      * [UART](#uart)
      * [SPI](#spi)
      * [JTAG](#jtag)
      * [Wire](#wire)
      * [I2C](#i2c)
      * [SUMP](#sump)
  * [Getting started](#getting-started)
    * [Prepare environment](#prepare-environment)
      * [Permissions](#permissions)
      * [udev](#udev)
      * [Packages](#packages)
    * [Build firmware](#build-firmware)
    * [Flash firmware](#flash-firmware)
 * [Basic interaction](#basic-interaction)
    * [Serial terminal](#serial-terminal)
 * [Basic operations](#basic-operations)
    * [Dump SPI flash](#dump-spi-flash)
    * [Logic analyzer](#logic-analyzer)
 * [Hardware info](#hardware-info)
 * [Additional links](#additional-links) 




## Pinouts


### By connectors


#### PA-J2

**|**_|

|      |     <<<<====    | PA/left | PA/right |    ====>>>>      |
|:-----|:----------------|:-------:|:--------:|:-----------------|
|      | UBTN            |  `PA0`  |  `PA15`  | SPI1 CS          |
|      | ADC1            |  `PA1`  |  `BOOT0` |                  |
|      | UART2 / LIN2 TX |  `PA2`  |  `PD2`   | SDIO CMD         |
|      | UART2 / LIN2 RX |  `PA3`  |  `PA12`  | USB0 D+          |
| DAC1 | ULED            |  `PA4`  |  `PA11`  | USB0 D-          |
| DAC2 | SC /VCC         |  `PA5`  |  `PA10`  | USART1 / LIN1 RX |
|      | SC RST          |  `PA6`  |  `PA9`   | USART1 / LIN1 TX |
|      | SC CD           |  `PA7`  |  `PA8`   | SC CLK           |




#### PB-J1

|**_**|

|      |       |         | <<<<==== | PB/left | PB/right | ====>>>>  |         |        |
|:-----|:-----:|:-------:|:---------|:-------:|:--------:|:----------|:--------|:-------|
|      | SC TX | CAN2 TX | I2C SCL  |  `PB6`  |  `PB5`   | SPI1 MOSI | CAN2 RX | 3W SDO |
| TRST |       |         | I2C SDA  |  `PB7`  |  `PB4`   | SPI1 MISO | 2W IO   | 3W SDI |
| TDI  |       | WIEG D0 | CAN1 RX  |  `PB8`  |  `PB3`   | SPI1 SCK  | 2W CLK  | 3W CLK |
| TDO  |       | WIEG D1 | CAN1 TX  |  `PB9`  |  `PB2`   | BOOT1     |         |        |
| TMS  |       |         | SPI2 SCK |  `PB10` |  `PB1`   |           |         |        |
| TCK  |       |   PWM1  | 1Wire    |  `PB11` |  `PB0`   |           |         |        |


#### PC-J3

|_**|**

|            |  <<<<==== | PC/left | PC/right | ====>>>>   |         |
|:-----------|:---------:|:-------:|:--------:|:-----------|:--------|
|            | SUMP CH 0 |  `PC0`  |  `PC15`  | SUMP CH  8 |         |
| SPI2 CS    | SUMP CH 1 |  `PC1`  |  `PC14`  | SUMP CH  9 |         |
| SPI2 MISO  | SUMP CH 2 |  `PC2`  |  `PC13`  | SUMP CH 10 |         |
| SPI2 MOSI  | SUMP CH 3 |  `PC3`  |  `PC12`  | SUMP CH 11 | SDIO CK |
|            | SUMP CH 4 |  `PC4`  |  `PC11`  | SUMP CH 12 | SDIO D3 |
|            | SUMP CH 5 |  `PC5`  |  `PC10`  | SUMP CH 13 | SDIO D2 |
| Freq.count | SUMP CH 6 |  `PC6`  |  `PC9`   | SUMP CH 14 | SDIO D1 |
|            | SUMP CH 7 |  `PC7`  |  `PC8`   | SUMP CH 15 | SDIO D0 |


#### SWD_DEBUG


| pin |  SWD  | note |
|:---:|:-----:|------|
| `1` | +3V3  |      |
| `2` | SWCLK |`PA14`|
| `3` | GND   |      |
| `4` | SWDIO |`PA13`|
| `5` | NRST  |      |
| `6` | NC    |      |




### By protocols


#### UART

PA-J2 **|**_|

|             <<  | PA/left | PA/right |  >>              |
|:---------------:|:-------:|:--------:|:----------------:|
|                 |  `PA0`  |  `PA15`  |                  |
|                 |  `PA1`  |  `BOOT0` |                  |
| UART2 / LIN2 TX |**`PA2`**|  `PD2`   |                  |
| UART2 / LIN2 RX |**`PA3`**|  `PA12`  |                  |
|                 |  `PA4`  |  `PA11`  |                  |
|                 |  `PA5`  |**`PA10`**| USART1 / LIN1 RX |
|                 |  `PA6`  |**`PA9`** | USART1 / LIN1 TX |
|                 |  `PA7`  |  `PA8`   |                  |


#### SPI

PA-J2 **|**_|

| PA/left | PA/right | >       |
|:-------:|:--------:|:-------:|
|  `PA0`  |**`PA15`**| SPI1 CS |
|  `PA1`  |  `BOOT0` |         |
|  `PA2`  |  `PD2`   |         |
|  `PA3`  |  `PA12`  |         |
|  `PA4`  |  `PA11`  |         |
|  `PA5`  |  `PA10`  |         |
|  `PA6`  |  `PA9`   |         |
|  `PA7`  |  `PA8`   |         |


PB-J1 |**_**|

|        < |  PB/left | PB/right |   >>>     |
|:--------:|:--------:|:--------:|:----------|
|          |  `PB6`   |**`PB5`** | SPI1 MOSI |
|          |  `PB7`   |**`PB4`** | SPI1 MISO |
|          |  `PB8`   |**`PB3`** | SPI1 SCK  |
|          |  `PB9`   |  `PB2`   |           |
| SPI2 SCK |**`PB10`**|  `PB1`   |           |
|          |  `PB11`  |  `PB0`   |           |


PC-J3 |_**|**

|     <<<   | PC/left | PC/right |
|:----------|:-------:|:--------:|
|           |  `PC0`  |  `PC15`  |
| SPI2 CS   |**`PC1`**|  `PC14`  |
| SPI2 MISO |**`PC2`**|  `PC13`  |
| SPI2 MOSI |**`PC3`**|  `PC12`  |
|           |  `PC4`  |  `PC11`  |
|           |  `PC5`  |  `PC10`  |
|           |  `PC6`  |  `PC9`   |
|           |  `PC7`  |  `PC8`   |


#### JTAG

PB-J1 |**_**|

|<<<<< |  PB/left | PB/right |
|:-----|:--------:|:--------:|
|      |  `PB6`   |  `PB5`   |
| TRST |**`PB7`** |  `PB4`   |
| TDI  |**`PB8`** |  `PB3`   |
| TDO  |**`PB9`** |  `PB2`   |
| TMS  |**`PB10`**|  `PB1`   |
| TCK  |**`PB11`**|  `PB0`   |


#### Wire

PB-J1 |**_**|

| <     |  PB/left | PB/right | ==>>>  |        |
|:-----:|:--------:|:--------:|:-------|:-------|
|       |  `PB6`   | *`PB5`*  |        | 3W SDO |
|       |  `PB7`   |**`PB4`** | 2W IO  | 3W SDI |
|       |  `PB8`   |**`PB3`** | 2W CLK | 3W CLK |
|       |  `PB9`   |  `PB2`   |        |        |
|       |  `PB10`  |  `PB1`   |        |        |
| 1Wire |**`PB11`**|  `PB0`   |        |        |


#### I2C

PB-J1 |**_**|

| <<      | PB/left | PB/right |
|:--------|:-------:|:--------:|
| I2C SCL |**`PB6`**|  `PB5`   |
| I2C SDA |**`PB7`**|  `PB4`   |
|         |  `PB8`  |  `PB3`   |
|         |  `PB9`  |  `PB2`   |
|         |  `PB10` |  `PB1`   |
|         |  `PB11` |  `PB0`   |


#### SUMP

PC-J3 |_**|**

| <<<<<<<< | PC/left | PC/right | >>>>>>>> |
|:--------:|:-------:|:--------:|:--------:|
|   CH 0   |**`PC0`**|**`PC15`**|   CH  8  |
|   CH 1   |**`PC1`**|**`PC14`**|   CH  9  |
|   CH 2   |**`PC2`**|**`PC13`**|   CH 10  |
|   CH 3   |**`PC3`**|**`PC12`**|   CH 11  |
|   CH 4   |**`PC4`**|**`PC11`**|   CH 12  |
|   CH 5   |**`PC5`**|**`PC10`**|   CH 13  |
|   CH 6   |**`PC6`**|**`PC9`** |   CH 14  |
|   CH 7   |**`PC7`**|**`PC8`** |   CH 15  |




## Getting started


Express tutorials for GnuLinux/Ubuntu box


### Prepare environment


#### Permissions

 - add a current user to `dialout` group for tty device access without `sudo` (root):
```
$ sudo  gpasswd  -a ${USER}  dialout
```
or
```
$ sudo  usermod  -a  -G dialout  ${USER}
```

 - restart a host to apply changes


#### udev

 - add a line like this to `/etc/udev/rules.d/99-usb.rules` for permanent name in `/dev`:
```
# Hydrabus board

## - set ignore for Modem Manager & permissions
ATTRS{idVendor}=="1d50", ATTRS{idProduct}=="60a7", ENV{ID_MM_DEVICE_IGNORE}="1", ENV{ID_MM_TTY_BLACKLIST}="1", ENV{MTP_NO_PROBE}="1", ENV{ID_MM_PORT_IGNORE}="1", ENV{ID_MM_TTY_MANUAL_SCAN_ONLY}="1", MODE="0664", GROUP="plugdev"

## - set symlink & permissions
SUBSYSTEM=="tty", ATTRS{bcdDevice}=="0200", ATTRS{idProduct}=="60a7", ATTRS{idVendor}=="1d50", ATTRS{product}=="HydraBus 1.0 COM Port1", ATTRS{removable}=="removable", ATTRS{version}==" 1.10", MODE="0664", GROUP="plugdev", SYMLINK+="hydrabus"
```

 - restart `udev` to apply changes:
```
$ sudo  udevadm  control  --reload-rules  &&  sudo  udevadm  trigger
```


#### Packages

 - install required `deb` packages (to build firmware and to work with the device):
```
$ sudo  apt  install  dfu-util  picocom  python3-pip  python3-serial  sigrok  sigrok-cli  pulseview  flashrom  flashrom  gcc-arm-none-eabi  libnewlib-arm-none-eabi  libstdc++-arm-none-eabi-newlib  libc6:i386
```

 - install required `python` modules:
```
$ python3  -m pip  install  GitPython intelhex pyHydrabus  --upgrade
```




### Build firmware

 - download firmware source and 3rd party components (chibios, tokenline & python-hydrabus):
```
$ git  clone  git@ssh.github.com:hydrabus/hydrafw.git
$ cd  hydrafw
$ git  submodule  init
$ git  submodule  update
```

 - (optionally) create a separate local branch for experiments before building:
```
$ git  checkout  -b 0.11-local  v0.11
```

 - build local firmware:
```
$ cd  src
$ make  clean
$ make
```




### Flash firmware

 - put the device into DFU mode - the most reliable way:
    * connect the device to a host using **microUSB1** connector (the one next to **two buttons**): two green LEDs will be on, one **user LED** will be blinking
    * press & **HOLD** the **UBTN** (user button - closest to the microUSB port): one **user LED** will be blinking faster
    * while keep **HOLDING** the **UBTN**, press for a second **RESET button** and release it while keep holding the **UBTN** still
    * release **UBTN**: now the device should be in DFU mode, two green LEDs will be on, one **user LED** will be **OFF**
    * check DFU mode using `dfu-util`:
```
$ dfu-util  --list
dfu-util 0.9

Copyright 2005-2009 Weston Schmidt, Harald Welte and OpenMoko Inc.
Copyright 2010-2016 Tormod Volden and Stefan Schmidt
This program is Free Software and has ABSOLUTELY NO WARRANTY
Please report bugs to http://sourceforge.net/p/dfu-util/tickets/

Found DFU: [0483:df11] ver=2200, devnum=31, cfg=1, intf=0, path="2-1", alt=3, name="@Device Feature/0xFFFF0000/01*004 e"
Found DFU: [0483:df11] ver=2200, devnum=31, cfg=1, intf=0, path="2-1", alt=2, name="@OTP Memory /0x1FFF7800/01*512 e,01*016 e"
Found DFU: [0483:df11] ver=2200, devnum=31, cfg=1, intf=0, path="2-1", alt=1, name="@Option Bytes  /0x1FFFC000/01*016 e"
Found DFU: [0483:df11] ver=2200, devnum=31, cfg=1, intf=0, path="2-1", alt=0, name="@Internal Flash  /0x08000000/04*016Kg,01*064Kg,07*128Kg"
```

 - flash:
```
$ dfu-util  -d 0483:df11  -a 0  -D build/hydrafw.dfu
dfu-util 0.9

Copyright 2005-2009 Weston Schmidt, Harald Welte and OpenMoko Inc.
Copyright 2010-2016 Tormod Volden and Stefan Schmidt
This program is Free Software and has ABSOLUTELY NO WARRANTY
Please report bugs to http://sourceforge.net/p/dfu-util/tickets/

Opening DFU capable USB device...
ID 0483:df11
Run-time device DFU version 011a
Claiming USB DFU Interface...
Setting Alternate Setting #0 ...
Determining device status: state = dfuERROR, status = 10
dfuERROR, clearing status
Determining device status: state = dfuIDLE, status = 0
dfuIDLE, continuing
DFU mode device DFU version 011a
Device returned transfer size 2048
DfuSe interface name: "Internal Flash  "
file contains 1 DFU images
parsing DFU image 1
image for alternate setting 0, (1 elements, total size = 184476)
parsing element 1, address = 0x08000000, size = 184468
Download	[=========================] 100%       184468 bytes
Download done.
done parsing DfuSe file
```




## Basic interaction


Use the following settings for serial connection to the device:

|  Option  | Value  |
|:--------:|:------:|
| Baudrate |`115200`|
| Databits |   `8`  |
| Parity   |   `n`  |
| Flow     |   `n`  |
| Stopbits |   `1`  |


### Serial terminal

Connect to the device using one of serial terminal tools like `picocom`:
```
$ picocom  --baud 115200  --databits 8  --parity n  --flow n  --stopbits 1  /dev/hydrabus

picocom v2.2

port is        : /dev/hydrabus
flowcontrol    : none
baudrate is    : 115200
parity is      : none
databits are   : 8
stopbits are   : 1
escape is      : C-a
local echo is  : no
noinit is      : no
noreset is     : no
nolock is      : no
send_cmd is    : sz -vv
receive_cmd is : rz -vv -E
imap is        : 
omap is        : 
emap is        : crcrlf,delbs,

Type [C-a] [C-h] to see available commands

Terminal ready

```

To make sure that the device is up and running, check the following basic commands:

  * type `show system` to display system information:
```
> show system
HydraFW (HydraBus) v0.11-0-gd6d6586-dirty 2023-03-24
sysTime: 0x000699ed.
cyclecounter: 0xb1357118 cycles.
cyclecounter64: 0x00000000b1357127 cycles.
10ms delay: 1680032 cycles.

MCU Info
DBGMCU_IDCODE:0x10076413
CPUID:        0x410FC241
Flash UID:    0x41002D 0x34365111 0x39333434
Flash Size:   1024KB

Kernel:       ChibiOS 5.1.0
Compiler:     GCC 6.3.1 20170620
Architecture: ARMv7E-M
Core Variant: Cortex-M4F
Port Info:    Advanced kernel mode
Platform:     STM32F405 High Performance with DSP and FPU
Board:        HydraBus 1.0
Build time:   May 30 2023 - 17:52:55
> 
```




## Basic operations


### Dump SPI flash

 - connect pins from the device to a flash chip according to [SPI](#spi) table
 - `flashrom` usage (dump flash):
```
$ flashrom -p serprog:dev=/dev/hydrabus -r DUMP.bin

LOG OUTPUT TBA
```


### Logic analyzer

 - connect pins from the device to target pins according to [SUMP](#sump) table
 - TBA




## Hardware info


### Board

 - hardware revision: **v1_3**
 - **HydraBus** schematic:

![HydraBus schematic](../master/resources/hydrabus/schematic_hb.png)  


### MCU

 - labeled chip: **STM32F415**
 - [ST documentation](https://www.st.com/en/microcontrollers-microprocessors/stm32f405-415/documentation.html)
 - [ST documentation/pdf](https://www.st.com/resource/en/datasheet/stm32f415rg.pdf)
 - **TBA_MCU** pinout table:

![TBA_MCU DS pinout](../master/resources/hydrabus/mcu_hb_ds_pt.png)  




## Additional links

 - [Blog](https://hydrabus.com)
 - [Shop](https://hydrabus.com/buy-online)




