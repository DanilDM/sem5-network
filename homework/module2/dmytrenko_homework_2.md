## 1

```bash
[root@DESKTOP-I1PMRS5 module1]# ss -tuln
Netid        State         Recv-Q        Send-Q                Local Address:Port               Peer Address:Port
udp          UNCONN        0             0                    10.255.255.254:53                      0.0.0.0:*
udp          UNCONN        0             0                         127.0.0.1:323                     0.0.0.0:*
udp          UNCONN        0             0                             [::1]:323                        [::]:*
tcp          LISTEN        0             1000                 10.255.255.254:53                      0.0.0.0:*
tcp          LISTEN        0             1                           0.0.0.0:8080                    0.0.0.0:*
```

| № | Протокол | Порт | Стан |
|---|---|---|---|
| 1 | UDP | 53 | UNCONN |
| 2 | UDP | 323 | UNCONN |
| 3 | TCP | 53 | LISTEN |
| 4 | TCP | 8080 | LISTEN |

TCP-порти 53 і 8080 перебувають у стані LISTEN (очікують на вхідні з'єднання). UDP порти 53 і 323 мають стан UNCONN. Було використано сервер netcat: ``` nc -lk 8080 ```.

## 2

```bash
[root@DESKTOP-I1PMRS5 module1]# ss -tulpn
Netid    State     Recv-Q    Send-Q        Local Address:Port       Peer Address:Port   Process
udp      UNCONN    0         0            10.255.255.254:53              0.0.0.0:*
udp      UNCONN    0         0                 127.0.0.1:323             0.0.0.0:*
udp      UNCONN    0         0                     [::1]:323                [::]:*
tcp      LISTEN    0         1000         10.255.255.254:53              0.0.0.0:*
tcp      LISTEN    0         1                   0.0.0.0:8080            0.0.0.0:*       users:(("nc",pid=1151,fd=3))
```

- Процес: nc
- PID: 1151
- Порт: 8080 (TCP)

## 3

За результатами виконання команди ```nc -vz google.com 80``` отримано повідомлення:

```Connection to google.com (142.250.130.102) 80 port [tcp/http] succeeded!```

- Тип з’єднання: TCP
- Порт: 80 (HTTP)
- Результат: з’єднання успішно встановлено

## 4

```bash
[root@DESKTOP-I1PMRS5 module1]# nc -u -vz 8.8.8.8 53
Connection to 8.8.8.8 53 port [udp/domain] succeeded!
[root@DESKTOP-I1PMRS5 module1]# nc -vz 8.8.8.8 53
Connection to 8.8.8.8 53 port [tcp/domain] succeeded!
```

TCP: є підтвердження з’єднання<br>
UDP: немає гарантії відповіді (connection-less)<br>
Різниця: TCP - встановлює сесію, UDP - ні

## 5

```bash
[root@DESKTOP-I1PMRS5 module1]# nslookup archlinux.org
Server:         10.255.255.254
Address:        10.255.255.254#53

Non-authoritative answer:
Name:   archlinux.org
Address: 209.126.35.79
Name:   archlinux.org
Address: 2604:cac0:a104:d::3
```

**Домен**: archlinux.org<br>
**IP**: 209.126.35.79<br>
**DNS сервер**: 10.255.255.254