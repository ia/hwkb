

# SPI 101 FAQ


### Contents

 * [What is SPI?](#what-is-spi)
 * [What is the practical purpose and usage of SPI?](#what-is-the-practical-purpose-and-usage-of-spi)
 * [How to find SPI flash ship on PCB?](#how-to-find-spi-flash-ship-on-pcb)
 * [How to determine pinouts?](#how-to-determine-pinouts)
 * [How to determine suitable voltage?](#how-to-determine-suitable-voltage)
 * [What is the physical meaning and purpose of each SPI pin?](#what-is-the-physical-meaning-and-purpose-of-each-spi-pin)
 * [How to dump SPI flash storage?](#how-to-dump-spi-flash-storage)
 * [What is the purpose of dumping SPI flash?](#what-is-the-purpose-of-dumping-spi-flash)
 * [How SPI flash image can be analyzed?](#how-spi-flash-image-can-be-analyzed)





## What is SPI?

`SPI` stands for **Serial Peripheral Interface**.  
`SPI` is an interface bus commonly used to send data between microcontrollers and small peripherals such as shift registers, sensors, and SD cards.  

See [short technical tutorial from _SparkFun_](https://learn.sparkfun.com/tutorials/serial-peripheral-interface-spi) for some details.  




## What is the practical purpose and usage of SPI?

`SPI` is used in embedded devices for flash chips usually to store low-level boot stuff such as:
 - pre-compiled proprietary bootloader and its data
 - [`UEFI`](https://www.uefi.org)
 - [`u-boot`](https://www.denx.de/wiki/U-Boot)
 - [`coreboot`](https://coreboot.org/)
 - etc.  




## How to find SPI flash ship on PCB?

 - make visual inspection of a `PCB`(**Printed Circuit Board**)
 - locate a chip with dimensions around *5 mm x 5 mm* and **8 pins**
 - ???
 - PROFIT!

![Winbond 25Q32BVSIG 1125](../master/resources/spi_flash.png)  




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

![Winbond 25Q32BV datasheet pinout](../master/resources/w25q32bv_datasheet_pinout.png)  




## How to determine suitable voltage?

It's **3.3 V** _usually_. At least that voltage source should be used on the other end when some hardware debugging tool is connected to SPI.  
However, **it's always better just to make sure** by looking through a datasheet again.  

![Winbond 25Q32BV datasheet voltage](../master/resources/w25q32bv_datasheet_voltage.png)  

So, for **25Q32BV** it's from **2.7**/**3.0** to **3.6** V.  




## What is the physical meaning and purpose of each SPI pin?


### Pins table

| pin |  name  |      other names     |
|:---:|--------|----------------------|
|  1  |   /CS  |  !CE  /  CS  / SS    |
|  2  |    DO  |   SO  /  SDO / MISO  |
|  3  |   /WP  |  !WP                 |
|  4  |   GND  |                      |
|  5  |    DI  |   SI  /  SDI / MOSI  |
|  6  |   CLK  |  SCK  / SCLK         |
|  7  | /HOLD  | !RST                 |
|  8  |   VCC  |    +V / VLK          |


### Pins notes

 - **1** - **/CS**
    - _chip select_ **input**: enables and disables device operation
    - _slave select_ **output**: controls other SPI devices connected to the same bus
 - **2** - **DO**
    - _data_ **output**: where data comes out (**Master In/Slave Out**)
 - **3** - **/WP**
    - _write protect_ **input**: used to prevent the
 [Status Register](http://www.avrbeginners.net/architecture/spi/spi.html#spsr) from being written
 - **4** - **GND**
    - [**ground**](https://en.wikipedia.org/wiki/Ground_(electricity)#Electronics)
 - **5** - **DI**
    - _data_ **input**: where data comes in (**Master Out/Slave In**)
 - **6** - **CLK**
    - _serial clock_ **input**: provides the timing for serial input and output operations
 - **7** - **/HOLD**
    - _hold_ **input**: "pauses" device operation
 - **8** - **VCC**
    - _power supply_ [**voltage**](https://en.wikipedia.org/wiki/Voltage)

![Winbond 25Q32BV datasheet pinout table](../master/resources/w25q32bv_datasheet_pinout_table.png)  
![Winbond 25Q32BV datasheet pinout info](../master/resources/w25q32bv_datasheet_pinout_info.png)  




## How to dump SPI flash storage?


### Software

Install [`flashrom`](https://www.flashrom.org/Flashrom).


### Hardware

Get one of `USB-to-SPI` hardware tools:
 * [BusPirate](http://dangerousprototypes.com/docs/Bus_Pirate_v3.6)
 * [HydraBus](https://hydrabus.com)
 * [Shikra](https://int3.cc/products/the-shikra)
 * [FT2232](https://www.ftdichip.com/Products/ICs/FT2232H.html)-based [breakout board](http://dangerousprototypes.com/docs/FT2232_breakout_board)
 * ... or even [`Arduino`-based board](https://tomvanveen.eu/flashing-bios-chip-arduino/)!

### Wiring

Connect related pins from a board to SPI chip in the following way:
```
                    +----------+
  CS pin | /CS  -~~-| o      8 |----  VCC | 3.3 voltage pin
MOSI pin |  DO  ->>-| 2      7 |---- /HOLD
           /WP  ----| 3      6 |-_-_  CLK |  CLK pin
 GND pin | GND  3>--| 4      5 |->>-  DI  |  MISO pin
                    +----------+
```

**WARNINGS**:
 - **DO NOT CONNECT TO PINS OF SPI CHIP FROM MOTHERBOARD WHEN MOTHERBOARD IS POWERED ON**
 - **DO NOT CONNECT TO VOLTAGE PIN OF SPI CHIP FROM LAPTOP MOTHERBOARD WHEN BATTERY IS CONNECTED**


### Putting it all together

 - turn off a target device
 - **disconnect any power source**:
    - **unplug AC adapter** (if any)
    - **disconnect battery** (if applicable)
 - connect pins from debug board to SPI chip on target device according to the scheme above
 - plug in debug board to PC
 - run `flashrom` in dummy mode to verify wiring:
```
$ sudo flashrom -p HW_PROGRAMMER_NAME[:PARAMETERS]
```
 - run `flashrom` to dump memory (depending on hardware type it may required some time):
```
$ sudo flashrom -p HW_PROGRAMMER_NAME[:PARAMETERS] -r OUTPUT_FILE
```




## What is the purpose of dumping SPI flash?

 - backup firmware
 - firmware research and development:
    - flashing
    - testing
    - debugging
    - analyzing
    - reverse engineering




## How SPI flash image can be analyzed?

 - [`strings`](http://man7.org/linux/man-pages/man1/strings.1.html)
 - [`hexdump`](http://man7.org/linux/man-pages/man1/hexdump.1.html)
 - [`veles`](https://github.com/codilime/veles)
 - [`binwalk`](https://github.com/ReFirmLabs/binwalk)
 - [`fwtools`](https://github.com/flammit/fwtools)
 - [`UEFITool`](https://github.com/LongSoft/UEFITool)




