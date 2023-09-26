# Ethernet protocol & gnulinux network configuration notes

# DRAFT

ip address
ip link


networkctl status enp0s25


echo 1 | sudo tee /proc/sys/net/ipv4/ip_forward

sudo ip link set enp0s25 promisc on

sudo service systemd-networkd status

/etc/netplan/01-network-manager-all.yaml
sudo netplan apply

/etc/udev/rules.d/
/var/lib/udev/


nmcli con up id eth.tap

tcpdump -i eth1 -nn -XX -vvv -s0 -S -e dst 192.168.1.1


- sudo tcpdump -vvvnnvXS -s 0 port 1234 -i lo
- nc -l localhost 1234
- nc localhost 1234


sudo ethtool enp0s25
SIOCETHTOOL

# TBA:


