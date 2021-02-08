# Quest 10. 자동화된 테스트

## Introduction
* 이번 퀘스트에서는 자동화된 테스트에 어떤 장점이 있는지, 어떤 식으로 구축할 수 있는지에 대해 알아보겠습니다.

## Topics
* 유닛 테스트

    Unit testing — With unit tests we are testing small isolated pieces of our code.

* Mock과 Stub

* 통합 테스트

    Integration testing — In this type of testing we combine and test individual units and test them as a group.

* E2E 테스트

    End-to-end testing is a technique that tests the entire software product from beginning to end to ensure the application flow behaves as expected
    The main purpose of End-to-end (E2E) testing is to test from the end user’s experience by simulating the real user scenario and validating the system under test and its components for integration and data integrity.
        프론트엔드 - 원하는 text가 나오는지, 버튼 클릭 시 기대 동작을 하는지 등을 테스트
        벡엔드 - API, Function 등에 mock 처리하여 기대하는 값을 return하는지 확인

* Jest, Puppeteer

    Jest: Is fully featured testing framework, which is developed by Facebook. It needs very little configuration and works basically out of the box.
    Puppeteer: A Node.js library created by Google, which provides a convenient API to control Headless Chrome.

## Resources
* https://www.popit.kr/unit-test-%EB%8B%A8%EC%9C%84-%ED%85%8C%EC%8A%A4%ED%8A%B8-%EB%8F%84%EC%9E%85%ED%95%98%EA%B8%B0-1%ED%8E%B8/
* https://medium.com/touch4it/end-to-end-testing-with-puppeteer-and-jest-ec8198145321
* https://stackoverflow.com/questions/3459287/whats-the-difference-between-a-mock-stub
* https://velog.io/@leejh3224/%EC%86%8C%ED%94%84%ED%8A%B8%EC%9B%A8%EC%96%B4-%ED%85%8C%EC%8A%A4%ED%8A%B8-%EC%95%88%ED%8B%B0-%ED%8C%A8%ED%84%B4

## Checklist
* 자동화된 테스트를 작성하는 것은 왜 중요할까요?

        테스트 자동화는 방대한 테스트 항목을 처리할 수 있으며, 테스트 시간을 단축시켜 기존 자원을 보다 효과적으로 운영할 수 있게 함. 
        테스터의 역량과 컨디션 관계없이 테스트의 양과 질을 상향 표준화 -->  비용 절감 품질 향상

* 유닛 테스트와 통합 테스트 중 한 가지 종류만 작업하는 경우에는 어떤 단점이 있을까요?

        1. 단위 테스트(O), 통합 테스트(X)

            어떤 이슈는 통합 테스트를 통해서만 발견할 수 있음 --> 시스템의 여러 부분과 관련 있는 이슈를 간과할 수 있음
                데이터 베이스 이슈(transaction, trigger, stored procedures 관련 이슈)
                다른 모듈이나 외부 모듈 사용하는 경우 또는 성능 검증해야 할 때 통합 테스트를 통해서만 검증 가능

        2. 단위 테스트(X), 통합 테스트(O)
            
            테스트 작성, 실행에 필요한 시간에 과도한 낭비
            테스트 복잡성 올라감 --> 테스트 디버깅 복잡함
            
* Mock과 Stub은 무엇일까요? 이들을 사용할 때 주의해야 할 점은 무엇일까요?

        Mock - A stub with an assertion that the method gets called. stub보다 구체적 검증. 테스트 코드를 지나치게 구체적으로 만들 수 있어 
               깨지기 쉬운 코드(코드 변경에 의해 영향 받기 쉬움) 만들기 쉬움
        Stub - override methods to return hard-coded values, also referred to as state-based. 테스트를 어렵게 만드는 코드 대체

## Quest
* Jest를 이용해, 클라이언트의 테스트, 서버의 테스트, 그리고 클라이언트와 서버의 통합(E2E) 테스트를 만들어 보세요.
