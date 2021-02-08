# Quest 09. 정적인 컨텐츠 서비스 하기

## Introduction
* 이번 퀘스트를 통해 정적인 컨텐츠의 서비스와 CDN의 개념, 그리고 HTTP 캐싱에 대해 알아보겠습니다.

## Topics
* S3
* Cloudfront
* HTTP Cache
* CORS
* Invalidation(Purging)

        https://www.varnish-software.com/glossary/what-is-cache-invalidation/
        https://www.imperva.com/blog/purge-cache-keeps-content-fresh-responsive/
        
        Cache invalidation refers to process during which web cache proxies declare cached content as invalid, 
        meaning it will not longer be served as the most current piece of content when it is requested.
        Several invalidation methods are possible, including purging, refreshing and banning.
        Purging refers to the active removal of a resource from the cache without waiting for 
        the predetermined cache expiry time

## Resources
* https://aws.amazon.com/ko/s3/
* https://aws.amazon.com/ko/cloudfront/
* https://developer.mozilla.org/ko/docs/Web/HTTP/Caching
* https://developer.mozilla.org/ko/docs/Web/HTTP/CORS


## Checklist
* AWS의 S3는 어떤 서비스일까요?

        Amazon Simple Storage Service(Amazon S3)는 확장성, 데이터 가용성 및 보안과 성능을 제공하는 객체 스토리지 서비스. 
        구모 업종에 상관 없이 고객이 이 서비스를 이용하여 데이터 레이크, 웹사이트, 모바일 애플리케이션, 배업 및 복원, 아카이브, 엔터프라이즈 애플리케이션, 
        IoT 디바이스, 빅데이터 분석과 같은 다양한 사용 사례에서 원하는 만큼의 데이터를 저장하고 보호할 수 있음
        여러 시스템에 걸쳐 모든 S3 객체의 복사본을 자동으로 생성하고 저장하여 99.999999999%의 데이터 내구성 보장
        AWS Lambda와 연동되어 추가 인프라를 관리하지 않고도 활동을 기록하여 알림 정의하며 워크플로를 자동화 가능

* AWS의 Cloudfront는 어떤 서비스일까요?

        개발자 친화적 환경에서 짧은 지연 시간과 빠른 전송 속도로 데이터, 동영상, 애플리케이션 및 API를 전 세계 고객에게 안전하게 전송하는 고속 콘텐츠 전송 네트워크(CDN, Content Delivery Network) 서비스.
        필드 수준 암호화 및 HTTPS 지원을 포함한 대부분의 고급 보안 기능 제공
        엣지 네트워킹 위치에 함께 상주하며, AWS 네트워크 백본을 통해 전 세계적으로 확장 및 연결

        * 필드 수준 암호화 : 필드 레벨 암호화는 HTTPS와 함께 추가 보안 레이어를 추가하여 시스템 처리 전체에서 특정 데이터를 보호하고 특정 애플리케이션만 이를 볼 수 있도록 함

        * 엣지 컴퓨팅 : 사용자 또는 데이터 소스의 물리적인 위치나 그 위치와 가까운 곳에서 컴퓨팅을 수행하는 것
        
        * 백본 : 기간망, 여러 소형 네트워크들을 묶어 대규모 파이프라인을 통해 극도로 높은 대역폭으로 다른 네트워크들의 집합과 연결되는 네트워크

* HTTP의 캐싱은 어떤 식으로 이루어질까요?

        주어진 리소스의 복사본을 저장하고 있다가 요청 시에 그것을 제공하는 기술
        웹 케시가 자신의 저장소 내에 요청된 리소스를 가지고 있다면, 요청을 가로채 원래의 서버로부터 리소스를 다시 다운로드 하는 대신 리소스의 복사본 반환
        모든 리소스가 영언히 변하지 않는 것은 아니므로 리소스가 변하기 전까지만 캐싱하고 변한 이후에는 더이상 캐싱하지 않는 것이 중요

                - 공유 캐시 : 한 명 이상의 사용자가 재사용할 수 있도록 응답을 저장하는 캐시
                - 사설 캐시 : 한 명의 사용자만 사용하는 캐시


* CORS는 무엇인가요? 어떤 헤더를 통해 구현되나요? EC2 서버나 S3에 구현하려면 어떻게 해야 할까요?

        CORS(Cross-Origin Resource Sharing, 교차 출처 리소스 공유)은 추가 HTTP 헤더를 사용하여, 
        한 출처에서 실행 중인 웹 애플리케이션이 다른 출처의 선택한 자원에 접근할 수 있는 권한을 부여하도록 브라우저에 알려주는 체제
        웹 애플리케이션은 리소스가 자신의 출처(도메인, 프로토콜, 포트)와 다를 때 교차 출처 HTTP 요청을 실행
        다른 출처의 리소스를 불러오려면 그 출처에서 올바른 CORS 헤더를 포함한 응답 반환해야 함
        CORS 체제는 브라우저와 서버 간의 안전한 교차 출처 요청 및 데이터 전송 지원

        - HTTP 응답 헤더

                Access-Control-Allow-Origin: <origin> | *
                Access-Control-Expose-Headers: <header-name>[, <header-name>]*
                Access-Control-Max-Age: <delta-seconds>
                Access-Control-Allow-Credentials: true
                Access-Control-Allow-Methods: <method>[, <method>]*
                Access-Control-Allow-Headers: <header-name>[, <header-name>]*

        - HTTP 요청 헤더

                Origin: <origin>
                Access-Control-Request-Method: <method>
                Access-Control-Request-Headers: <field-name>[, <field-name>]*

## Quest
* 만들어 둔 서버 API로부터 특정 정보를 받아 웹 서비스로 뿌려주는 클라이언트를 개발해 봅시다.(O)

                https://github.com/enginepdf/Quest/tree/main/ClientQ
                // 서버로 fetch 할 링크 수정 필요

* 이를 S3에 배포하고, Cloudfront를 통해 서비스하는 인프라를 구성해 봅시다.

                www --> Edge Locations(Local Cache) of Cloudfront with OAI(Origin Access Identity) --> S3(clientq) as an Origin

* 클라이언트를 수정 배포했을 때, 수정사항이 Cloudfront를 통해 최대한 빨리 반영되게 하려면 어떻게 해야 할까요?

                TTL(Time To Live) 값을 작게 설정하여 Edge Locations의 Local Cache 교체가 빨리 이루어지게 한다.
