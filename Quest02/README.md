# Quest 02. 프로그래밍의 기초

## Introduction
* 이번 퀘스트를 통해 가장 기초적인 프로그래밍과 그를 둘러싼 개념을 익힙니다.

## Topics
* node.js
* 컴파일러, 인터프리터, JIT 컴파일, 가상머신, 런타임
* `let`, `const`, `if`, `for`, `function`

## Resources
* https://nodejs.org/ko/download/package-manager/
* https://nodejs.org/ko/

## Checklist
* node.js란 무엇일까요? 
  비동기 이벤트 주도 javascript 런타임으로 확장성 있는 네트워크 애플리케이션을 만들 수 있도록 설계
  blocking이 없어 프로세스의 교착상태에  대해 걱정할 필요 없음


* `node.js는 V8 엔진 위에 만들어진 자바스크립트 런타임이다`라는 문장을 뜯어 보면 어떤 의미일까요?

   작동을 위해 V8이 꼭 필요하다. 
   V8은 자바스크립트를 바이트코드로 컴파일하고 실행하는 방식 사용(JIT 컴파일)
* 사람의 언어에 가까운 프로그램 코드를 어떻게 컴퓨터가 실행시킬까요? 그 과정은 무엇일까요?

## Quest
* node.js 최신 LTS 버전을 설치합니다.(O)
* 다음과 같이 입력에 따라 별의 산을 툴력하는 프로그램을 작성합니다.
```
$ node quest02.js 5
    *
   ***
  *****
 *******
*********
```
* 위 프로그램을 더 좋은 구조로 짜려면 어떻게 해야 할까요?
