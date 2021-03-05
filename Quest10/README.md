# Quest 10. 자동화된 테스트

## Introduction
* 이번 퀘스트에서는 자동화된 테스트에 어떤 장점이 있는지, 어떤 식으로 구축할 수 있는지에 대해 알아보겠습니다.

        기술적인 세부사항보다 비즈니스 요구사항을 나타내는 테스트를 작성
        내부 구현을 테스트하는 대신 테스트는 동작을 테스트해야 함(내부 구현과 강하게 결합 되어 있다면 새로운 기능 추가 시 기존 테스트들을 리팩토링하는 작업이 많아짐)
        잘 작성된 테스트 케이스는 사전과 같다고 함

## Topics
* 유닛 테스트

        Unit testing — With unit tests we are testing small isolated pieces of our code.
        셋업이 필요하지 않고 복잡도 낮고 속도가 빠름. 클래스/메쏘드 대상, 그 외 부분은 mocked, stubbed 처리
        유지보수가 통합 테스트보다 용이하다고 함(테스트 케이스가 꼼꼼히 작성되어 있다면, 개발 과정 중에 미리 문제 파악 가능. 개발 스타일, 표준, 컨벤션 등을 공유하기 좋음)
        새로 개발하거나 변경하는 부분을 대상으로 우선적으로 단위 테스트를 시작하는 것이 좋다고 함
        기존의 레거시 코드는 모두 단위 테스트 처리하는 것보다는 E2E 테스트로 먼저 처리해보라고 함

* Mock과 Stub

        Mock : imitation, Mock Object란 주로 객체 지향 프로그래밍으로 개발한 프로그램을 테스트 할 경우 테스트를 수행할 모듈과 연결되는 외부의 다른 서비스나 모듈들을 실제 사용하는 모듈을 사용하지 않고 실제의 모듈을 "흉내"내는 "가짜" 모듈을 작성하여 테스트의 효용성을 높이는데 사용하는 객체. 사용자 인터페이스(UI)나 데이터베이스 테스트 등과 같이 자동화된 테스트를 수행하기 어려운 때 널리 사용

        Method Stub : 스텁은 소프트웨어 개발에 쓰이고 다른 프로그래밍 기능을 대리하는 코드. 스텁은 기존 코드(예를 들어 원격 머신의 프로시저)를 흉내내거나 아직 개발되지 않은 코드를 임시로 대치하는 역할을 수행. 일반 소프트웨어 개발과 테스팅을 포함하여 특히 이식과 분산 컴퓨팅에 유용

* 통합 테스트

        Integration testing — In this type of testing we combine and test individual units and test them as a group.
        셋업이 필요하고 복잡도 중간에 속도 느림(외부 시스템과의 Input/Output 수행). 컴포넌트/서비스 대상. 특정 데이터를 집어넣은 결과를 살펴보는 테스트(단위 테스트로 검증 가능한 모든 이슈는 통합 테스트를 통해 검증 가능)
        외부 시스템은 전부 모킹, 교체(인메모리 데이터베이스로 처리 되거나 그대로 사용될 수 있음))

* E2E 테스트(사용자 경험과 관련)

        End-to-end testing is a technique that tests the entire software product from beginning to end to ensure the application flow behaves as expected
        The main purpose of End-to-end (E2E) testing is to test from the end user’s experience by simulating the real user scenario and validating 
        the system under test and its components for integration and data integrity.
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
        테스터의 역량과 컨디션 관계없이 테스트의 양과 질을 상향 표준화 -->  비용 절감 및 품질 향상

* 유닛 테스트와 통합 테스트 중 한 가지 종류만 작업하는 경우에는 어떤 단점이 있을까요?

        1. 단위 테스트(O), 통합 테스트(X)

            어떤 이슈는 통합 테스트를 통해서만 발견할 수 있음 --> 시스템의 여러 부분과 관련 있는 이슈를 간과할 수 있음
                데이터 베이스 이슈(transaction, trigger, stored procedures 관련 이슈)
                다른 모듈이나 외부 모듈 사용하는 경우 또는 성능 검증해야 할 때 통합 테스트를 통해서만 검증 가능

        2. 단위 테스트(X), 통합 테스트(O)
            
            테스트 작성, 실행에 필요한 시간에 과도한 낭비
            테스트 복잡성 올라감 --> 테스트 디버깅 복잡함(단위 테스트를 통해 개별 모듈의 if-else문을 통해 나타날 수 있는 상황의 갯수를 내부에서 고려하지 않아서 모듈간 고려를 하면 각 모듈에서 발생 가능한 상황들의 곱이 전체 발생 가능한 상황의 갯수가 됨. 에러 발생 시 어디가 문제인지 확인할 곳이 많아지게 됨)
            
