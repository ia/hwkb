# SPI 101 FAQ


## What is SPI?

`SPI` stands for **Serial Peripheral Interface**.  
`SPI` is an interface bus commonly used to send data between microcontrollers and small peripherals such as shift registers, sensors, and SD cards.  

See [short technical tutorial from _SparkFun_](https://learn.sparkfun.com/tutorials/serial-peripheral-interface-spi) for some details.  


## What is the practical purpose and usage of SPI?

`SPI` is used in embedded devices for flash chips usually to store low-level boot stuff such as: pre-compiled proprietary bootloader and its data, `coreboot`, `UEFI`, `u-boot`, etc.  


## How to find SPI flash ship on PCB?

 - make visual inspection of a `PCB`(**Printed Circuit Board**)
 - locate a chip with dimensions around *5 mm x 5 mm* and **8 pins**
 - ???
 - PROFIT!

![Winbond 25Q32BVSIG 1125](../master/pics/spi_flash.png)  


## How to determine pinouts?

 - determine an exact model of a chip
 - [find](https://duckduckgo.com/?q=winbond+"w25q32bvsig"+datasheet) &
 [read](https://www.winbond.com/resource-files/w25q32bv_revi_100413_wo_automotive.pdf) related datasheet
 - ???
 - PROFIT!

For example, from the picture above it's _Winbond 25Q32BVSIG_.  
Approximate transcription of the name is:
 - _Winbond_ - the name of a manufacturer
 - _25Q_ - the name of a line of a chip
 - _32_ - the size of a flash chip (in _MBytes_)
 - _VSIG_ - additional information (like _encoded_ by a manufacturer
 [package type](https://en.wikipedia.org/wiki/List_of_integrated_circuit_packaging_types) and so on)

Hint: Use "**o**" mark on a chip as a starting point.

![Winbond 25Q32BV datasheet pinout](../master/pics/w25q32bv_datasheet_pinout.png)


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

