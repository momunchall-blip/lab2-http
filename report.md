\### A1. DNS для example.com

Команда: nslookup example.com

DNS-сервер: 192.168.0.1 (роутер).

Адреса: 2606:4700:10::6814:179a, 2606:4700:10::ac42:93f3 (IPv6),

104.20.23.154, 172.66.147.243 (IPv4).

Вывод: DNS переводит имя сайта в IP-адрес, потому что для соединения

с сервером браузеру нужен именно адрес, а не имя.



\### A2. Сравнение с google.com

Команда: nslookup google.com

Адреса: 4 IPv6 (2a00:1450:4001:c0f::...) и 6 IPv4 (142.251.14.100 и др.).

У example.com 4 адреса (2 IPv6 + 2 IPv4), у google.com 10 (4 IPv6 + 6 IPv4).

Вывод: у обоих сайтов есть IPv4 и IPv6. Крупному сайту нужно несколько

адресов, чтобы распределять нагрузку между серверами и продолжать

работать при отказе одного из них.



\### A3. ping

Команда: ping -n 4 example.com

Результат: отправлено 4, получено 4, потеряно 0 (0% потерь),

время ответа min 5 мс, max 7 мс, среднее 6 мс.

Вывод: сервер отвечает быстро и стабильно, вероятно, он расположен

близко (сайт работает через CDN).



\### A4. tracert

Команда: tracert example.com

Результат: 7 прыжков: домашний роутер, сеть провайдера (aknet.kg),

узел обмена в Бишкеке (elcat.kg), внешняя сеть, сервер 104.20.23.154.

Времена на всех прыжках 1-11 мс, резкого роста задержки нет.

Вывод: маршрут короткий, сервер находится недалеко; пик 11 мс на

5-м прыжке единичный.



\## A5. Разбор адреса



https://user@uni.example:8443/api/v1/students/42?group=IS-21\&sort=asc#grades



\- схема: https

\- пользователь: user

\- хост: uni.example

\- порт: 8443

\- путь: /api/v1/students/42

\- параметры запроса: group=IS-21\&sort=asc

\- фрагмент: grades



\## Задание B. Анализ запросов в DevTools



\### B1. Общая картина (bbc.com)

Число запросов: 328

Передано: 6.3 MB (ресурсов 14.3 MB)

Время загрузки (Finish): 35.31 с

Больше всего запросов типа: ping, затем gif.

Вывод: это аналитика и рекламные трекеры. Сама страница весит

немного, но к сторонним серверам идёт очень много мелких запросов.

Скриншот: screenshots/b1-overview.png



\### B2-B3. Запрос 1: Document



URL: https://www.bbc.com/

Метод: GET

Статус: 200 OK

Remote Address: 151.101.192.81:443



Заголовки запроса:

\- :authority: www.bbc.com

\- Accept: text/html,application/xhtml+xml,...

\- Accept-Language: ru-RU,ru;q=0.9,en-US;q=0.8,en;q=0.7

\- Accept-Encoding: gzip, deflate, br, zstd



Заголовки ответа:

\- Cache-Control: private, stale-if-error=90, stale-while-revalidate=30, max-age=0, must-revalidate

\- Content-Encoding: gzip

\- Accept-Ranges: bytes

\- Alt-Svc: h3=":443" (сервер предлагает HTTP/3)



Cookies (только имена): optimizelyEndUserId, optimizelySession,

ckns\_policy, ckns\_explicit, ckns\_echo\_device\_id



Тело запроса: нет (GET)

Тело ответа: HTML-код страницы (проверить на вкладке Response)



Скриншот: screenshots/b2-doc.png



\### Запрос 2: ресурс (изображение)



URL: https://ichef.bbci.co.uk/news/320/cpsprodpb/0a5b/live/25d71060-c2fa-11f1-b8c6-6d610e41a5d9.jpg.webp

Метод: GET

Статус: 200 OK

Remote Address: 184.24.144.174:443



Заголовки запроса:

\- :authority: ichef.bbci.co.uk

\- Accept: image/avif,image/webp,image/apng,image/svg+xml,image/\*,\*/\*;q=0.8

\- Accept-Encoding: gzip, deflate, br, zstd

\- Referer: https://www.bbc.com/



Заголовки ответа:

\- Content-Type: image/webp

\- Content-Length: 11320

\- Cache-Control: max-age=31536000

\- Access-Control-Allow-Origin: \*



Cookies: нет (запрос к другому домену, вкладка Cookies пуста)

Тело запроса: нет (GET)

Тело ответа: изображение WebP, около 11 kB (видно на вкладке Preview)



Скриншот: screenshots/b2-resource.png

