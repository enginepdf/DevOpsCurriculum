# Quest 07. 여러 대의 서버로 서비스 하기

## Introduction
* 이번 퀘스트는 여러 대의 서버를 통해 안정적인 서비스를 만드는 방법을 다룹니다.

## Topics

## Resources
* https://aws.amazon.com/ko/elasticloadbalancing
* `로드밸런서`와 `리스너`, 그리고 `대상그룹`와 `상태 검사`

        로드밸런서 : 부하 분산을 위해 가상 IP를 통해 여러 서버에 접속하도록 분해하는 기능
            하드웨어일 수도 소프트웨어일 수도 있음.
            Round Robin, Least Connection, Weighted Least Connection, Fastest Response Time, Adaptive, Hash(특정 클라이언트는 특정 서버로만 할당. 경로 보장. 접속자 수 많을수록 분산 및 효율 뛰어남. 접속자 수 적을수록 비효율적 분산)
            -> 보통 Least Connection 후 동등한 수의 Connection일 때 Round Robin

        Target group(대상 그룹) : EC2 인스턴스를 오토 스케일링 할 수 있는 단위. 정의된 상태 검사 수행
                                하나의 ELB(Elastic Load Balancer 내부적으로 트래픽 처리 성능 달라짐)에 여러 대상 그룹 연결 가능. 요청 포트에 따라 다른 대상 그룹으로 요청 전달 가능
                                
        상태 검사(Health check) : 대상 그룹에 원하는 경로와 포트를 설정하여 정상적으로 원하는 HTTP 응답이 오는지 확인. 정상적으로 응답이 오지 않는 인스턴스가 있다면 비정상 상태의 인스턴스를 제외한 다른 인스턴스로만 트래픽 분산
            Interval : Amount of time between health checks(5~300 seconds)
            Response Timeout : Time to wait when receiving a response from the health check(2~60 seconds)
            Healthy/Unhealthy threshold : Number of consecutive health check successes/failures before declaring an EC2 instance healthy/unhealthy

        리스너 : 구성한 프로토콜 및 포트를 사용하여 연결 요청을 확인하는 프로세스

## Checklist
* 여러 대의 서버로 서비스할 때의 장점은 무엇일까요? 또 주의해야 할 점은 무엇일까요?

        (1)
        성능 향상(트래픽을 여러 대의 서버가 나누어 개별 서버의 부하량을 낮춤) 및 무중단 서비스 구축(안정성, 가용성) 가능, Scale up 보다는 저렴(scale up할 때는 서버 중단해야 함)

        (2)
        데이터 불일치가 발생하지 않도록 조심(세션 처리 등)
        적정 서버 개수를 잘 판단하여 소프트웨어 라이선스 비용이 지나치게 발생하지 않도록 조심
        서버가 늘어날 때마다 도메인 새로 필요

