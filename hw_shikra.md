

# Shikra


![shikra pinout](../master/resources/shikra_pinout.png)  


### Contents

 * [TODO](#todo)
 * [Pinouts](#pinouts)
 * [Software notes](#software-notes)
 * [Hardware info](#hardware-info)
 * [Additional links](#additional-links)




## TODO

- [ ] refactor document
- [ ] more photos/3rdparty content/links (including extracted from web archive)
- [ ] ftprog & drivers for windows
- [ ] enable led using ftprog
- [ ] configure pins using ftprog
- [ ] pinout table(s)



## Pinouts

| pin | shikra/uart | shikra/jtag | shikra/spi |   ft232h   | spi pin |
|:---:|-------------|-------------|------------|------------|---------|
|  1  |      TX     |     TCK     |     SCK    | 13: ADBUS0 | 6:  CLK |
|  2  |      RX     |     TDI     |     SDI    | 14: ADBUS1 | 2: MISO |
|  3  |             |     TDO     |     SDO    | 15: ADBUS2 | 5: MOSI |
|  4  |             |     TMS     |     *CS    | 16: ADBUS3 | 1:  /CS |
|  5  |             |             |            | 17: ADBUS4 |         |
|  6  |             |             |            | 18: ADBUS5 |         |
|  7  |             |             |            | 19: ADBUS6 |         |
|  8  |             |             |            | 20: ADBUS7 |         |
|  9  |             |             |            | 21: ACBUS0 |         |
| 10  |             |             |            | 25: ACBUS1 |         |
| 11  |             |             |            | 26: ACBUS2 |         |
| 12  |             |             |            | 27: ACBUS3 |         |
| 13  |             |             |            | 28: ACBUS4 |         |
| 14  |             |             |            | 29: ACBUS5 |         |
| 15  |             |             |            | 30: ACBUS6 |         |
| 16  |             |             |            | 31: ACBUS7 |         |
| 17  |             |             |            | 32: ACBUS8 |         |
| 18  |     GND     |     GND     |     GND    | --: ------ |   GND   |

[Official documentation](../master/resources/shikra_documentation.pdf).  


## Software notes

 - [shikra EEPROM programming utility](https://github.com/Xipiter/shikra-programmer)
 - `flashrom` usage (dump flash):
```
$ sudo flashrom -p ft2232_spi:type=232H -r OUTPUT_FILE
```




## Hardware info

 - labeled chip: **FT232HL**
 - [data sheet](https://www.ftdichip.com/Support/Documents/DataSheets/ICs/DS_FT232H.pdf)
 - **FT232H** pinout table:

![ft232h DS pinout](../master/resources/ft232h_datasheet_pinout_table.png)  

 - **Shikra** schematic:

![Shikra schematic](../master/resources/shikra_schematic.png)  




## Additional links

 - [Shop](https://int3.cc/products/the-shikra)
 - [Using the Shikra to Attack Embedded Systems: Getting Started](https://www.xipiter.com/musings/using-the-shikra-to-attack-embedded-systems-getting-started)
 - [Programming the Shikra](https://www.xipiter.com/musings/programming-the-shikra)




