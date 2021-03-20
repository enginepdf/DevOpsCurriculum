# Quest 13. 마이크로서비스와 람다

## Introduction
* 이번 퀘스트에서는 마이크로서비스의 개념과 람다 서비스에 대해 알아보겠습니다.

## Topics
* 마이크로서비스
* Lambda
* API Gateway

## Resources
* https://news.hada.io/topic?id=638
* https://www.redhat.com/ko/topics/microservices/what-are-microservices
* https://docs.aws.amazon.com/ko_kr/lambda/latest/dg/welcome.html
* https://docs.aws.amazon.com/ko_kr/apigateway/latest/developerguide/welcome.html

## Checklist
* 마이크로서비스가 무엇인가요? 이러한 구조의 장점은 무엇일까요?

        애플리케이션 구축을 위한 아키텍처 기반의 접근 방식
        전통적 monolithic 접근 방식과 구별 짓는 기준은 애플리케이션을 핵심 기능으로 세분화하는 방식
        각 기능을 서비스라고 부르며, 독립적 구축 및 배포 가능(개별 서비스가 다른 서비스에 부정적 영향을 주지 않으면서 작동(또는 장애가 발생)할 수 있음을 의미)
          ex) 검색창, 상품 추천 내역, 온라인 장바구니
        마이크로서비스 아키텍처는 애플리케이션의 핵심 기능을 유연하게 결합하고, 불가피한 장애, 향후 확장 여부 및 새로운 기능 통합에 대비할 수 있도록 서비스 간 커뮤니케이션 및 개발팀의 구조를 조정하는 것
          -> 서비스 지향 아키텍처(Service Oriented Architecture, SOA)의 기본 사항을 채택하여 마이크로서비스를 배포하는 것


* AWS Lambda는 어떤 서비스일까요? 이러한 서비스는 어떤 특징을 가지고, 어디에 쓰일 수 있을까요?
  * AWS Lambda의 Cold start와 Warm start는 어떤 개념일까요? Lambda의 동시성이란 무엇일까요?
  * AWS Lambda의 Layer는 어떤 개념일까요?
* API Gateway는 어떤 서비스인가요? 어떤 설정을 할 수 있을까요?

## Quest
* AWS Lambda와 API Gateway를 이용해 새로운 웹서비스를 하나 구축해 보세요. (기존 컨테이너 기반의 서비스는 그대로 둡니다!)
* 만들어 둔 정적인 웹사이트에서, 기존 컨테이너 기반의 API에 더해 Lambda 기반의 API 역시 활용할 수 있도록 수정해 보세요.
* API Gateway를 통해 서비스 되고 있는 API가 우리만의 도메인을 가지고 HTTPS로 서비스될 수 있도록 하려면 어떻게 해야 할까요?