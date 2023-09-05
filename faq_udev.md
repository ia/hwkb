# Rules for `udev` & permissions for devices HOWTO

This note should be applicable to any modern GNU/Linux distro.


# Static names for devices using `udev` rules





# Permission settings for devices

- to get access to device which using `tty` subsystem without root/`sudo`, add your user to `dialout` group:
`$ sudo  gpasswd  -a ${USER}  dialout`

- to get access to device which using `usb` subsystem without root/`sudo`, add your user to `plugdev` group:
$ sudo  gpasswd  -a ${USER}  plugdev




