# Quest 05. 형상관리툴

## Introduction
* git은 2021년 현재 개발 생태계에서 가장 각광받고 있는 버전 관리 시스템입니다. 이번 퀘스트를 통해 git의 기초적인 사용법을 알아볼 예정입니다.

## Topics
* git
  * `git clone`, `git add`, `git commit`, `git push`, `git pull`, `git branch`, `git stash`
* GitHub

## Resources
* https://try.github.io
* http://pcottle.github.io/learnGitBranching

## Checklist
* 형상 관리 시스템은 왜 필요한가요?

        소프트웨어 개발 및 유지 보수 과정에서 발생하는 소스 코드, 문서 등 각종 결과물(형상)에 대한 변경사항을 체계적 관리하고 제어하기 위함

* 분산형 형상관리 시스템이란 무엇일까요? git과 같은 분산형 시스템의 장점은 무엇일까요?

        각 개발자가 중앙 서버에 접속하지 않은 상태에서도 코드 작업 가능
        서버에 문제가 생겨도 클라이언트에서 다운 받은 복제물을 통해 복원 가능
        여러 사람이 동일 소스 코드를 공유해 효율적 작업
        소스 코드 공유할 때 생기는 버전 충돌 문제 해결
        여러 개의 버전으로 분기해서 개발 가능


* git의 clone/add/commit/push/pull/branch/stash 명령은 무엇이며 어떨 때 이용하나요? 그리고 어떻게 사용하나요?

        - git clone

            클라이언트가 파일들의 마지막 스냅샷을 가져오는 대신, 저장소를 통째로 복사. 
            클라이언트 상에 아무 것도 없을 때 서버의 프로젝트를 내려받는 명령어

        - git add 

            working directory(작업 디렉토리) 상의 변경 내용을 staging area(스테이징 영역)에 추가하기 위해 사용하는 명령어

        - git commit 

            변경 내용의 확정(Head - 로컬 저장소 에 반영)

        - git push

            원격 저장소에 commit 저장

        - git pull

            다른 사람이 Pull Request를 통해 코드 업데이트 했거나, Github를 통해 commit 했을 때 그 내용을 클라이언트로 내려받는 명령어

        - git branch

            독립적으로 작업하기 위한 영역

        - git stash

            git stash   // 아직 마무리하지 않은 작업을 스택에 잠시 저장할 수 있도록 하는 명령어. commit 하지 않고 나중에 다시 작업 재개
                        // stash의 저장 영역은 스택 구조
                        // === git stash save  스택에 새로운 stash가 만들어지고 working directory는 깨끗해짐

            git stash list // cat .git/refs/stash

            git stash apply // 작업 가져오기

            git stash apply --index // Staged 상태가지 복원

            git stash drop [stash name] // 스택에 남아 있는 stash를 제거

            git stash pop  // git stash apply +  git stash pop

            git stash show -p | git apply -R  // 가장 최근의 stash 사용하여 패치 만들어 그것을 거꾸로 적용.(stash 적용한 것 되돌림)

## Quest
* github의 개인 계정에 이번 퀘스트를 위한 리포를 만들어 보세요.(O)

      https://github.com/enginepdf/Quest05

* Quest 04에서 vi로 입력한 웹 서비스 대신 github의 저장소를 clone하여 같은 일을 하는 서버를 띄워 보세요.(O)

      sudo yum install git
      git clone https://github.com/enginepdf/Quest05
      cd Quest05
      npm install
      sudo service nginx start
      node app.js  // port 3000 connected

      /check    // this is /process/check
      /check1   // this is /check1

      // sudo service nginx stop

* 리포지토리에 민감한 정보(개인정보, 패스워드 등)가 들어가지 않도록 주의합니다.(O)
