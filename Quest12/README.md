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

        암호화(Encryption) - 교환되는 데이터를 암호화
        데이터 무결성(Data Integrity) - 데이터 전송 중 의도적/비의도적으로 감지 및 탐지되지 않고 수정되거나 손상 될 수 없음
        인증 - 사용자가 의도한 웹 사이트와 통신하고 있음이 증명됨(중간자 공격으로부터 보호)

* HTTPS와 TLS는 어떤 식으로 동작하나요? HTTPS는 어떤 역사를 가지고 있나요?

* HTTPS의 서비스 과정에서 인증서는 어떤 역할을 할까요? 인증서는 어떤 체계로 되어 있을까요?

        클라이언트와 서버간의 통신을 공인된 제3자(CA, Certificate Authority) 업체가 보증해주는 전자화된 문서
        SSL 통신에 사용할 공개키를 클라이언트에게 제공
        인증서의 내용(서비스 정보 - 인증서 발급자, CA의 디지털 서명, 서비스 도메인, 서버측 공개키은 CA의 비공개 키를 이용해서 암호화 되어 웹 브라우저에게 제공)
            웹 브라우저가 서버에 접속 시 서버는 제일 먼저 인증서 제공
            브라우저는 인증서를 발급한 CA가 자신이 갖고 있는 CA 리스트에 있는지 확인
            리스트에 있다면 해당 CA의 공개키를 이용해서 인증서 복호화(CA의 비공개키에 의해 암호화 된 것을 의미 --> 데이터 제공한 사람의 신원 보장)

* HTTP/3은 기존 버전과 어떻게 다를까요? HTTP의 버전 3이 나오게 된 이유는 무엇일까요?

        HTTP/0.9

            요청이 단일 라인으로 구성, 가능 메서드는 GET이 유일

        HTTP/1.0

            요청에 버전 정보가 붙어서 전송되기 시작
            응답 시작 부분에 상태 코드 추가
            모든 요청과 응답에 헤더 개념 추가(확장성 증가, 다른 타입 문서 전달 가능 등)

        HTTP/1.1

            커넥션의 재사용 가능(기존 연결에 대한 handshake 생략 가능)
            파이프라이닝, 이전 요청에 대한 응답이 완전히 전송되기 전에 다음 전송을 가능케 함. 레이턴시 낮춤
            언어, 인코딩 타입 등을 포함한 컨텐츠 전송
            동일 IP 주소에 다른 도메인을 호스트 하는 기능 가능(HOST header)
            연결당 하나의 요청과 응답 처리(동시 전송 문제, 다수 리소스 처리하기에 속도, 성능 이슈 존재)
            HOLB(Head Of Line Blocking, 특정 응답 지연)
            RTT(Round Trip Time) 증가(양방향 지연)
            헤드가 크다(특히 쿠키)

        HTTP/2.0

            SDPY 기반 
            클라이언트와 서버가 HTTP 1.1, 2.0 혹은 다른 비 HTTP 프로토콜 사용을 협상할 수 있는 메커니즘 구현
            HTTP 1.1과 호환성 유지
            HTTP 헤더 데이터 압축 : 지연 시간 감소(HPACK 압축 방식)
            서버 푸시 기술 : 지연 시간 감소(HTML 문서 상에 필요한 리소스를 클라이언트 요청 없이 보낼 수 있음)
            요청을 HTTP 파이프라인으로 처리 : 지연 시간 감소
            HTTP 1.x의 HOL Blocking 문제 해결 : 지연 시간 감소
            TCP 연결 하나로 여러 요청을 다중화 하여 처리(Multiplexed Streams) : 지연 시간 감소  --> HOL Blocking 발생하지 않음 --> HTTP 1.1에 비해 속도 향상

        HTTP/3.0

            QUIC(Quick UDP Internet Connection) 사용

            암호화가 프로토콜의 일부 기능으로 포함
            스트림 연결과 암호화 스펙 등을 포함한 모든 handshake가 단일 요청/응답으로 끝남
            패킷의 개별적 암호화, 다른 데이터 부분의 패킷을 기다릴 필요 없음
            통신이 멀티플렉싱 되어 HOLB 극복
            QUIC는 운영체제 커널과 독립적으로 응용 프로그램 공간 내에서 구현, 데이터 이동에 따른 컨텍스트 전환에 의한 오버헤드가 없어짐
            Source Address와 무관하게 서버에 대한 연결을 고유하게 식별하는 연결 식별자가 포함. IP주소가 변결되더라도 커넥션 유지 가능
            새로운 연결에 대해 handshake로 인한 지연, 패킷 손실이 다른 스트림에 주는 영향, 패킷 손실로 인한 지연으로부터 자유로워짐

## Quest
* `xxx.knowre.com`에 해당하는 커스텀 도메인을 하나 부여해 드리겠습니다. Route53을 활용하여 이 도메인의 자체 네임서버를 구축해 보세요.(O)

* Cloudfront에 연결된 정적인 웹사이트와 ECS에서 서비스 되는 API 서버가 위 도메인으로 서비스 되도록 설정과 연결해 보세요.(O)

        Cloudfront - Alias를 이용해 연결 처리
        API 서버 - Alias를 이용해 ALB 연결 처리

* Certificate Manager를 통해 인증서를 만들어 보세요. 그리고 위 두 서비스가 HTTPS로 서비스될 수 있도록 바꿔 보세요.(O)

        Cloudfront - HTTPS 처리
        API 서버 - HTTPS 처리

* HTTP로 접속했을 때 HTTPS로 리다이렉트 되도록 설정해 보세요.(O)

        Cloudfront - HTTPS로 리다리렉트 처리
        API 서버 - HTTPS로 리다이렉트 처리
