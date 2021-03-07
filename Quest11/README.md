# Quest 11. 보안의 기초

## Introduction
* 이번 퀘스트에서는 네트워크와 보안에 관한 아주 기초적인 것을 다룹니다.

## Topics
* VPC, 서브넷, 방화벽
* IAM
* Secrets Manager

## Resources
* https://www.44bits.io/ko/post/understanding_aws_vpc
* https://docs.aws.amazon.com/ko_kr/IAM/latest/UserGuide/introduction.html
* https://aws.amazon.com/ko/secrets-manager/
* https://blog.logrocket.com/web-security-101/

## Checklist
* AWS의 VPC는 어떤 서비스일까요?

        Amazon Virtual Private Cloud(VPC)를 사용하면 AWS 클라우드에서 논리적으로 격리된 공간을 프로비저닝하여 고객이 정의하는 가상 네트워크에서 AWS 리소스 시작 가능
        IP 주소 범위 선택, 서브넷 생성, 라우팅 테이블 및 네트워크 게이트웨이 구성 등 가상 네트워킹 환경을 완벽하게 제어 가능
        하나의 계정에서 생성하는 리소스들만의 격리된 네트워크를 만들어주는 기능(AWS에서 제공되는 대부분의 서비스가 VPC에 의존적, 아마존 웹서비스에서 계정 생성 시 함께 생성되는 기본 VPC(Default VPC))

* 인프라의 네트워크를 가상으로 구축하는 것은 어떤 장점이 있을까요?

        하드웨어 구매 및 유지 관리 비용 줄일 수 있음
        네트워크 가상화는 효율적으로 리소스 확장 가능
        보안 및 복구 시간 향상(트래픽 유형 제어, 네트워크 가상화 소프트웨어는 하드웨어 오류로 인한 중단을 줄이거나 제거, IP 주소 변경 및 방화벽 업데이트 등과 같은 수동적이고 시간 집약적인 단계를 제거)
        빠른 애플리케이션 제공(네트워크 구성을 자동화, 배포 시간 단축 -> 더 빠른 신제품 롤아웃 또는 주요 응용 프로그램 업데이트 가능)
        기반 하드웨어에서 네트워크 서비스를 분리하고, 네트워크 전체의 가상 프로비저닝 지원, 중앙 집중식 관리 시스템을 통해 스위치, 라우터 같은 물리적 네트워크 리소스를 풀링하고 이에 액세스 가능
        여러 관리 작업을 자동화, 수작업에 따른 오류를 줄이고 프로비저닝 시간을 단축하여 네트워크 생산성과 효율성 증가

* IAM은 어떤 서비스일까요? 어떤 식으로 계정에 권한을 부여할까요?

        AWS Identity and Access Management(IAM)는 AWS 리소스에 대한 액세스를 안전하게 제어할 수 있는 웹 서비스
        IAM을 사용하여 리소스를 사용하도록 인증(로그인) 및 권한 부여(권한 있음)된 대상을 제어
        AWS 계정을 처음 생성할 때는 해당 계정의 모든 AWS 서비스 및 리소스에 대한 완전한 액세스 권한이 있는 SSO(Single Sign-In) ID로 시작(AWS 계정 루트 사용자)
        IAM > 사용자 추가 > 권한 설정 > 그룹 생성 > 부여하고 싶은 권한과 관련된 정책 추가 > 그룹에 다른 사용자 추가(필요 시)
        EC2의 경우 인스턴스 생성 시 IAM role 선택하여 사용하거나 생성 후 정책 추가 가능

* DB 패스워드와 같이 민감한 정보를 어떤 식으로 관리하는 것이 좋을까요?

        패스워드를 데이터베이스 처리할 때 해싱 처리를 함(사용되는 salt가 계속 바뀌도록 함)
        정기적으로 DB 패스워드 교체를 사용자에게 요청 함
        해당 사용자만 로그인 할 수 있는 링크를 그 사용자에게 보내서 로그인 시킴(비밀번호 불필요)
        인증, 권한 부여를 위해 OAuth 2.0와 같은 third-party 사용
        third-party 제공자(트위터, 페이스북, 구글 등)를 이용해서 로그인 할 수 있는 기능을 만듬
        환경 변수 처리(혹은 플랫폼에서 제공하는 환경 변수 처리 방식 사용)
        property만 관리하는 파일 생성
        AWS Secrets Manager, 1Password 같은 서비스를 사용    

* Secrets Manager는 어떤 서비스일까요? AWS 외에 비슷한 역할을 하는 서비스로는 어떤 것이 있을까요?

        애플리케이션, 서비스, IT 리소스에 액세스할 때 필요한 보안 정보를 보호하는 데 도움을 줌
        사용자 및 애플리케이션에서 Secrets Manager API를 호출하여 보안 정보를 검색하여 민감한 정보를 평문으로 하드코딩 할 필요 없음
        정해진 일수마다 비밀번호 로테이션 기능
        Amazon RDS와 통합하여 사용 가능
        1Password, LastPass

* 어플리케이션 레벨에서의 보안 전략에는 어떤 것들이 있을까요?

        Cross-site request forgery

            The solution is to generate a token and put this token in a new cookie and in a hidden field in the form. 
            Then when the form is submitted the backend will check if the token from the cookie is equal to the token in the form. 
            An attacker doesn’t see the cookies so it’s unable to craft a form with a valid CSRF token.

        SQL injection

            The main mechanism to void this problem is by escaping the input parameters. Any good SQL library should have a way to achieve this

            query = "SELECT * FROM users WHERE login = '" + input_login + "';"

            const text = 'INSERT INTO users(name, email) VALUES($1, $2) RETURNING *'
            const values = ['brianc', 'brian.m.carlson@gmail.com']
            const response = await client.query(text, values)

            Instead of interpolating the values manually, you use placeholders ($1, $2), that the library will replace by the escaped version of the array of values.

        Cross-site scripting (XSS)

            If you want to allow users to use some HTML in their content, but you want to avoid XSS attacks then you should clean up the HTML first allowing just some tags and attributes to be used. For JavaScript, you have this sanitizer.

        Be careful with external links

            The solution is as easy as using these values for the rel attribute:

            <a href=”...” target=”_blank” rel=”noopener noreferrer”>Malicious link</a>

        Analyze your website

            Nowadays there are also tools that allow you to catch problems, including security problems, easily. One of them is webhint. It has some rules that catch problems such as poor HTTP headers, vulnerable external links, etc.
            There are also more advanced tools such as OWASP ZAP if you are interested in digging deeper into these topics.

## Quest
* VPC를 구축하고, 적절한 서브넷 설정, 보안 그룹과 방화벽을 설정해 보세요. 각각의 단계와 구성요소들이 어떤 의미인지 이해해야 합니다.

        Network ACL(NACL)
        Security Group
        Subnet

* 지금까지 구현한 웹 어플리케이션을 VPC 안에서 서비스 해 보세요.
