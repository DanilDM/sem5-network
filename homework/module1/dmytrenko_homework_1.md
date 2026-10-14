## 1

```bash
[root@DESKTOP-I1PMRS5 module1]# ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet 10.255.255.254/32 brd 10.255.255.254 scope global lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host proto kernel_lo
       valid_lft forever preferred_lft forever
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP group default qlen 1000
    link/ether 00:15:5d:29:f0:cc brd ff:ff:ff:ff:ff:ff
    altname enx00155d29f0cc
    inet 192.168.142.13/20 brd 192.168.143.255 scope global eth0
       valid_lft forever preferred_lft forever
    inet6 fe80::215:5dff:fe29:f0cc/64 scope link proto kernel_ll
       valid_lft forever preferred_lft forever
```

- Інтерфейс = eth0
- тип підключенння = віртуальний ethernet у wsl
- MAC-адреса = 00:15:5d:29:f0:cc
- IP-адресу (вказати рівень моделі OSI) = 192.168.142.13, 3 рівень
- маску (вказати рівень моделі OSI) = 255.255.240.0 (/20), 3 рівень
- gateway = 192.168.128.1 (з другої команди)
- broadcast = 192.168.143.255
- hosts = 4096

## 2

```bash
[root@DESKTOP-I1PMRS5 module1]# ip route
default via 192.168.128.1 dev eth0 proto kernel onlink
192.168.128.0/20 dev eth0 proto kernel scope link src 192.168.142.13
```

протокол (через який отримано IP) = kernel

## 3

```bash
[root@DESKTOP-I1PMRS5 module1]# curl ifconfig.io
86.123.229.92
```

Локальна та публічна адреси відрізняються. Локальна адреса використовується всередині приватної мережі, а публічна забезпечує зв’язок із зовнішнім інтернетом. Домашній роутер використовує технологію NAT для перетворення приватних IP-адрес у публічну.