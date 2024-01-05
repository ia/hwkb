Basic notes regarding logic analyzers, its software & hardware nuances & howtos.


# Projects




## Saleae


### Logic 1.x

#### Download page: https://support.saleae.com/logic-software/legacy-software/older-software-releases

#### Notes:
- latest version (Aug-2023): 1.2.40
- new HW rev. doesn't work with v1.X
- to unpack AppImage: `./Logic-1.2.40-Linux.AppImage --appimage-extract`
- configs location is: `~/.config/Logic` or `~/.local/share/Logic`
- "decoded protocols panel" is removed
- for Windows:
  - install drivers (i.e., from zip or follow https://support.saleae.com/logic-software/driver-install)
  - install MSFT VS runtime: https://www.microsoft.com/en-us/download/details.aspx?id=48145
- macOS compatibility: 10.8+


### Logic 2.x

#### Download page: https://www.saleae.com/downloads/

#### Notes:
- latest version (Aug-2023): 2.4.9
- compatibility:
  - Windows: 8, 10, 11
  - GNU/Linux: Ubuntu 18.04+ 64bit
  - macOS: 10.14 Mojave+


---


## DSLab

### DSView

#### Download page: https://www.dreamsourcelab.com/download/

#### Github pages:
- https://github.com/DreamSourceLab/
- https://github.com/DreamSourceLab/DSView

### Notes:
- latest version (Aug-2023): 1.3.0
- compatibility:
  - Windows: x64
  - GNU/Linux: Ubuntu +
  - macOS: 10.13 +


---


## Sigrok

### PulseView

#### Download page: https://sigrok.org/wiki/Downloads

#### Github page: https://github.com/sigrokproject

#### Notes:
- latest version (Aug-2023): sigrok_cli / PView - 0.7.2 / 0.4.2
- compatibility:
  - Windows XP +
  - GNU/Linux: Ubuntu 16.04 +
  - macOS: 10.9 +


---
---




# HOWTOs




## PulseView stable release on Ubuntu 18.04 LTS

- download [PulseView 0.4.2 AppImage](https://sigrok.org/download/binary/pulseview/PulseView-0.4.2-x86_64.AppImage)
- extract image:  
`chmod a+x PulseView-0.4.2-x86_64.AppImage && ./PulseView-0.4.2-x86_64.AppImage --appimage-extract`
- patch sources - line 138 in `pd.py` should be look like this:  
`usr/share/libsigrokdecode/decoders/signature/pd.py : 138: incoming = (bin(shiftreg & 0x0291).count('1') + data) & 1`
- run `PulseView` application from unpacked & patched _AppImage_:  
`./AppRun`


## Fast checking logic analyzer using any USB-UART converter

- connect **`LA/Channel 0`** to **`TX`** pin of `USB-UART converter`
- connect `USB-UART converter` to `PC`
- connect `LA` to `PC`
- run terminal app:  
`$ picocom  --baud 9600  --databits 8  --parity n  --flow n  --stopbits 1  --echo /dev/ttyUSB0`
- setup `LA` software tool: _max frequency, one channel, ~one minute / 100 M samples_
- run capture & type any ascii text like `testing testing`
- once capture is stopped, configure `LA SW tool` to decode `UART` protocol using the settings based on a terminal app configuration (i.e., like from the above for `picocom`)
- if `testing` _ASCII_ text is successfully decoded then the setup is good to go for further discovery of the unkown signals & protocols over the wires


## DSLogic Plus firmware

- if there is a message like `TBA` then firmware is missing
- download [`fpga/bin`](../master/resources/blobs/dslogic-plus/v0.97/dreamsourcelab-dslogic-plus-fpga.fw) & [`fx2/fw`](../master/resources/blobs/dslogic-plus/v0.97/dreamsourcelab-dslogic-plus-fx2.fw) files
- place files to `usr/share/sigrok-firmware` location


