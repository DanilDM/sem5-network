## 1 

```bash
[root@DESKTOP-I1PMRS5 module1]# curl -I -L http://archlinux.org/nothing
HTTP/1.1 301 Moved Permanently
Server: nginx
Date: Wed, 14 Oct 2026 10:21:38 GMT
Content-Type: text/html
Content-Length: 162
Connection: keep-alive
Location: https://archlinux.org/nothing

HTTP/2 404
server: nginx
date: Wed, 14 Oct 2026 10:21:38 GMT
content-type: text/html; charset=utf-8
content-length: 3616
content-security-policy: base-uri 'none'; form-action 'self'; default-src 'self'; script-src 'self'; img-src 'self' data:; frame-ancestors 'none'
x-content-type-options: nosniff
referrer-policy: strict-origin
cross-origin-opener-policy: same-origin
x-frame-options: DENY
vary: Cookie
strict-transport-security: max-age=31536000; includeSubdomains; preload
```

**Статус-код**

- `301 Moved Permanently` - постійне перенаправлення на іншу адресу
- `404 Not Found` - запитувану сторінку не знайдено на сервері

**Заголовок Location**

```http
Location: https://archlinux.org/nothing
```

Вказує на перенаправлення з HTTP на HTTPS, для захищеного з'єднання. Після перенаправлення сервер повернув статус-код `404 Not Found`, оскільки сторінка `/nothing` не існує.

**Версія протоколу**

- `HTTP/1.1` для початкової відповіді з кодом 301
- `HTTP/2` для кінцевої відповіді з кодом 404

## 2

```bash
[root@DESKTOP-I1PMRS5 module1]# curl -i -X POST https://reqres.in/api/users \
  -H "Content-Type: application/json" \
  -d '{"name":"Student","job":"Tester"}'
HTTP/2 201
date: Wed, 14 Oct 2026 10:27:00 GMT
content-type: application/json; charset=utf-8
content-length: 490
access-control-allow-origin: *
cache-control: no-store
cross-origin-opener-policy: same-origin
cross-origin-resource-policy: cross-origin
etag: W/"1ea-Bq/5h+d//un7JQRedyRkPTYDY24"
nel: {"report_to":"heroku-nel","response_headers":["Via"],"max_age":3600,"success_fraction":0.01,"failure_fraction":0.1}
origin-agent-cluster: ?1
ratelimit-limit: 20
ratelimit-policy: 20;w=60
ratelimit-remaining: 19
ratelimit-reset: 60
referrer-policy: strict-origin-when-cross-origin
report-to: {"group":"heroku-nel","endpoints":[{"url":"https://nel.heroku.com/reports?s=1Cm%2FOfi3%2Ft9Fc0b687q2mFzxfqsv1Ty9mqzUYJdtnBg%3D\u0026sid=c4c9725f-1ab0-44d8-820f-430df2718e11\u0026ts=1791973620"}],"max_age":3600}
reporting-endpoints: heroku-nel="https://nel.heroku.com/reports?s=1Cm%2FOfi3%2Ft9Fc0b687q2mFzxfqsv1Ty9mqzUYJdtnBg%3D&sid=c4c9725f-1ab0-44d8-820f-430df2718e11&ts=1791973620"
server: cloudflare
strict-transport-security: max-age=31536000; includeSubDomains
via: 1.1 heroku-router
x-content-type-options: nosniff
x-dns-prefetch-control: off
x-download-options: noopen
x-frame-options: DENY
x-permitted-cross-domain-policies: none
x-powered-by: ReqRes.in - Deploy backends in 30 seconds
x-ratelimit-limit: 40
x-ratelimit-remaining: 39
x-ratelimit-reset: 1792022400
x-reqres-docs: https://reqres.in
x-reqres-message: This API is powered by ReqRes. Deploy your own backend in 30 seconds!
x-reqres-templates: https://app.reqres.in/templates
x-reqres-tip: Need more than /api/users? Build custom collections - app.reqres.in/dashboard
x-reqres-upgrade: https://app.reqres.in/upgrade
x-request-id: f94f1fa1-0677-1f10-3032-d1d501a61e7d
x-robots-tag: noindex, nofollow
x-xss-protection: 0
cf-cache-status: DYNAMIC
cf-ray: a4a5e399da01f4c8-OTP

{"name":"Student","job":"Tester","id":"710","createdAt":"2026-10-14T10:27:00.896Z","_meta":{"powered_by":"ReqRes","docs_url":"https://app.reqres.in/documentation","upgrade_url":"https://app.reqres.in/upgrade","example_url":"https://app.reqres.in/examples/notes-app","variant":"v1_b","message":"This is a read-only demo endpoint. Sign up to create your own collections with full CRUD and auth.","cta":{"label":"Get started","url":"https://app.reqres.in/upgrade"},"context":"legacy_success"}}
```

