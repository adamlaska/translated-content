---
title: HTTP сессия
slug: Web/HTTP/Guides/Session
---

Так как HTTP — это клиент-серверный протокол, HTTP сессия состоит из трёх фаз:

1. Клиент устанавливает TCP соединения (или другое соединение, если не используется TCP транспорт).
2. Клиент отправляет запрос и ждёт ответа.
3. Сервер обрабатывает запрос и посылает ответ, в котором содержится код статуса и соответствующие данные.

Начиная с версии HTTP/1.1, после третьей фазы соединение не закрывается, так как клиенту позволяется инициировать другой запрос. То есть, вторая и третья фазы могут повторяться.

## Установка соединения

Так как HTTP это клиент-серверный протокол, соединение всегда устанавливается клиентом. Открыть соединение в HTTP — значит установить соединение через соответствующий транспорт, обычно TCP.

В случае с TCP, в качестве порта HTTP сервера по умолчанию на компьютере используется порт 80, хотя другие также часто используются, например 8000 или 8080. URL загружаемой страницы содержит доменное имя и порт, который можно и не указывать если он соответствует порту по умолчанию.

> [!NOTE]
> Клиент-серверная модель не позволяет серверу посылать данные клиенту без явного запроса этих данных. Чтобы обойти эту проблему, веб разработчики используют различные техники: периодически пингуют сервер используя [XMLHTTPRequest](/en-US/DOM/XMLHttpRequest) Javascript объект, HTML [WebSockets API](/en-US/WebSockets), или похожие протоколы.

## Отправка запроса клиента

Когда соединение установлено user-agent может послать запрос. (user-agent это обычно веб браузер, но может им не быть) Клиентский запрос это текстовые директивы, разделённые между собой при помощи CRLF (переноса строки). Сам запрос включает в себя три блока:

1. Первые строки содержат метод запроса и его параметры:
   - путь к документу - абсолютная URL без указания протокола и доменного имени
   - версию HTTP протокола

2. Каждая последующая строка представляет собой HTTP заголовок и передаёт серверу некоторую информацию о типах предпочитаемых данных (например, какой язык , какие MIME типы) или инструкции меняющие поведение сервера (например, не отправлять ответ, если он уже в кеше) . Эти HTTP заголовки формируют блок, который заканчивается пустой строкой.
3. Последний блок является не обязательным и содержит дополнительные данные. По большей части используется методом POST.

### Примеры запросов

