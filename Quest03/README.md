# Quest 03. 네트워크의 기초

## Introduction
* 이번 퀘스트에서는 인터넷이 어떻게 동작하며, 서버와 클라이언트, 웹 브라우저 등의 역할은 무엇인지 알아보겠습니다.

## Topics
* 서버와 클라이언트, 그리고 웹 브라우저
* 인터넷을 구성하는 여러 가지 프로토콜
  * IP
  * TCP
  * HTTP
* DNS

## Resources
* https://ko.wikipedia.org/wiki/OSI_%EB%AA%A8%ED%98%95
* https://ko.wikipedia.org/wiki/%EC%9D%B8%ED%84%B0%EB%84%B7_%ED%94%84%EB%A1%9C%ED%86%A0%EC%BD%9C
* https://ping.eu/traceroute/
* https://ko.wikipedia.org/wiki/%EC%A0%84%EC%86%A1_%EC%A0%9C%EC%96%B4_%ED%94%84%EB%A1%9C%ED%86%A0%EC%BD%9C
* https://www.wireshark.org/download.html
* https://ko.wikipedia.org/wiki/HTTP
* https://ko.wikipedia.org/wiki/%EB%8F%84%EB%A9%94%EC%9D%B8_%EB%84%A4%EC%9E%84_%EC%8B%9C%EC%8A%A4%ED%85%9C
* https://networking.ringofsaturn.com/Tools/dig.php

## Checklist
* 인터넷은 어떻게 동작하나요? OSI 7 Layer에 입각하여 설명해 보세요.
* 우리가 브라우저의 주소 창에 `www.knowre.com` 을 쳤을 때, 어떤 과정을 통해 노리의 서버 주소를 알게 되나요?

## Quest
* tracert(Windows가 아닌 경우 traceroute) 명령을 통해 `www.google.com` 까지 가는 경로를 찾아 보세요.
  * 어떤 IP주소들이 있나요?
  * 그 IP주소들은 어디에 위치해 있나요?
* Wireshark를 통해 `www.google.com` 으로 요청을 날렸을 떄 어떤 TCP 패킷이 오가는지 확인해 보세요
  * TCP 패킷을 주고받는 과정은 어떻게 되나요?
  * 각각의 패킷의 헤더에 어떤 정보들이 담겨 있나요?
* telnet 명령을 통해 `http://www.google.com/` URL에 HTTP 요청을 날려 보세요.
    telnet google.com 80

    GET / HTTP/1.1

HTTP/1.1 200 OK
Date: Thu, 14 Jan 2021 04:52:47 GMT
Expires: -1
Cache-Control: private, max-age=0
Content-Type: text/html; charset=ISO-8859-1
P3P: CP="This is not a P3P policy! See g.co/p3phelp for more info."
Server: gws
X-XSS-Protection: 0
X-Frame-Options: SAMEORIGIN
Set-Cookie: 1P_JAR=2021-01-14-04; expires=Sat, 13-Feb-2021 04:52:47 GMT; path=/; domain=.google.com; Secure
Set-Cookie: NID=207=JPVb700edY33M9pDiB8m3IaitTzIKvJcg9mT3kj4WTDC7Ol08v8q7QtoUJAH-nWgElos0jKfYueGnMioJduuLtOlW6jISskhlaRWZPvqn3UTbk0-pHCkh21wixjRqa3C_bbtYzo6E60aymQajCTWM1cr85uisZeXaMrIZ_zvnAw; expires=Fri, 16-Jul-2021 04:52:47 GMT; path=/; domain=.google.com; HttpOnly
Accept-Ranges: none
Vary: Accept-Encoding
Transfer-Encoding: chunked

  * 어떤 헤더들이 있나요?
    Date, Expires, Cache-Control, Content-Type, P3P, Server, X-XSS, X-Frame-Options, Set-Cookie, GMT, Set-Cookie, NID, Accept-Ranges, Vary, Transfer-Encoding
    
  * 그 헤더들은 어떤 역할을 하나요?
