

# Periphery boards and pre-assembled units

This document describes ready-to-use PCBs which are widely available to apply in different hardware projects for different purposes.




## USB-C DC power trigger

![USB-C DC power trigger board](../master/resources/boards/usbc-dc.png)  


### Description

| switch |  3V |  9V | 12V | 15V | 20V |
|:------:|:---:|:---:|:---:|:---:|:---:|
|  `S1`  |  0  |  1  |  1  |  0  |  0  |
|  `S2`  |  0  |  0  |  1  |  1  |  1  |
|  `S3`  |  0  |  0  |  0  |  0  |  1  |


### Notes

TBA


### Applications

 - _USB-C_-_DC_ converter with configurable voltage




## Mini DC-DC converter

![Mini DC-DC converter](../master/resources/boards/mini-dcdc.png)  


### Description

|   pin   |       info       |
|:-------:|:-----------------|
|  `VO+`  |  output voltage  |
|  `GND`  |  common ground   |
|  `IN+`  |  input voltage   |
|  `EN`   |  enabled control |


### Notes

TBA


### Applications

 - _DC_-_DC_ converter with configurable voltage




## TTP223-based touch sensor (w/o I/O line)

![TTP223-based touch sensor board](../master/resources/boards/ttp223-touch.png)  


### Description

|    pin    |       info       |
|:---------:|:-----------------|
| `12VDC -` |  power source -  |
| `12VDC +` |  power source +  |
|  `LED  -` |  power sink -    |
|  `LED  +` |  power sink +    |
|  `S1`     |  shorted latch   |
|  `S2`     |  shorted jog     |
|  `C1`     |  sens. capacitor |


### Notes

Information gathered from images & from descriptions on online markets:

 - **power source input**: _**6-28**_ _V_, (up to) _**3**_ _A_;
 - _**power sink**_ _should be_ equal to _**power source**_;
 - _shorted pads_:
   - _shorted latch_ means shorted _**S1**_: touch to open, touch again to close;
   - _shorted jog_ means shorted _**S2**_: touch to open, release to close;
 - _**C1**_ sensitivity adjustment capacitor (_**0-20**_ _pF_): the larger the capacity of _**C1**_, the lower the sensitivity of _touch button area_;
 - standby consuption: _**6**_ _mA_.


### Applications

 - touch-based power switch
 - touch-based trigger for power relay




