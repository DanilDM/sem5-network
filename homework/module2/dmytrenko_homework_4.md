## 1

```
[root@DESKTOP-I1PMRS5 module1]# ip route
default via 192.168.128.1 dev eth0 proto kernel onlink
192.168.128.0/20 dev eth0 proto kernel scope link src 192.168.142.13
```

- Default route: default via 192.168.128.1 dev eth0
- Пояснення: Запис `default via 192.168.1.1` означає, що це маршрут за замовчуванням. Якщо в таблиці маршрутизації немає конкретнішого маршруту до мережі призначення, пакети надсилаються через шлюз 192.168.1.1.
- Мережевий інтерфейс: eth0

## 2

```
[root@DESKTOP-I1PMRS5 module1]# iptables -L -n
Chain INPUT (policy ACCEPT)
target     prot opt source               destination

Chain FORWARD (policy ACCEPT)
target     prot opt source               destination

Chain OUTPUT (policy ACCEPT)
target     prot opt source               destination
```

1. INPUT, FORWARD, OUTPUT
2. ACCEPT
3. INPUT відповідає за вхідний трафік до самого пристрою, FORWARD за трафік що проходить через пристрій до інших мереж

## 3

```
[root@DESKTOP-I1PMRS5 module1]# docker run -d --name test-net nginx
Unable to find image 'nginx:latest' locally
latest: Pulling from library/nginx
f7e1db57b58b: Pull complete
807ab930f3a0: Pull complete
ecc510c1e359: Pull complete
947d22046304: Pull complete
04bdbc7ea6b5: Pull complete
1efc53192927: Pull complete
8020170dc65f: Pull complete
a1b85ab28c90: Download complete
c329293b1d90: Download complete
Digest: sha256:f9ea18bfa4fad859e1ed38259d711da7ccad2c3516e875cec3351a57c859f571
Status: Downloaded newer image for nginx:latest
3ff0a4b36d0c3e0c47c540ab70eb12d2ecfa07c0e275b54679abf4b472ee3a85
[root@DESKTOP-I1PMRS5 module1]#
[root@DESKTOP-I1PMRS5 module1]# docker ps
CONTAINER ID   IMAGE     COMMAND                  CREATED         STATUS         PORTS     NAMES
3ff0a4b36d0c   nginx     "/docker-entrypoint.…"   8 seconds ago   Up 7 seconds   80/tcp    test-net
[root@DESKTOP-I1PMRS5 module1]# docker inspect -f '{{.NetworkSettings.Networks.bridge.IPAddress}}' test-net
172.17.0.2
```

1. `172.17.0.2`
2. `bridge`
3. Контейнер отримує приватний IP у власному мережевому просторі (network namespace).

## 4

```
[root@DESKTOP-I1PMRS5 module1]# docker run -d -p 8080:80 --name web-server nginx
a7cfef21de61a85377154dc40498e0c25c405a59b9b7035e175a672e1aa61e8f
[root@DESKTOP-I1PMRS5 module1]# docker ps
CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS         PORTS                                     NAMES
a7cfef21de61   nginx     "/docker-entrypoint.…"   10 seconds ago   Up 9 seconds   0.0.0.0:8080->80/tcp, [::]:8080->80/tcp   web-server
```
1.
```
[root@DESKTOP-I1PMRS5 module1]# curl -i localhost:8080
HTTP/1.1 200 OK
Server: nginx/1.31.6
Date: Wed, 14 Oct 2026 11:37:29 GMT
Content-Type: text/html
Content-Length: 896
Last-Modified: Tue, 15 Sep 2026 12:54:15 GMT
Connection: keep-alive
ETag: "6aa93ff7-380"
Accept-Ranges: bytes

<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
...
```
2.
```
[root@DESKTOP-I1PMRS5 module1]# iptables -t nat -L -n
Chain PREROUTING (policy ACCEPT)
target     prot opt source               destination
DOCKER     all  --  0.0.0.0/0            0.0.0.0/0            ADDRTYPE match dst-type LOCAL

Chain INPUT (policy ACCEPT)
target     prot opt source               destination

Chain OUTPUT (policy ACCEPT)
target     prot opt source               destination
DOCKER     all  --  0.0.0.0/0           !127.0.0.0/8          ADDRTYPE match dst-type LOCAL

Chain POSTROUTING (policy ACCEPT)
target     prot opt source               destination
MASQUERADE  all  --  172.17.0.0/16        0.0.0.0/0

Chain DOCKER (2 references)
target     prot opt source               destination
DNAT       tcp  --  0.0.0.0/0            0.0.0.0/0            tcp dpt:8080 to:172.17.0.2:80
```
Chain: `DOCKER`, target: `DNAT`

3. Параметр `-p 8080:80` публікує порт 80 контейнера на порту 8080 хоста. Docker використовує DNAT перенаправлення трафіку. Клієнт може звернутися до Nginx через адресу хоста, посередньо (не беспосередньо) до внутрішньої IP-адреси контейнера.

## 5

1. Усі контейнери одного Pod спільно використовують мережевий простір імен (network namespace).

2. IP самого Pod не гарантує стабільності, він може бути видалений, перезапущений або створений заново, отримуючи інше IP.

3. kube-proxy налаштовує мережеві правила, які спрямовують трафік із ClusterIP до відповідних Pod. Він також оновлює ці правила при зміні кінцевих точок сервісу, що забезпечує стабільний доступ до застосунків.