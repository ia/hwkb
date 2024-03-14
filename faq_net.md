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


### Check network ports

 - list open tcp ports using `ss`: `$ sudo  ss  -ntlp`
 - list open tcp ports using `netstat` (**DEPRECATED**): `$ sudo  netstat  -ntlp`
   - _-n_ / _--numeric_ : show IP address instead of resolved host name
   - _-t_ / _--tcp_ : show TCP ports only
   - _-l_ / _--listening_ : show listening / open ports only
   - _-p_ / _--programs_ : show PID / app name




## Configuration


### Configuration files and directories related to network settings

- `/etc/netplan/01-network-manager-all.yaml`
- `/etc/udev/rules.d/`
- `/var/lib/udev/`


### Static canonical predictable names of network interfaces

- add `net.ifnames=0  biosdevname=0` options to `GRUB_CMDLINE_LINUX_DEFAULT` variable inside `/etc/default/grub` configuration file
- optionally, to name interface based on MAC address, add `udev` rule like:
```
SUBSYSTEM=="net", ACTION=="add", DRIVERS=="?*", ATTR{address}=="__:__:__:__:__:__", ATTR{dev_id}=="0x0", ATTR{type}=="1", KERNEL=="eth*", NAME="em0"
```




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




## iptables


### open port(s)
```
$ sudo  iptables  -I INPUT   -p ${PROTO}  --dport ${PORT}  -j ACCEPT
$ sudo  iptables  -I OUTPUT  -p ${PROTO}  --sport ${PORT}  -j ACCEPT
```
 * _**PROTO**_ - protocol: _tcp_ or _udp_
 * _**PORT**_  - port number




## SSH


### Client common options

#### Basic

 - `-i` : /path/to/private/key
 - `-C` : enable data compression
 - `-f` : run in background
 - `-N` : do not execute [remote shell] command
 - `-n` : disable reading from _STDIN_
 - `-q` : enable quite output and show only fatal errors
 - `-T` : disable pseudo-terminal allocation
 - `-D` : [bind_host:]port

#### Extended

 - `-o PubkeyAuthentication=    ` : `yes`/`no` - use keys' pair authentication method
 - `-o PasswordAuthentication=  ` : `yes`/`no` - use password authentication method
 - `-o StrictHostKeyChecking=   ` : `yes`/`no` - use interactive request (`yes`) or just add host to `known_hosts` (`no`)
 - `-o ServerAliveInterval=     ` : `N`        - use `N` _seconds_ keep-alive _"heart beat"_


### Permissions

Permissions for files in `~/.ssh` directory[^perms]:
```
drwxr-xr-x / 755  ..
drwx------ / 700  .
-rw------- / 600  config
-rw------- / 600  authorized_keys
-rw------- / 600  known_hosts
-rw------- / 600  *.priv
-rw-r--r-- / 644  *.pub
```
[^perms]: https://superuser.com/questions/215504/permissions-on-private-key-in-ssh-folder


### Helpers

#### ssh-keygen

`$ ssh-keygen [-H] -F example.com`
 -  **locally** detect fingerprint line for `example.com` host in `~/.ssh/known_hosts` config file and print it (if any)
 - `-H`: hide the line number info

`$ ssh-keygen  -R  exampe.com
 - remove fingerprint line for `exampe.com` host from `~/.ssh/known_hosts` config file (if any)

#### ssh-keyscan

`$ ssh-keyscan [-H] example.com`
 - **remotely** scan `example.com` host for fingerprint line and print it (if any)
 - `-H`: hash host name


### Direct SOCKS proxy

 - pick port number for proxy and set it:
```
$ PORT=1234
```
 - set packet forwarding:
```
$ echo  1  |  sudo  tee  /proc/sys/net/ipv4/ip_forward
```
 - optionally, set `iptables` rules for _**IP**_ client address only:
```
$ sudo  iptables  -A INPUT  --src ${IP}  -p tcp  --dport ${PORT}  -j ACCEPT
$ sudo  iptables  -A INPUT               -p tcp  --dport ${PORT}  -j REJECT
```
 - run `ssh` session:
```
$ ssh  -N  -D  0.0.0.0:${PORT}  localhost
```
 - optionally, to get _IP_ address of a server:
```
$ curl  ifconfig.me
```
 - set web proxy setting in browser/environment on client IP as `IP/HOSTNAME:PORT`


### Reverse SOCKS proxy

 - pick port number for route and set it:
```
$ PORT_ROUTE=1234
```
 - pick port number for proxy and set it:
```
$ PORT_PROXY=5678
```
 - run `ssh` session on _NAT gateway_ host:
```
$ ssh  -R  ${PORT_ROUTE}:localhost:22  user@example.com
```
 - run `ssh` session on _example_ host:
```
$ ssh  -p ${PORT_ROUTE}  localhost  -D  ${PORT_PROXY}
```
 - test the connection from _example_ host:
```
$ curl  http://portquiz.net
Port 80 test successful!
Your IP: <IP address of example.com>

$ curl  --socks5 localhost:${PORT_PROXY}  http://portquiz.net
Port 80 test successful!
Your IP: <IP address of NAT gateway>
```


### Reverse SSH tunnel

 - pick port number for tunnel and set it:
```
$ PORT_TUNNEL=4321
```
 - run `ssh` session on _NAT gateway_ host:
```
$ ssh  -N  -R  ${PORT_TUNNEL}:localhost:22  user@example.com  -p 22
```
 - run `ssh` client connecting back to _NAT gateway_ host from _example_ host:
```
$ ssh  client@localhost  -p ${PORT_TUNNEL}
```


### Port only access

DRAFT

 - add user: `$ sudo  useradd  -m  mysshuser`
 - set password
 - populate .ssh & set permissions
 - add section to sshd:
```
AllowUsers mysshuser

Match User mysshuser
	AllowTcpForwarding yes
	PermitTunnel yes
	GatewayPorts yes
	AllowAgentForwarding no
	PermitOpen localhost:8080
	ForceCommand echo 'No no no!'
```
 - restart sshd and test local ssh login using keys: `$ sudo  service  ssh  restart ; ssh  mysshuser@localhost  -i /home/mysshuser/.ssh/id_rsa`
 - edit etc/passwd to set bin/false
```
mysshuser:...:/bin/false
```
 - edit etc/shadow to disable password
```
mysshuser:!:1...`
```




## Web


### Network throughput test

As a quick'n'dirty solution:
 - on **server**:
   - install & run _any_ web server, i.e.: `$ python3.8  -m http.server  8080  -d www/html`
   - go to its `www/html` directory
   - run (tweak command for desirable size to test): `$ sudo  touch  file.bin  &&  sudo  truncate  -s 1GB  file.bin`
 - on **client**:
   - run `$ wget  -v  -O /dev/null  https://server/file.bin` or `$ curl  -o /dev/null  http://server:8080/file.bin`


### PAC

_Proxy Auto Configuration_ file is a special server-side file which allows redirecting client requests for particular hosts.
To use this feature:
 - create & put sample file like `inet.pac` to `www/html` directory on **server** with this content:
```
function FindProxyForURL(url, host) {
	alert("url  = " + url);
	alert("host = " + host);
	if (
		shExpMatch(host, "*test.org*")  ||
		shExpMatch(host, "*example.com*")
	) {
		return "SOCKS host:port";
	} else {
		return "DIRECT";
	}
}
```
 - configure browser on client to use _proxy autoconfiguration_ and provide the setting line as `https://server/inet.pac`