* AWS에서 제공하는 로드밸런서에는 어떤 종류가 있나요? 각각의 용도는 무엇일까요?

        - Application Load Balancer  --> 7(HTTP) 계층 - ALB, HTTP 패킷 확인 -> 데이터가 더 캡슐화되어 네트워크 로드밸런서보다 더 무거움

            HTTP, HTTPS 트래픽의 로드밸런싱에 가장 적합. 마이크로서비스와 컨테이너 등 최신 애플리케이션 아키텍처 전달을 위한 고급 요청 라우팅 기능을 제공. 요청 내용에 따라 트래픽을 Amazon VPC 내의 대상으로 라우팅
            포트, 헤더 수정 가능
            하나의 대상 그룹에 더 많은 컨테이너를 넣어 비용 최적화 가능
            대상을 EC2 인스턴스, 람다, IP로도 연결 가능. 특정 요청에 대해 서버 없이 직접 응답 메시지 작성 가능하여 마이크로 아키텍쳐 구성하기 좋음
            동적 호스트 포트 매핑을 지원하여 동일한 컨테이너 인스턴스에서 단일 서비스의 다중 작업 가능
            경로 기반 라우팅 지원(URL 콘텐츠를 기반으로 요청을 올바른 서비스로 라우팅 가능)
            호스트 기반 라우팅 지원(HTTP 헤더의 호스트 필드를 기반으로 요청 전달)
            표준 또는 사용자 지정 HTTP 헤더 및 메서드, 쿼리 파라미터, 소스 IP 주소 같은 요청의 필드 기반으로 하는 라우팅 지원
            단일 EC2 인스턴스의 여러 애플리케이션으로 요청을 라우팅하는 것 지원(여러 포트 사용하여 각 인스턴스 또는 IP주소를 동일한 대상 그룹에 등록 가능)
            한 URL에서 다른 URL로 요청 리디렉션하는 작업 지원
            사용자 지정 HTTP 응답 회신 지원

        - Network Load Balancer -> 5,6,7    L2(허브), 3계층(IP, L3 스위치 : 공유기 - 스위치, 4(TCP) - 네트워크 로드밸런서

            극도의 성능이 요구되는 TCP, TLS 트래픽의 로드 밸런싱에 가장 적합. 트래픽을 Amazon VPC 내의 대상으로 라우팅. 매우 짧은 지연 시간을 유지하면서 초당 수백만 개의 요청 처리 가능
            동적 호스트 포트 매핑을 지원하여 동일한 컨테이너 인스턴스에서 단일 서비스의 다중 작업 가능
            로드 밸런서에 고정 IP 주소 지원
            리스너 구성에 지정된 포트에서 선택한 대상에 대한 TCP 연결 열려고 시도
            

        - Gateway Load Balancer - 온프레미스 서비스 위함

            타사 가상 네트워킹 어플라이언스의 배포, 확장 및 실행을 쉽게 해줌. 트래픽의 소스와 대상에 대해 투명하게 작동. 보안, 네트워크 분석 및 기타 사용 사례에 타사 어플라이언스와 함께 사용하기에 적합

        - Classic Load Balancer

            EC2 인스턴스에서 기본적 로드 밸런싱. 요청 수준 및 연결 수준에서 작동
            애플리케이션에 대한 요청의 전체적 흐름을 방해하지 않고 필요에 따라 로드 밸런서에서 인스턴스를 추가 및 제거 가능
            전송 계층에서 작동하여 데이터 수정, 변경할 수 없어 포트나 헤더 변경 불가
            전송 계층(TCP/SSL) 또는 애플리케이션 계층(HTTP/HTTPS)에서 라우팅 결정
            애플리케이션 생성 쿠키를 사용하여 고정 세션 지원
            로드 밸런서 포트와 컨테이너 인스턴스 포트의 고정된 관계 필요

* AWS 로드밸런서의 리스너 규칙을 이용해 어떤 일들을 할 수 있을까요?

        리스너에 대해 정의한 규칙에 따라 로드 밸런서가 하나 이상의 대상 그룹에서 대상으로 요청을 라우팅하는 방법 결정

        authenticate-cognito
            [HTTPS 리스너] Amazon Cognito를 사용하여 사용자를 인증

        authenticate-oidc
            [HTTPS 리스너] OpenID Connect(OIDC)와 호환되는 자격 증명 공급자를 사용하여 사용자를 인증

        fixed-response
            사용자 지정 HTTP 응답을 반환

        forward
            요청을 지정된 대상 그룹으로 전달

        redirect
            한 URL의 요청을 다른 URL로 리디렉션

* AWS의 여러 리전(서울, 도쿄 등)으로 로드밸런싱을 하는 것도 가능할까요?

        리전별 로드밸런서들을 관리하는 로드밸런서 사용(총 두 단계의 로드밸런서 사용)

        AWS Global Accelerator를 사용하여 하나 이상의 AWS 리전에 있는 여러 로드 밸런서에 트래픽 분산(애플리케이션 상태, 네트워크 상태 및 사용자의 지리적 위치를 기반으로 트래픽을 전달하고 정적 애니 캐스트 IP 주소 세트 제공)

        Device -> AWS Global Accelerator(AWS edge, Global static anycast two IPs) -> (Amazon global network up to 10 AWS regions) each Region(Network load balancer) -> Amazon EC2


## Quest
* EC2 인스턴스를 한 대 더 늘리고, 앞단에 ELB를 사용하여 두 대를 로드밸런싱 해 보세요. 직전 퀘스트에서 만들었던 컨테이너를 이용하시면 됩니다.(O)

        Classic Load Balancer 사용 -> Application Load Balancer로 수정
        sudo service nginx start
        sudo service docker start
        docker-compose up -d
        docker-compose ps

* 두 대의 서버 모두에 번갈아 요청이 들어오는지 확인해 보세요.(O)

        /check   // this is /process/check
        /check   // this is Amazon Linux2 /process/check

* 만약 한 대의 서버의 컨테이너를 내렸을 때, 자동으로 나머지 한 대의 서버로만 요청이 가게 되도록 설정해 보세요.(O)

        Health Check가 한 대의 서버 컨테이너를 바로 내리고 업데이트가 안 되었을 때(아직 in service) 로드 밸런싱 되어 에러가 뜨다가 Check 후(해당 서버 out of service) Service 대수 1대로 내려가면 나머지 작동 중인 한 대의 서버로만 요청이 감(Health Check Interval를 줄이니까 빨라지긴 함)
        ELB는 서버가 요청을 받을 수 있을 때만 요청 전달. 상태 검사를 이용 HTTP 경로 /check, response(200)
