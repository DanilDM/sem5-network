## 1

```
[root@DESKTOP-I1PMRS5 danylo]# tcpdump -i any port 80 -c 10
tcpdump: WARNING: any: That device doesn't support promiscuous mode
(Promiscuous mode not supported on the "any" device)
tcpdump: verbose output suppressed, use -v[v]... for full protocol decode
listening on any, link-type LINUX_SLL2 (Linux cooked v2), snapshot length 262144 bytes
17:46:41.245332 eth0  Out IP DESKTOP-I1PMRS5.41668 > 104.20.23.154.http: Flags [S], seq 2145728418, win 64240, options [mss 1460,sackOK,TS val 3769101428 ecr 0,nop,wscale 10], length 0
17:46:41.257950 eth0  In  IP 104.20.23.154.http > DESKTOP-I1PMRS5.41668: Flags [S.], seq 3518805345, ack 2145728419, win 65535, options [mss 1400,sackOK,TS val 3019146958 ecr 3769101428,nop,wscale 13], length 0
17:46:41.257984 eth0  Out IP DESKTOP-I1PMRS5.41668 > 104.20.23.154.http: Flags [.], ack 1, win 63, options [nop,nop,TS val 3769101441 ecr 3019146958], length 0
17:46:41.258034 eth0  Out IP DESKTOP-I1PMRS5.41668 > 104.20.23.154.http: Flags [P.], seq 1:76, ack 1, win 63, options [nop,nop,TS val 3769101441 ecr 3019146958], length 75: HTTP: GET / HTTP/1.1
17:46:41.270833 eth0  In  IP 104.20.23.154.http > DESKTOP-I1PMRS5.41668: Flags [.], ack 76, win 16, options [nop,nop,TS val 3019146971 ecr 3769101441], length 0
17:46:41.278404 eth0  In  IP 104.20.23.154.http > DESKTOP-I1PMRS5.41668: Flags [P.], seq 1:932, ack 76, win 16, options [nop,nop,TS val 3019146978 ecr 3769101441], length 931: HTTP: HTTP/1.1 200 OK
17:46:41.278404 eth0  In  IP 104.20.23.154.http > DESKTOP-I1PMRS5.41668: Flags [P.], seq 932:937, ack 76, win 16, options [nop,nop,TS val 3019146978 ecr 3769101441], length 5: HTTP
17:46:41.278471 eth0  Out IP DESKTOP-I1PMRS5.41668 > 104.20.23.154.http: Flags [.], ack 932, win 66, options [nop,nop,TS val 3769101461 ecr 3019146978], length 0
17:46:41.278479 eth0  Out IP DESKTOP-I1PMRS5.41668 > 104.20.23.154.http: Flags [.], ack 937, win 66, options [nop,nop,TS val 3769101461 ecr 3019146978], length 0
17:46:41.278693 eth0  Out IP DESKTOP-I1PMRS5.41668 > 104.20.23.154.http: Flags [F.], seq 76, ack 937, win 66, options [nop,nop,TS val 3769101461 ecr 3019146978], length 0
10 packets captured
12 packets received by filter
0 packets dropped by kernel
```

1. `[S]` `[S.]` `[.]`
2. так
3. `[P]` (Push) - отримувачу слід передати отримані дані без зайвого очікування накопичення буфера
<br>`[.]` (Acknowledge) - підтвердження отримання даних або сегментів

## 2

1. Ця директива передає бекенду реальну IP-адресу клієнта, який звернувся до nginx. Без неї бекенд бачитиме IP-адресу proxy-сервера, а не початкового клієнта. Це важливо для ведення логів, обмеження кількості запитів і визначення джерела підозрілої активності.
2. `proxy_pass` дає змогу nginx приймати зовнішні запити та передавати їх бекенду не відкриваючи порт 3000 прямо в інтернет. Завдяки цьому бекенд можна залишити доступним лише локально або через внутрішню мережу, зменшивши ризик прямих атак.

## 3

**IP Hash**

Цей алгоритм спрямовує запити одного клієнта на той самий сервер на основі його IP-адреси. Завдяки цьому сесія користувача, яка зберігається в локальній пам’яті сервера, залишається доступною під час наступних запитів, що допомагає зберігати стан авторизації користувача без необхідності передавати сесію між серверами, і взагалі є обов'язковим для Stateful додатків. Головна властивість хеш функцій в детерміністичності результату та рівномірності розподілу всіх можливих результатів, що дозволяє роботу обраного підходу. 

## 4 

1. Політика застосовується до всіх Pod з міткою `app: database`
2. Доступ дозволено всім Pod з міткою `app: database`, що знаходяться в спільному namespace з базою даних
3. Якщо зловмисник зламає Pod із `app: frontend`, то він не зможе напряму підключитися до бази даних, оскільки політика дозволяє вхідні з'єднання лише від Pod із міткою `app: backend`

## 5 

1. IDS лише виявляє підозрілий трафік і генерує сповіщення про можливу SQL ін’єкцію, але не блокує пакет автоматично. IPS може заблокувати або відкинути пакет із підозрілим SQL запитом, щоб він не досягнув цільового сервера
2. На рівні edge таку атаку міг би зупинити firewall