# Quest 12. DNS와 HTTPS

## Introduction
* 이번 퀘스트에서는 DNS에 대한 더 깊은 이해와 함께, HTTPS와 TLS에 대해 알아보겠습니다.

## Topics
* DNS

        Root > Top Level Domains > Second Level Domains > subdomains > hosts

        DNS가 트래픽을 웹 애플리케이션에 라우팅하는 방법

                사용자가 웹 브라우저를 열어 주소 표시줄에 www.example.com을 입력하고 enter 키 누름
                DNS 해석기(DNS resolver)로 라우팅. 일반적으로 케이블 인터넷 공급업체, DSL(Digital Subscriber Line) 광대역 공급업체 또는 기업 네트워크 같은 인터넷 서비스 제공업체(ISP)가 관리
                ISP의 DNS 해석기는 www.example.com에 대한 요청을 DNS 루트 이름 서버에 전달
                ISP의 DNS 해석기는 www.example.com에 대한 요청을 이번에는 .com 도메인의 TLD 이름 서버 중 하나에 다시 전달. 
                        .com 도메인의 이름 서버는 example.com 도메인과 연관된 4개의 Amazon Route 53 이름 서버의 이름을 사용하여 요청에 응답
                ISP의 DNS 해석기는 Amazon Route 53 이름 서버 하나를 선택해 www.example.com에 대한 요청을 해당 이름 서버에 전달
                Amazon Route 53 이름 서버는 example.com 호스팅 영역에서 www.example.com 레코드를 찾아 웹 서버의 IP 주소 등 연관된 값을 받고 이 IP 주소를 DNS 해석기로 반환
                ISP의 DNS 해석기가 사용자에게 필요한 IP 주소를 확보. 해석기는 이 값을 웹 브라우저로 반환
                        DNS 해석기는 다음에 누군가 example.com을 탐색할 때 좀 더 빠르게 응답할 수 있도록 사용자가 지정하는 일정 기간 동안 example.com의 IP 주소를 캐싱(저장)
                웹 브라우저는 DNS 해석기로부터 얻은 IP 주소로 www.example.com에 대한 요청을 전송
                해당 IP 주소에 있는 웹 서버 또는 그 밖의 리소스는 www.example.com의 웹 페이지를 웹 브라우저로 반환하고, 웹 브라우저는 이 페이지를 표시

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

        A(Host) : 주소/호스트 레코드. 도메인의 IP 주소 갖고 있음
                  정규화된 도메인 이름/호스트명(Fully Qualified Domain Name, FQDN)을 IPv4에 연결

        AAAA : 주소 레코드
               호스트를 IPv6에 연결

        CNAME(Canonical NAME) : 별칭 레코드
                                실제 호스트명(A 레코드)과 연결되는 별칭, 별명을 정의. IP 주소 제공하지 않음

        MX(Mail Exchange) : 메일 교환 레코드
                            메일 서버(사서함)에 도달할 수 있는 라우팅 정보(메일 서버)를 제공

        SRV(SeRVice) : 서비스 위치 레코드. 특정 서비스에 대한 포트 지정
                       비슷한 TCP/IP 서비스를 제공하는 다수의 서버 위치 정보를 제공

        PTR(PoinTeR) : 포인터 리소스 레코드
                       다른 DNS 레코드를 가리킴. 역방향 조회에서 A 레코드를 가리킬 때 사용(리버스 조회에서 도메인 이름 제공)

        SOA(Start Of Authority) : 권한 시작 레코드. 도메인에 대한 관리자 정보 저장
                                  DNS 영역의 주 DNS 서버를 정의하며 일련번호를 통해 영역의 변경사항을 기록
                                  보조 영역의 새로고침 및 다시시도 간격 등을 정의. 영역의 기본 TTL 값 정의

        NS(Name Server) : 네임 서버 레코드
                          영역을 풀이할 수 있는 DNS 서버의 목록을 가짐

        CERT : 인증서 레코드. 공개 키 인증서 저장

        CAA : 인증 기관 인증 레코드. 이 레코드에서 도메인 소유자는 해당 도메인에 대해 인증서를 발급할 수 있는 인증 기관을 명시 가능
              CAA 레코드가 없으면, 누구나 도메인에 대한 인증서 발급 가능. 하위 도메인에도 상속

        DNAME : 위임 레코드. CNAME과 마찬가지로 도메인 별칭을 생성하지만 이 별칭은 모든 하위 도메인도 리디렉션
                'example.com'의 소유자가 'website.net' 도메인을 구매하고, 여기에 'example.com'을 가리키는 DNAME 레코드 부여 시, 해당 포인터는
                'blog.website.net' 및 다른 하위 도메인으로도 확장 가능

* Route53의 Alias 기능이란 무엇인가요?

        Amazon Route 53 alias records provide a Route 53–specific extension to DNS functionality. 
        Alias records let you route traffic to selected AWS resources, such as CloudFront distributions and Amazon S3 buckets

