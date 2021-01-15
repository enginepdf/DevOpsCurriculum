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

         계층 1 : 물리 계층(Physical Layer)
            실제 장치 연결 위한 전기적, 물리적 세부 사항 정의
            핀들의  배치, 전압, 전선의 명세
            허브나 리피터가 물리 계층의 장치
            물리적 정보 전달 매개체에 대한 연결의 성립 및 종료. 여러 사용자들 간의 통신 자원을 효율적 분배하는 데 관여
            물리 계층에서 데이터 교환하는 방식은 회선 교환, 메시지 교환, 패킷 교환 방식

         계층 2 : 데이터 링크 계층(Data Link Layer)
            네트워크 위의 개체들 간 데이터 전달.
            물리 계층에서 발생할 수 있는 오류 찾고 수정하는 데 필요한 기능적, 절차적 수단 제공

         계층 3 : 네트워크 계층(Network Layer)
            여러 개의 노드 거칠 때마다 경로 찾아주는 역할
            라우팅, 흐름 제어, segmentation/desegmentation, 오류 제어, internetworking 등 수행

         계층 4 : 전송 계층(Transport Layer)
            양 끝단의 사용자들이 신뢰성 있는 데이터를 주고 받을 수 있도록 해주어, 상위 계층들이 데이터 전달의 유효성이나 효율성 생각하지 않도록 해줌
            패킷들의 전송이 유효한지 확인하고 전송 실패한 패킷들을 다시 전송한다는 뜻

         계층 5 : 세션 계층(Session Layer)
            양 끝단의 응용 프로세스가 통신을 관리하기 위한 방법 제공
            동시 송수신 방식(duplex), 반이중 방식(half-duplex), 전이중 방식(full duplex)의 통신과 함께, 체크 포인팅과 유휴, 종료, 다시 시작 과정 등을 수행. TCP/IP 세션을 만들고 없애는 책임을 짐
            통신하는 사용자들을 동기화하고 오류 복구 명령들을 일괄적으로 다룸

         계층 6 : 표현 계층(Presentation Layer)
            코드 간의 번역 담당. 사용자 시스템에서 데이터의 형식 상 차이를 다루는 부담을 응용 계층으로부터 덜어 줌
            MIME 인코딩, 암호화 등의 동작이 이루어짐

         계층 7 : 응용 계층(Application Layer)
            응용 프로세스와 직접 관계하여 일반적 응용 서비스 수행.
            일반적 응용 서비스는 관련된 응용 프로세스들의 사이의 전환 제공
            
* 우리가 브라우저의 주소 창에 `www.knowre.com` 을 쳤을 때, 어떤 과정을 통해 노리의 서버 주소를 알게 되나요?

         웹 브라우저에 'www.knowre.com' 입력 
         Local DNS에 'www.knowre.com'이라는 host name에 대한 ip 주소 물어봄
         ip 주소 알고 있다면 Local DNS에서 요청 pc에 ip 주소 리턴, 아니라면 Local DNS는 다른 DNS 서버들과 통신(DNS 메시지) 시작. 먼저 Root DNS서버에게 'www.knowre.com'의 ip 주소 질의
         안다면 해당 ip 주소를 Local DNS가 캐싱을 하고(추후 같은 요청에 대해 빠른 응답을 위함) PC에 ip 정보 전달, 모른다면 Local DNS 서버는 Root DNS 서버로부터 'knowre.com' 도메인 관리 DNS 서버 정보를 전달 받고 해당 서버에 'www.knowre.com'에 대한 ip 주소 질의
         'knowre.com' 도메인 관리 DNS 서버가 Local DNS 서버로 'www.knowre.com'에 대한 ip 주소 정보를 보내고 이를 Local DNS는 캐싱을 함(추후 같은 요청에 빠른 응답을 위해) 그리고 PC에 전달

