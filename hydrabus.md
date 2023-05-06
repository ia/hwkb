

# HydraBus


![HydraBus pinout](../master/resources/hydrabus_pinout.png)  


## Contents


  * [Pinouts](#pinouts)
    * [PA-J2](#pa-j2)
    * [PB-J1](#pb-j1)
    * [PC-J3](#pc-j3)
    * [SWD_DEBUG](#swd_debug)
  * [Getting started](#getting-started)
    * [Prepare environment](#prepare-environment)
    * [Update firmware](#update-firmware)
    * [Build firmware](#build-firmware)
 * [Basic operations](#basic-operations)
    * [Dump SPI flash](#dump-spi-flash)
    * [Logic analyzer](#logic-analyzer)
 * [Hardware info](#hardware-info)
 * [Additional links](#additional-links) 




## Pinouts


### PA-J2


|      |     <<<<====    | PA/left | PA/right |    ====>>>>      |
|:----:|-----------------|---------|----------|------------------|
|      | UBTN            |   PA0   |   PA15   | SPI1 CS          |
|      | ADC1            |   PA1   |   BOOT0  |                  |
|      | UART2 / LIN2 TX |   PA2   |   PD2    | SDIO CMD         |
|      | UART2 / LIN2 RX |   PA3   |   PA12   | USB0 D+          |
| DAC1 | ULED            |   PA4   |   PA11   | USB0 D-          |
| DAC2 | SC /VCC         |   PA5   |   PA10   | USART1 / LIN1 RX |
|      | SC RST          |   PA6   |   PA9    | USART1 / LIN1 TX |
|      | SC CD           |   PA7   |   PA8    | SC CLK           |


### PB-J1


|       |         | <<<<==== | PB/left | PB/right | ====>>>>  |         |        |
|:-----:|---------|----------|---------|----------|-----------|---------|--------|
| SC TX | CAN2 TX | I2C SCL  |   PB6   |   PB5    | SPI1 MOSI | CAN2 RX | 3W SDO |
|       |         | I2C SDA  |   PB7   |   PB4    | SPI1 MISO | 2W IO   | 3W SDI |
|       | WIEG D0 | CAN1 RX  |   PB8   |   PB3    | SPI1 SCK  | 2W CLK  | 3W CLK |
|       | WIEG D1 | CAN1 TX  |   PB9   |   PB2    | BOOT1     |         |        |
|       |         | SPI2 SCK |   PB10  |   PB1    |           |         |        |
|       |   PWM1  | 1Wire    |   PB11  |   PB0    |           |         |        |


### PC-J3


|            |  <<<<==== | PC/left | PC/right | ====>>>>   |         |
|:----------:|-----------|---------|----------|------------|---------|
|            | SUMP CH 0 |   PC0   |   PC15   | SUMP CH  8 |         |
| SPI2 CS    | SUMP CH 1 |   PC1   |   PC14   | SUMP CH  9 |         |
| SPI2 MISO  | SUMP CH 2 |   PC2   |   PC13   | SUMP CH 10 |         |
| SPI2 MOSI  | SUMP CH 3 |   PC3   |   PC12   | SUMP CH 11 | SDIO CK |
|            | SUMP CH 4 |   PC4   |   PC11   | SUMP CH 12 | SDIO D3 |
|            | SUMP CH 5 |   PC5   |   PC10   | SUMP CH 13 | SDIO D2 |
| Freq.count | SUMP CH 6 |   PC6   |   PC9    | SUMP CH 14 | SDIO D1 |
|            | SUMP CH 7 |   PC7   |   PC8    | SUMP CH 15 | SDIO D0 |


### SWD_DEBUG


| pin |  SWD  | note |
|:---:|-------|------|
|  1  | +3V3  |      |
|  2  | SWCLK | PA14 |
|  3  | GND   |      |
|  4  | SWDIO | PA13 |
|  5  | NRST  |      |
|  6  | NC    |      |




## Getting started


Express tutorials for GnuLinux/Ubuntu box


### Prepare environment

 - add a current user to `dialout` group for tty device access without `sudo` (root):
```
$ sudo  gpasswd  -a ${USER}  dialout
```
or
```
$ sudo  usermod  -a  -G dialout  ${USER}
```

 - add a line like this to `/etc/udev/rules.d/99-usb.rules` for permanent name in `/dev`:
```

# Hydrabus board
SUBSYSTEM=="tty", ATTRS{bcdDevice}=="0200", ATTRS{idProduct}=="60a7", ATTRS{idVendor}=="1d50", ATTRS{product}=="HydraBus 1.0 COM Port1", ATTRS{removable}=="removable", ATTRS{version}==" 1.10", SYMLINK+="hydrabus"

```

 - restart `udev` to apply changes:
```
$ sudo  udevadm  control  --reload-rules  &&  sudo  udevadm  trigger
```

 - install helpful packages:
```
$ sudo  apt  install  dfu-util  picocom  python3-pip  python3-serial  sigrok  sigrok-cli  pulseview  flashrom  flashrom  gcc-arm-none-eabi  libnewlib-arm-none-eabi  libstdc++-arm-none-eabi-newlib  libc6:i386
```


### Update firmware

 - download firmware
 - TBA


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




## Basic operations


### Dump SPI flash

 - connect TBA PINS
 - `flashrom` usage (dump flash):
```
$ flashrom -p TBA -r OUTPUT_FILE
```


### Logic analyzer

 - TBA




## Hardware info

 - hardware revision: **v1_3**
 - labeled chip: **TBA_MCU**
 - [data sheet](https://example.com/mcu.pdf)
 - **TBA_MCU** pinout table:

![TBA_MCU DS pinout](../master/resources/hydrabus_MCU_datasheet_pinout_table.png)  

 - **HydraBus** schematic:

![HydraBus schematic](../master/resources/hydrabus_schematic.png)  




## Additional links

 - [Blog]()
 - [Shop]()




