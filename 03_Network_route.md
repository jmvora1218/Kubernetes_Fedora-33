``` bash
# Network and route configuration 

[jv@fedora-33 SimpleWebKV]$ ifconfig
br-bdbc4d641496: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.49.1  netmask 255.255.255.0  broadcast 192.168.49.255
        inet6 fe80::42:e3ff:fe30:4a7c  prefixlen 64  scopeid 0x20<link>
        ether 02:42:e3:30:4a:7c  txqueuelen 0  (Ethernet)
        RX packets 37  bytes 3311 (3.2 KiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 52  bytes 29506 (28.8 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

docker0: flags=4099<UP,BROADCAST,MULTICAST>  mtu 1500
        inet 172.17.0.1  netmask 255.255.0.0  broadcast 172.17.255.255
        inet6 fe80::42:d4ff:fe1d:5104  prefixlen 64  scopeid 0x20<link>
        ether 02:42:d4:1d:51:04  txqueuelen 0  (Ethernet)
        RX packets 37  bytes 3311 (3.2 KiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 52  bytes 29506 (28.8 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

ens33: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.2.21  netmask 255.255.255.0  broadcast 192.168.2.255
        inet6 fe80::1da6:b517:a560:e8ea  prefixlen 64  scopeid 0x20<link>
        ether 00:0c:29:f2:15:1b  txqueuelen 1000  (Ethernet)
        RX packets 1135504  bytes 1561512516 (1.4 GiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 240146  bytes 106363633 (101.4 MiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10<host>
        loop  txqueuelen 1000  (Local Loopback)
        RX packets 44386  bytes 514582868 (490.7 MiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 44386  bytes 514582868 (490.7 MiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

vethe6d988e: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet6 fe80::24aa:84ff:fec6:9ed2  prefixlen 64  scopeid 0x20<link>
        ether 26:aa:84:c6:9e:d2  txqueuelen 0  (Ethernet)
        RX packets 52476  bytes 4514720 (4.3 MiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 66304  bytes 868999746 (828.7 MiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

virbr0: flags=4099<UP,BROADCAST,MULTICAST>  mtu 1500
        inet 192.168.122.1  netmask 255.255.255.0  broadcast 192.168.122.255
        ether 52:54:00:3b:be:c2  txqueuelen 1000  (Ethernet)
        RX packets 0  bytes 0 (0.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 0  bytes 0 (0.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

[jv@fedora-33 SimpleWebKV]$ route
Kernel IP routing table
Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
default         _gateway        0.0.0.0         UG    100    0        0 ens33
172.17.0.0      0.0.0.0         255.255.0.0     U     0      0        0 docker0
192.168.2.0     0.0.0.0         255.255.255.0   U     100    0        0 ens33
192.168.49.0    0.0.0.0         255.255.255.0   U     0      0        0 br-bdbc4d641496
192.168.122.0   0.0.0.0         255.255.255.0   U     0      0        0 virbr0

sudo iptables -I FORWARD -o br-bdbc4d641496 -d 192.168.49.2 -j ACCEPT
sudo iptables -t nat -I PREROUTING -p tcp --dport 32210 -j DNAT --to 192.168.49.2:32210
sudo iptables -I FORWARD -o br-bdbc4d641496 -d 192.168.2.21 -j ACCEPT
sudo iptables -t nat -A POSTROUTING -s 192.168.49.0/24 -j MASQUERADE
sudo iptables -A FORWARD -o br-bdbc4d641496 -m state --state RELATED,ESTABLISHED -j ACCEPT
sudo iptables -A FORWARD -i br-bdbc4d641496 -o ens33 -j ACCEPT
sudo iptables -A FORWARD -i br-bdbc4d641496 -o lo -j ACCEPT


Hello from SimpleKV[jv@fedora-33 SimpleWebKV]$ sudo iptables --table nat --list
[sudo] password for jv:
Chain PREROUTING (policy ACCEPT)
target     prot opt source               destination
DNAT       tcp  --  anywhere             anywhere             tcp dpt:32210 to:192.168.49.2:32210

Chain INPUT (policy ACCEPT)
target     prot opt source               destination

Chain OUTPUT (policy ACCEPT)
target     prot opt source               destination

Chain POSTROUTING (policy ACCEPT)
target     prot opt source               destination
MASQUERADE  all  --  192.168.49.0/24      anywhere
[jv@fedora-33 SimpleWebKV]$


[jv@fedora-33 SimpleWebKV]$ curl http://192.168.49.2:32210/
Hello from SimpleKV

```