## Quest
* tracert(Windows가 아닌 경우 traceroute) 명령을 통해 `www.google.com` 까지 가는 경로를 찾아 보세요.

         traceroute google.com
         traceroute to google.com (216.58.200.78), 64 hops max, 52 byte packets
         1  192.168.0.1 (192.168.0.1)  2.582 ms  1.756 ms  1.767 ms
         2  -(집 근처)  5.155 ms  4.315 ms  5.799 ms
         3  10.203.187.213 (10.203.187.213)  5.883 ms  6.656 ms
         4  10.204.81.117 (10.204.81.117)  4.086 ms  4.323 ms  3.587 ms
         5  210.124.146.73 (210.124.146.73)  4.367 ms
            1.213.4.221 (1.213.4.221)  7.524 ms
            1.213.4.213 (1.213.4.213)  4.178 ms
         6  1.208.144.193 (1.208.144.193)  5.702 ms
            1.213.106.125 (1.213.106.125)  5.342 ms
            1.208.106.117 (1.208.106.117)  8.365 ms
         7  1.208.165.133 (1.208.165.133)  5.105 ms
            1.208.165.77 (1.208.165.77)  4.781 ms
            1.208.165.113 (1.208.165.113)  6.386 ms
         8  kwjbba13-fe1-0-0.rt.bora.net (210.120.198.13)  4.404 ms
            1.208.104.57 (1.208.104.57)  6.681 ms
            1.208.105.253 (1.208.105.253)  6.653 ms
         9  1.208.0.74 (1.208.0.74)  40.644 ms
            61.42.0.118 (61.42.0.118)  38.917 ms
            1.208.145.38 (1.208.145.38)  56.103 ms
         10  1.208.148.206 (1.208.148.206)  41.127 ms
             1.208.106.210 (1.208.106.210)  47.267 ms
             1.208.148.206 (1.208.148.206)  40.788 ms
         11  74.125.118.154 (74.125.118.154)  40.576 ms
             72.14.215.29 (72.14.215.29)  61.346 ms  60.683 ms
         12  10.23.215.30 (10.23.215.30)  41.454 ms * *
         13  72.14.239.94 (72.14.239.94)  42.335 ms
             108.170.233.0 (108.170.233.0)  41.659 ms
             108.170.241.97 (108.170.241.97)  40.809 ms
         14  209.85.254.177 (209.85.254.177)  43.304 ms
             108.170.241.80 (108.170.241.80)  49.453 ms
             209.85.254.177 (209.85.254.177)  42.474 ms
         15  hkg07s30-in-f14.1e100.net (216.58.200.78)  47.787 ms
             172.253.64.172 (172.253.64.172)  42.802 ms

  * 어떤 IP주소들이 있나요?

             1  192.168.0.1
             2  - : 집 근처
             3  10.203.187.213
             4  10.204.81.117
             5  210.124.146.73
                1.213.4.221
                1.213.4.213
             6  1.208.144.193
                1.213.106.125
                1.208.106.117
             7  1.208.165.133
                1.208.165.77
                1.208.165.113
             8  210.120.198.13
                1.208.104.57
                1.208.105.253
             9  1.208.0.74
                61.42.0.118
                1.208.145.38
             10 1.208.148.206
                1.208.106.210
                1.208.148.206
             11 74.125.118.154
                72.14.215.29
             12 10.23.215.30
             13 72.14.239.94
                108.170.233.0
                108.170.241.97
             14 209.85.254.177
                108.170.241.80
                209.85.254.177
             15 216.58.200.78
                172.253.64.172

  * 그 IP주소들은 어디에 위치해 있나요?

             1  192.168.0.1 : 집
             2  - : 집 근처
             3  10.203.187.213 : 모바일 또는 사설아이피의 경우 위치 확인이 불가능
             4  10.204.81.117 : 모바일 또는 사설아이피의 경우 위치 확인이 불가능
             5  210.124.146.73 : 서울특별시 강남구 역삼동
                1.213.4.221 : 서울특별시 강남구 역삼동
                1.213.4.213 : 서울특별시 강남구 역삼동
             6  1.208.144.193 : 서울특별시 강남구 역삼동
                1.213.106.125 : 서울특별시 강남구 역삼동
                1.208.106.117 : 서울특별시 강남구 역삼동
             7  1.208.165.133 : 서울특별시 강남구 역삼동
                1.208.165.77 : 서울특별시 강남구 역삼동
                1.208.165.113 : 서울특별시 강남구 역삼동
             8  210.120.198.13 : 210.120.198.13
                1.208.104.57 : 서울특별시 강남구 역삼동
                1.208.105.253 : 서울특별시 강남구 역삼동
             9  1.208.0.74 : 서울특별시 강남구 역삼동
                61.42.0.118  : 서울특별시 강남구 역삼동
                1.208.145.38 : 서울특별시 강남구 역삼동
             10  1.208.148.206 : Seoul
                 1.208.106.210 : Seoul
                 1.208.148.206 : Seoul
             11  74.125.118.154 : Located in City Mountain View [ California ]
                 72.14.215.29 : Located in City Mountain View [ California ]
             12  10.23.215.30 : Seoul
             13  72.14.239.94 : Located in City Mountain View [ California ]
                 108.170.233.0 : Located in City Mountain View [ California ]
                 108.170.241.97 : Located in City Mountain View [ California ]
             14  209.85.254.177 : Located in City Mountain View [ California ]
                 108.170.241.80 : Located in City Mountain View [ California ]
                 209.85.254.177 : Located in City Mountain View [ California ]
             15  216.58.200.78 : Located in City Mountain View [ California ]
                 172.253.64.172 : Located in City Mountain View [ California ]

