# Rules for `udev` & permissions for devices HOWTO

This note should be applicable to any modern GNU/Linux distro.


# Basic commands to control & monitor `udev`

- apply rules & restart udev & trigger new rules:  
`$ sudo  udevadm  control --reload-rules  &&  sudo  udevadm  trigger`

- monitor events in real time:  
`$ udevadm  monitor  --udev  [--environment]`

- dump information about specific _DEVICE_:  
`$ udevadm  info  -a  -n  /dev/DEVICE`


# Static names for devices using `udev` rules

- to set a static custom name for a device, create file inside `/etc/udev/rules.d` directory and add this line:  
`SUBSYSTEM=="tty", ATTRS{bcdDevice}=="____", ATTRS{idProduct}=="____", ATTRS{idVendor}=="____", ATTRS{product}=="____", ATTRS{removable}=="removable", ATTRS{version}=="____", SYMLINK+="name"`

- to ignore `Modem Manager` probing a device, add this line:  
`ATTRS{idVendor}=="____", ATTRS{idProduct}=="____", ENV{ID_MM_DEVICE_IGNORE}="1", ENV{ID_MM_TTY_BLACKLIST}="1", ENV{MTP_NO_PROBE}="1", ENV{ID_MM_PORT_IGNORE}="1", ENV{ID_MM_TTY_MANUAL_SCAN_ONLY}="1"`


# Execute script/app on inserting specific device

- to run command on insert of a device:
`ACTION=="add", ENV{DEVTYPE}=="usb_device", ATTR{idVendor}=="____", ATTR{idProduct}=="____", ENV{ID_MODEL}=="____", RUN+="/full/path/to/script/with/x/bits"`

- content of script:
```
$ cat /full/path/to/script/with/x/bits
#!/usr/bin/env bash

# run command as root:
command

# run GUI app as a user:
su LOCAL_USER_NAME -c "DISPLAY=:0.0 /full/path/to/GUI/APP" &

# return to caller:
exit 0
```


# Permission settings for devices


## Group policies

- to get access to device which using `tty` subsystem without root/`sudo`, add your user to `dialout` group:  
`$ sudo  gpasswd  -a ${USER}  dialout`  
or  
`$ sudo  usermod  -a  -G dialout  ${USER}`

- to get access to device which using `usb` subsystem without root/`sudo`, add your user to `plugdev` group:  
`$ sudo  gpasswd  -a ${USER}  plugdev`  
or  
`$ sudo  usermod  -a  -G plugdev  ${USER}`


## Generic permission udev rules for devices

- to set permissions add this line:  
`SUBSYSTEM=="tty", ATTRS{bcdDevice}=="____", ATTRS{idProduct}=="____", ATTRS{idVendor}=="____", ATTRS{product}=="____", ATTRS{removable}=="removable", ATTRS{version}=="____", MODE="0664", GROUP="plugdev"`