* 대부분의 최신 브라우저에서는 HTTP 대신 HTTPS가 권장됩니다. 이유가 무엇일까요?

        암호화(Encryption) - 교환되는 데이터를 암호화
        데이터 무결성(Data Integrity) - 데이터 전송 중 의도적/비의도적으로 감지 및 탐지되지 않고 수정되거나 손상 될 수 없음
        인증(Authentification) - 사용자가 의도한 웹 사이트와 통신하고 있음이 증명됨(중간자 공격으로부터 보호)

* HTTPS와 TLS는 어떤 식으로 동작하나요? HTTPS는 어떤 역사를 가지고 있나요?

        HTTPS(Hyper Text Protocol Secure) :  HTTPS는 웹사이트를 SSL/TLS 인증서로 보안하는 경우 URL 창에 표시, SSL을 조합한 HTTP를 HTTPS(HTTP Secure) or HTTP over SSL이라고 부름.
                                             HTTP 통신하는 소켓 부분을 SSL, TLS라는 프로토콜로 대체
                                             HTTP는 원래 TCP와 직접 통신, HTTPS에서 HTTP는 SSL과 통신하고 SSL이 TCP와 통신.
                                             HTTPS의 SSL에서는 공통키(대칭키) 암호화 방식과 공개키 암호화 방식을 혼합한 하이브리드 암호 시스템 사용.
                                             공통키를 공개키 암호화 방식으로 교환한 다음부터의 통신은 공통키 암호를 사용하는 방식
                                             통신할 때마다 암호화를 하면 많은 리소스를 사용하여 서버 한 대당 처리할 수 있는 리퀘스트의 수가 줄어듬

        TLS(Transport Layer Security) : TLS는 가장 최신 기술로 더 강력한 버전의 SSL(세션 키를 만들 때 유지하고자 할 때 공개키 암호화 방식 사용)
                                        TLS is a cryptographic protocol that provides end-to-end security of data sent between applications over the Internet. 
                                        It is mostly familiar to users through its use in secure web browsing, and in particular the padlock icon that appears in web browsers when a secure session is established. 
                                        However, it can and indeed should also be used for other applications such as e-mail, file transfers, video/audioconferencing, instant messaging and voice-over-IP, as well as Internet services such as DNS and NTP

                                        TLS is normally implemented on top of TCP in order to encrypt Application Layer protocols such as HTTP, FTP, SMTP and IMAP, 
                                        although it can also be implemented on UDP, DCCP and SCTP as well (e.g. for VPN and SIP-based application uses). 
                                        This is known as Datagram Transport Layer Security (DTLS) and is specified in RFCs 6347, 5238 and 6083

                                        TLS uses a combination of symmetric and asymmetric cryptography, as this provides a good compromise between performance and security when transmitting data securely.

                                        With symmetric cryptography, data is encrypted and decrypted with a secret key known to both sender and recipient; 
                                        typically 128 but preferably 256 bits in length (anything less than 80 bits is now considered insecure). 
                                        Symmetric cryptography is efficient in terms of computation, but having a common secret key means it needs to be shared in a secure manner.

                                        Asymmetric cryptography uses key pairs – a public key, and a private key. The public key is mathematically related to the private key, but given sufficient key length, it is computationally impractical to derive the private key from the public key. 
                                        This allows the public key of the recipient to be used by the sender to encrypt the data they wish to send to them, but that data can only be decrypted with the private key of the recipient.

                                        The advantage of asymmetric cryptography is that the process of sharing encryption keys does not have to be secure, 
                                        but the mathematical relationship between public and private keys means that much larger key sizes are required. 
                                        The recommended minimum key length is 1024 bits, with 2048 bits preferred, but this is up to a thousand times more computationally intensive than symmetric keys of equivalent strength 
                                        (e.g. a 2048-bit asymmetric key is approximately equivalent to a 112-bit symmetric key) 
                                        and makes asymmetric encryption too slow for many purposes.

                                        For this reason, TLS uses asymmetric cryptography for securely generating and exchanging a session key. 
                                        The session key is then used for encrypting the data transmitted by one party, and for decrypting the data received at the other end. Once the session is over, the session key is discarded.

* HTTPS의 서비스 과정에서 인증서는 어떤 역할을 할까요? 인증서는 어떤 체계로 되어 있을까요?

        클라이언트와 서버간의 통신을 공인된 제3자(CA, Certificate Authority) 업체가 보증해주는 전자화된 문서
        SSL 통신에 사용할 공개키를 클라이언트에게 제공
        인증서의 내용(서비스 정보 - 인증서 발급자, CA의 디지털 서명, 서비스 도메인, 서버측 공개키는 CA의 비공개 키를 이용해서 암호화 되어 웹 브라우저에게 제공)
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
            헤더가 크다(특히 쿠키)

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

        Cloudfront - HTTPS로 리다이렉트 처리
        API 서버 - HTTPS로 리다이렉트 처리