* Wireshark를 통해 `www.google.com` 으로 요청을 날렸을 떄 어떤 TCP 패킷이 오가는지 확인해 보세요
      
      ip.addr == 
      tcp.port == 
      udp.port == 53(DNS)
      
      ping google.com  --> 172.217.24.196: icmp_seq=113 ttl=53 time=43.241 ms 64 bytes from 172.217.24.19

      <img width="1426" alt="Screen Shot 2021-01-15 at 2 14 10 PM" src="https://user-images.githubusercontent.com/62423408/104684297-14969800-573c-11eb-8175-bf2bab3da2ba.png">

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
        Date, Expires, Cache-Control, Content-Type, P3P, Server, X-XSS-Protection, X-Frame-Options, Set-Cookie, Accept-Ranges, Vary, Transfer-Encoding

  * 그 헤더들은 어떤 역할을 하나요?

        Date : 메시지가 발생한 날짜와 시간 포함
        Expires : 응답이 만료되었다고 고려되는 날짜/시간
        Cache-Control : 요청과 응답 모두에서의 캐싱 메커니즘을 명시하는 지시문
        Content-Type : 리소스의 미디어 타입
        P3P(Platform for Privacy Preferences) : W3C에서 개발한 프라이버시 보호 관련 표준 기술, P3P를 적용한 사이트에서는 HTTP 헤더 또는 링크된 XML 파일을 통해 해당 사이트에서 취급하는 개인정보의 레벨이나 성격 등을 웹브라우저에서 알려줌
        Server : 요청을 처리하기 위해 오리진 서버에 의해 사용되는 소프트웨어에 대한 정보 포함 ex) gws : google web server
        X-XSS-Protection : 교차 사이트 스크립팅 필터링 활성화 여부
        X-Frame-Options(XFO) : 브라우저가 frame, iframe, embed 또는 object 태그에서 렌더링을 허용해야하는지를 나타냄
        Set-Cookie : 서버에서 유저 에이전트로 쿠키 전송
        Accept-Ranges : 서버가 범위 요청을 지원하는지 나타냄. 지원할 경우 범위가 표현될 수 있는 단위
        Vary : 오리진 서버로부터 새로운 요청을 하는 대신 캐시된 응답을 사용할지 결정하기 위한 향후의 요청 헤더를 매칭할 방법 정함
        Transfer-Encoding : 사용자에게 엔티티를 안전하게 전송하기 위해 사용할 인코딩 형식을 지정