Получаем главную страницу developer.mozilla.org, [http://developer.mozilla.org/](/), и говорим серверу, что user-agent предпочитает страницу на французском, если это возможно:

```
GET / HTTP/1.1
Host: developer.mozilla.org
Accept-Language: fr
```

Обращаем внимание на пустую строку в конце, которая отделяет блок данных от блока заголовков. Так как в запросе отсутствует `Content-Length:` HTTP заголовок, блок с данными пуст и сервер может начать обработку запроса, как только получит пустую строку, означающую конец заголовков.

Отправляем результат сабмита формы:

```
POST /contact_form.php HTTP/1.1
Host: developer.mozilla.org
Content-Length: 64
Content-Type: application/x-www-form-urlencoded

name=Joe%20User&request=Send%20me%20one%20of%20your%20catalogue
```

### Методы запроса

HTTP определяет набор [методов запроса](/ru/docs/Web/HTTP/Reference/Methods) с указанием желаемого действие на ресурсе. Хотя они также могут быть и существительными, эти запросы методы иногда называют HTTP-командами. Наиболее распространённые запросы `GET` и `POST`:

- {{HTTPMethod("GET")}} используется для запроса содержимого указанного ресурса. Запрос с использованием `GET` должен только получать данные.
- {{HTTPMethod("POST")}} метод отправляет данные на сервер, так что он может изменять своё состояние. Этот метод часто используется для [HTML форм](/ru/docs/Learn_web_development/Extensions/Forms).

## Структура ответа от сервера

После того как присоединённый агент отправил свой запрос, веб сервер обрабатывает его и отправляет ответ. По аналогии с клиентским запросом, ответ сервера — это текстовые директивы разделённые между собой CRLF, сгруппированные в три разных блока:

1. Первая строка — строка статуса, состоит из подтверждения используемой HTTP версии и статуса запроса (и его значения в виде, понятном человеку).
2. Последующие строки представляют собой HTTP заголовки, дающие клиенту некоторую информацию о посылаемых данных (прим. тип, размер, алгоритм сжатия, подсказки по кешированию). Так же как и в случае клиентского запроса, эти HTTP заголовки формируют блок, заканчивающийся пустой строкой.
3. Последний блок содержит данные (если таковые имеются).

### Примеры ответов

Успешное получение веб страницы:

```
HTTP/1.1 200 OK
Date: Sat, 09 Oct 2010 14:28:02 GMT
Server: Apache
Last-Modified: Tue, 01 Dec 2009 20:18:22 GMT
ETag: "51142bc1-7449-479b075b2891b"
Accept-Ranges: bytes
Content-Length: 29769
Content-Type: text/html

<!DOCTYPE html... (здесь идут 29769 байтов запрошенной веб-страницы)
```

Сообщение о том, что запрашиваемый ресурс был перемещён:

```
HTTP/1.1 301 Moved Permanently
Server: Apache/2.2.3 (Red Hat)
Content-Type: text/html; charset=iso-8859-1
Date: Sat, 09 Oct 2010 14:30:24 GMT
Location: https://developer.mozilla.org/ (это новый адрес запрошенного ресурса, ожидается, что клиент запросит его)
Keep-Alive: timeout=15, max=98
Accept-Ranges: bytes
Via: Moz-Cache-zlb05
Connection: Keep-Alive
X-Cache-Info: caching
X-Cache-Info: caching
Content-Length: 325 (Контент содержит стандартную страницу, которая будет показана, если клиент не может перейти по ссылке)

<!DOCTYPE HTML PUBLIC "-//IETF//DTD HTML 2.0//EN">
<html><head>
<title>301 Moved Permanently</title>
</head><body>
<h1>Moved Permanently</h1>
<p>The document has moved <a href="https://developer.mozilla.org/">here</a>.</p>
<hr>
<address>Apache/2.2.3 (Red Hat) Server at developer.mozilla.org Port 80</address>
</body></html>
```

Сообщение о том, что запрашиваемый ресурс не существует:

```
HTTP/1.1 404 Not Found
Date: Sat, 09 Oct 2010 14:33:02 GMT
Server: Apache
Last-Modified: Tue, 01 May 2007 14:24:39 GMT
ETag: "499fd34e-29ec-42f695ca96761;48fe7523cfcc1"
Accept-Ranges: bytes
Content-Length: 10732
Content-Type: text/html

<!DOCTYPE html... (содержит пользовательскую страницу, помогающую пользователю найти отсутсвующий ресурс)
```

### Коды статусов ответа

[HTTP-коды ответов](/ru/docs/Web/HTTP/Reference/Status) показывают, выполнен ли успешно определённый HTTP-запрос. Ответы сгруппированы в пять классов: информационные ответы, успешные ответы, редиректы, ошибки клиента и ошибки сервера.

- {{HTTPStatus(200)}}: OK. Запрос завершился успехом.
- {{HTTPStatus(301)}}: Moved Permanently. Этот код значит, что URI запрошенного ресурса был изменён.
- {{HTTPStatus(404)}}: Not Found. Сервер не может найти запрошенный ресурс.

## Смотрите также

- [Определение ресурсов в Интернете](/ru/docs/Web/URI)
- [HTTP заголовки](/ru/docs/Web/HTTP/Reference/Headers)
- [Методы HTTP запроса](/ru/docs/Web/HTTP/Reference/Methods)
- [Коды ответа HTTP](/ru/docs/Web/HTTP/Reference/Status)
