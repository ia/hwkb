

# Shikra


![shikra pinout](../master/resources/shikra_pinout.png)  


## Pinouts

| pin | shikra/spi | ft232h | spi pin |
|:---:|------------|--------|---------|
|  1  |     SCK    | ADBUS0 | 6:  CLK |
|  2  |     SDI    | ADBUS1 | 2: MISO |
|  3  |     SDO    | ADBUS2 | 5: MOSI |
|  4  |     *CS    | ADBUS3 | 1:  /CS |
|  5  |     +V?    | ADBUS4 | 8:  VCC |
| 18  |     GND    |        | 4:  GND |

[Official documentation](../master/resources/shikra_documentation.pdf).  


## Software notes

 - `flashrom` usage:
```
$ sudo flashrom -p ft2232_spi:type=232H -r OUTPUT_FILE
```




## Hardware info

 - labeled chip: **FT232HL**
 - [data sheet](https://www.ftdichip.com/Support/Documents/DataSheets/ICs/DS_FT232H.pdf)
 - **FT232H** pinout table:

![ft232h DS pinout](../master/resources/ft232h_datasheet_pinout_table.png)  




## Additional links

 - [Shop](https://int3.cc/products/the-shikra)
 - [Using the Shikra to Attack Embedded Systems: Getting Started](https://www.xipiter.com/musings/using-the-shikra-to-attack-embedded-systems-getting-started)
 - [Programming the Shikra](https://www.xipiter.com/musings/programming-the-shikra)




