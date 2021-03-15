# Quest 12. DNS와 HTTPS

## Introduction
* 이번 퀘스트에서는 DNS에 대한 더 깊은 이해와 함께, HTTPS와 TLS에 대해 알아보겠습니다.

## Topics
* DNS

    Root > Top Level Domains > Second Level Domains > subdomains > hosts

* Route53
* HTTPS
* TLS
* Certificate Manager
* HTTP/3

## Resources
* https://www.redhat.com/sysadmin/dns-domain-name-servers
* https://aws.amazon.com/ko/route53/what-is-dns/
* https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/getting-started.html
* https://anushadasari.medium.com/the-https-protocol-explained-under-the-hood-c7bd9f9aaa7b
* https://www.internetsociety.org/deploy360/tls/basics/
* https://docs.aws.amazon.com/ko_kr/acm/latest/userguide/acm-overview.html
* https://http3-explained.haxx.se/ko

## Checklist
* DNS의 레코드에는 어떤 종류들이 있나요? 이 종류들은 어떤 용도로 쓰일까요?

    A(Host) : 주소/호스트 레코드
              정규화된 도메인 이름/호스트명(Fully Qualified Domain Name, FQDN)을 IPv4에 연결

    AAAA : 주소 레코드
           호스트를 IPv6에 연결

    CNAME(Canonical NAME) : 별칭 레코드
                            실제 호스트명(A 레코드)과 연결되는 별칭, 별명을 정의

    MX(Mail Exchange) : 메일 교환 레코드
                        메일 서버(사서함)에 도달할 수 있는 라우팅 정보(메일 서버)를 제공

    SRV(SeRVice) : 서비스 위치 레코드
                   비슷한 TCP/IP 서비스를 제공하는 다수의 서버 위치 정보를 제공

    PTR(PoinTeR) : 포인터 리소스 레코드
                   다른 DNS 레코드를 가리킴. 역방향 조회에서 A 레코드를 가리킬 때 사용

    SOA(Start Of Authority) : 권한 시작 레코드
                              DNS 영역의 주 DNS 서버를 정의하며 일련번호를 통해 영역의 변경사항을 기록
                              보조 영역의 새로고침 및 다시시도 간격 등을 정의. 영역의 기본 TTL 값 정의

    NS(Name Server) : 네임 서버 레코드
                      영역을 풀이할 수 있는 DNS 서버의 목록을 가짐

* Route53의 Alias 기능이란 무엇인가요?

    Amazon Route 53 alias records provide a Route 53–specific extension to DNS functionality. Alias records let you route traffic to selected AWS resources, such as CloudFront distributions and Amazon S3 buckets

* 대부분의 최신 브라우저에서는 HTTP 대신 HTTPS가 권장됩니다. 이유가 무엇일까요?

* HTTPS와 TLS는 어떤 식으로 동작하나요? HTTPS는 어떤 역사를 가지고 있나요?

* HTTPS의 서비스 과정에서 인증서는 어떤 역할을 할까요? 인증서는 어떤 체계로 되어 있을까요?

    클라이언트와 서버간의 통신을 공인된 제3자(CA, Certificate Authority) 업체가 보증해주는 전자화된 문서
    SSL 통신에 사용할 공개키를 클라이언트에게 제공
    인증서의 내용(서비스 정보 - 인증서 발급자, CA의 디지털 서명, 서비스 도메인, 서버측 공개키은 CA의 비공개 키를 이용해서 암호화 되어 웹 브라우저에게 제공)
                    웹 브라우저가 서버에 접속 시 서버는 제일 먼저 인증서 제공
                    브라우저는 인증서를 발급한 CA가 자신이 갖고 있는 CA 리스트에 있는지 확인
                    리스트에 있다면 해당 CA의 공개키를 이용해서 인증서 복호화(CA의 비공개키에 의해 암호화 된 것을 의미 --> 데이터 제공한 사람의 신원 보장)

* HTTP/3은 기존 버전과 어떻게 다를까요? HTTP의 버전 3이 나오게 된 이유는 무엇일까요?

    Quick UDP Internet Connection(QUIC)
    TCP와 TLS에서 동작하는 HTTP/2의 단점으로 알려진 문제를 해결하면서 안정적이고 안전한 새 전송 프로토콜

## Quest
* `xxx.knowre.com`에 해당하는 커스텀 도메인을 하나 부여해 드리겠습니다. Route53을 활용하여 이 도메인의 자체 네임서버를 구축해 보세요.
* Cloudfront에 연결된 정적인 웹사이트와 ECS에서 서비스 되는 API 서버가 위 도메인으로 서비스 되도록 설정과 연결해 보세요.
* Certificate Manager를 통해 인증서를 만들어 보세요. 그리고 위 두 서비스가 HTTPS로 서비스될 수 있도록 바꿔 보세요.
* HTTP로 접속했을 때 HTTPS로 리다이렉트 되도록 설정해 보세요.
