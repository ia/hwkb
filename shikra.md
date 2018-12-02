Shikra:
SCK  1  ADBUS0  - 6 CLK
SDI  2  ADBUS1  - 2 MISO
SDO  3  ADBUS2  - 5 MOSI
*CS  4  ADBUS3  - 1 /CS
V+?  5  ADBUS4  - 8 VCC
GND 18          - 4 GND

FT232HL

flashrom -p ft2232_spi:type=232H -r spidump.bin

https://www.ftdichip.com/Support/Documents/DataSheets/ICs/DS_FT232H.pdf

https://int3.cc/products/the-shikra
https://www.xipiter.com/musings/using-the-shikra-to-attack-embedded-systems-getting-started

