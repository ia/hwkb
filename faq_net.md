# GNULinux network configuration notes


## Basic status


### Check interfaces

**WARNING: `ifconfig` is deprecated!**

- checking status of network interfaces using `ip` tool:
```
$ ip  address
$ ip  link
```

- checking status of network interfaces using `networkctl` tool:
```
$ networkctl  status  IFNAME
```

- checking status of ntework using `systemd` tool:
```
$ sudo  service  systemd-networkd  status
```


### Check detailed capabilities

- checking detailed physical capabilities of network interface using ethtool:
```
$ sudo  ethtool  IFNAME
```
**NOTE**: _`ethtool` uses set of `ioctl`'s (such as `SIOCETHTOOL` for media physical speed)
to get additional information which can't be provided by any other default tools_.




## Listening traffic


### Wire

To listen traffic (aka _"sniffing"_), these two steps should be performed:

- set packet forwarding:
```
$ echo  1  |  sudo  tee  /proc/sys/net/ipv4/ip_forward
```

- set promisc mode for network interface:
```
$ sudo  ip  link  set  IFNAME  promisc  on
```

- run `tcpdump` to capture packets fully:
```
$ sudo  tcpdump  -i IFNAME  -nn  -XX  -vvv  -s0  -S  [-e FILTER RULE(S)]
```




## Config locations related to network settings:

- `/etc/netplan/01-network-manager-all.yaml`
- `/etc/udev/rules.d/`
- `/var/lib/udev/`




## Static canonical predictable names of network interfaces

- add `net.ifnames=0  biosdevname=0` options to `GRUB_CMDLINE_LINUX_DEFAULT` variable inside `/etc/default/grub` configuration file
- optionally, to name interface based on MAC address, add `udev` rule like:
```
SUBSYSTEM=="net", ACTION=="add", DRIVERS=="?*", ATTR{address}=="__:__:__:__:__:__", ATTR{dev_id}=="0x0", ATTR{type}=="1", KERNEL=="eth*", NAME="em0"
```

