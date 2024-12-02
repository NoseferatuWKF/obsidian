client/server messaging
```
Client request:

GET /hello.txt HTTP/1.1\r\n
User-Agent: curl/7.64.1\r\n
Host: www.example.com\r\n
Accept-Language: en, mi\r\n

Server response:

HTTP/1.1 200 OK\r\n
Date: Mon, 27 Jul 2009 12:28:53 GMT\r\n
Server: Apache\r\n
Last-Modified: Wed, 22 Jul 2009 19:15:56 GMT\r\n
ETag: 34aa387-d-1568eb00\r\n
Accept-Ranges: bytes\r\n
Content-Length: 51\r\n
Vary: Accept-Encoding\r\n
Content-Type: text/plain\r\n\r\n

Hello World! My content includes a trailing CRLF.
```

CORS Headers
```
Access-Control-Allow-Origins
Access-Control-Allow-Methods
Access-Control-Allow-Headers
```

[HTTP Security Response Headers Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/HTTP_Headers_Cheat_Sheet.html)

status codes overview
```
1xx (Informational): The request was received, continuing process
2xx (Successful): The request was successfully received, understood, and accepted
3xx (Redirection): Further action needs to be taken in order to complete the request
4xx (Client Error): The request contains bad syntax or cannot be fulfilled
5xx (Server Error): The server failed to fulfill an apparently valid request
```

RFCs

[RFC-2616](https://datatracker.ietf.org/doc/html/rfc2616) (Draft Standard - June 1999)
[RFC-9110](https://datatracker.ietf.org/doc/html/rfc9110) (Internet Standard - June 2022)
[RFC-9113](https://datatracker.ietf.org/doc/html/rfc9113) (Proposed Standard HTTP/2 - June 2022)