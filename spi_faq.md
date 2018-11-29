# SPI 101 FAQ


## What is SPI?

`SPI` stands for **Serial Peripheral Interface**.  
`SPI` is an interface bus commonly used to send data between microcontrollers and small peripherals such as shift registers, sensors, and SD cards.  

See [short technical tutorial from _SparkFun_](https://learn.sparkfun.com/tutorials/serial-peripheral-interface-spi) for some details.  


## What is the practical purpose and usage of SPI?

`SPI` is used in embedded devices for flash chips usually to store low-level boot stuff such as: pre-compiled proprietary bootloader and its data, `coreboot`, `UEFI`, `u-boot`, etc.  


## How to find SPI flash ship on PCB?

 - make visual inspection of a `PCB`(**Printed Circuit Board**)
 - locate a chip with dimensions around *5mm x 5mm* and **8 pins**
 - ???
 - PROFIT!

![Winbond 25Q32BVSIG 1125](../master/pics/spi_flash.png)
Format: ![Alt Text](url)


## How to determine pinouts?

## How to dump storage?

## TBA

```
                         +----------+
(color1) XN   | !CE  ----| o        |----   +V | 3v3 (color4)
(color2) YM   |  SO  ----|          |---- !RST
                !WP  ----|          |----  SCK |  KP (color5)
(color3) GND  | GND  ----|          |----  SI  |  LQ (color6)
                         +----------+
```

