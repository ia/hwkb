```
# ThinkPad X240:
# - switch End and Insert keys (so that when Fn-Lock is enabled, End works without Fn).
# - switch Home/End <> PgUp/PgDn

evdev:atkbd:dmi:bvn*:bvr*:bd*:svnLENOVO:pn*:pvrThinkPadX240:*
 KEYBOARD_KEY_c7=pageup   # physical    Home
 KEYBOARD_KEY_d2=pagedown # physcial    End
 KEYBOARD_KEY_cf=insert   # physical Fn+End
 KEYBOARD_KEY_c9=home     # physical    PgUp
 KEYBOARD_KEY_d1=end      # physical    PgDn

# http://fliplinux.com/udev-hwdb-numlock-0.html (squotted)
# https://yulistic.gitlab.io/2017/12/linux-keymapping-with-udev-hwdb
```

tools to detect key codes:
- xev
- sudo evtest




/etc/dbus-1/system.d/org.freedesktop.UPower.conf


    <allow send_destination="org.freedesktop.UPower"
           send_interface="org.freedesktop.UPower.KbdBacklight"/>


    <deny send_destination="org.freedesktop.UPower"
           send_interface="org.freedesktop.UPower.KbdBacklight"/>


backlight keyboard control
/sys/devices/platform/thinkpad_acpi/leds/tpacpi::kbd_backlight/brightness
0 //   0%
1 //  50%
2 // 100%