- Результат = 201 Created
- Різниця = POST - передача даних на сервер, GET - отримання даних з сервера

Сервер може використовувати кешування для популярних GET запитів

## 3

```
[root@DESKTOP-I1PMRS5 module1]# openssl s_client -connect archlinux.org:443
...
subject=CN=archlinux.org
issuer=C=US, O=Let's Encrypt, CN=YE1
...
New, TLSv1.3, Cipher is TLS_AES_256_GCM_SHA384
Protocol: TLSv1.3
...
```

Issuer (ким видано сертифікат): Let's Encrypt, CN=YE1

Узгоджений протокол TLS: TLSv1.3

Обраний шифр (Cipher Suite): TLS_AES_256_GCM_SHA384

## 4

```
[root@DESKTOP-I1PMRS5 module1]# curl -v -o /dev/null https://www.google.com 2>&1 | grep -i "set-cookie"
< set-cookie: AEC=Aaa9EJpEriejdxChnK744kNTh6eDnG8bkhkxTulZ3ZQW-wNECJdqlWKa-lE; expires=Mon, 12-Apr-2027 10:42:04 GMT; path=/; domain=.google.com; Secure; HttpOnly; SameSite=lax
< set-cookie: __Secure-ENID=Co8CCAISwQEBAQxeRQwql5Fh0XAFrvyCeq7-rZ3BzT5EAoUquKizv7a2Sk7oe44KkrQ3Ny4RB6__1Z6IGXHElM2KSDxwSK-MkGw8tFUQkCWrI3qc7DGEuW7-1ZQG301mhnikKD8LJBwJJVg_57ATBvPQqbp0supQCAmGSoMajROvZ8_kgdALolMyQdMNVjdXWdN7nN2PnW8tNRDSiqR8AqhsNfrEXnzMu706wOfuFh7VlcRjaRJNAxDqjl3uoSNJj3Zo43jaD1McKAIyRQF9bMSqWrLzdeTD-EQPpR9IcXr3fm3EWOUnVmd3JLP0NncE29jBlxgZOi_7zTkwIx_IFHkukRpLcVnAzKXBImRvhYI4BQ; expires=Sun, 14-Nov-2027 03:00:22 GMT; path=/; domain=.google.com; Secure; HttpOnly; SameSite=lax
```

|Параметр|Значення|
|---|---|
|Cookie ID|Co8CCAISwQEB...|
|HttpOnly|Так|
|Secure|Так|
|SameSite|lax|

`HttpOnly` захищає Cookie від викрадення через JavaScript, що виконується в браузері внаслідок атаки типу Cross-Site Scripting.

`Secure` дозволяє браузеру надсилати Cookie лише через захищене з'єднання.

## 5

**Сценарій А**: Оновлення графіків цін у браузері в реальному часі.

Обраний протокол: **WebSocket**.

- Двосторонній зв’язок: сервер може надсилати оновлення цін браузеру одразу після їх зміни, без постійних HTTP-запитів від клієнта.
- Низька затримка: після встановлення з’єднання дані передаються без необхідності створювати новий HTTP-запит для кожного оновлення.
- Економія ресурсів: зменшується кількість службових даних і навантаження на сервер порівняно з постійним опитуванням (polling).
- Підтримка реального часу: відображення актуальних котирувань і графіків цін.

**Сценарій Б**: Взаємодія між внутрішніми сервісами білінг та облік акцій.

Обраний протокол: **gRPC**.

- Компактний формат даних: Protocol Buffers (Protobuf) передає структуровані дані компактніше за JSON.
- Строга типізація: контракти сервісів описуються у .proto файлах, що спрощує інтеграцію та зменшує ризик помилок.
- Підтримка різних моделей взаємодії: доступні звичайні запити-відповіді, потокова передача даних і двосторонній стримінг.