* Mock과 Stub은 무엇일까요? 이들을 사용할 때 주의해야 할 점은 무엇일까요?

        특정 상황을 만들기 위해 반복해서 Mock 객체에 Stub을 통해 의도한 결과를 만드는 것이 필요한 경우, 부모 클래스나 메소드로 분리 -> 테스트 작성자는 테스트로 검증하려는 대상에만 집중 가능
        테스트는 입력값에 대해 결과값을 확인하는 방식으로 작성. 의존하는 객체는 Mock 객체를 사용, 그 과정에서 특정 상황을 발생시키기 위해 의도하는 동작을 하게끔 하는 경우만 Stub 처리 함

        Mock - "superset" of stubs
               A stub with an assertion that the method gets called. 
               very similar to Stub but interaction-based rather than state-based. This means you don't expect from Mock to return some value, but to assume that specific order of method calls are made
               a dummy piece of code, that you verify is called correctly as part of the test

               A mock is an object used to fake a method that has pre-programmed behavior as well as pre-programmed expectations. If these expectations are not met then the mock will cause the test to fail

               Mocks are objects that register calls they receive. In test assertion, we can verify on Mocks that all expected actions were performed. Such as: a functionality that calls e-mail sending service

               Mock Object란 주로 객체 지향 프로그래밍으로 개발한 프로그램을 테스트 할 경우 테스트를 수행할 모듈과 연결되는 외부의 다른 서비스나 모듈들을 실제 사용하는 모듈을 사용하지 않고 실제의 모듈을 "흉내"내는 "가짜" 모듈을 작성하여 테스트의 효용성을 높이는데 사용하는 객체. 사용자 인터페이스(UI)나 데이터베이스 테스트 등과 같이 자동화된 테스트를 수행하기 어려운 때 널리 사용
               테스트 코드를 지나치게 구체적으로 만들 수 있어 깨지기 쉬운 코드(코드 변경에 의해 영향 받기 쉬움) 만들기 쉬움

               var Mock = {
                        calls: {
                                method_a: 0
                        }

                        method_a: function(param_a, param_b){
                                this.method_a++; 
                                console.log('Mock.method_a its been called!');
                        }
                }  --> Mock is like actually stepping into the method and making sure everything inside is correct before returning the correct value.

        Stub - override methods to return hard-coded values, also referred to as state-based. 
               a dummy piece of code that lets the test run, but you don't care what happens to it.
               A stub is an object used to fake a method that has pre-programmed behavior. You may want to use this instead of an existing method in order to avoid unwanted side-effects
               Stub is an object that holds predefined data and uses it to answer calls during tests. Such as: an object that needs to grab some data from the database to respond to a method call.
               테스트를 어렵게 만드는 코드 대체
               스텁은 소프트웨어 개발에 쓰이고 다른 프로그래밍 기능을 대리하는 코드. 스텁은 기존 코드(예를 들어 원격 머신의 프로시저)를 흉내내거나 아직 개발되지 않은 코드를 임시로 대치하는 역할을 수행. 일반 소프트웨어 개발과 테스팅을 포함하여 특히 이식과 분산 컴퓨팅에 유용

               var Stub = {
                        method_a: function(param_a, param_b){
                                return 'This is an static result';
                        }
               }  --> Stub is like making sure a method returns the correct value

## Quest
* Jest를 이용해, 클라이언트의 테스트, 서버의 테스트, 그리고 클라이언트와 서버의 통합(E2E) 테스트를 만들어 보세요.(O)

        https://github.com/enginepdf/Quest 
        cd ClientQ
        npm start (테스트를 로컬에서 클라이언트 실행 상태로 해야 작동)