# Quest 01. 리눅스와 친해지기

## Introduction
* 이번 퀘스트를 통해 리눅스의 기본적인 구조와 기능에 대해 공부할 수 있습니다.

## Topics
* 리눅스의 기본 커맨드
  * `cd`, `pwd`, `ls`, `cp`, `mv`, `mkdir`, `rm`, `touch`, `ln`, `echo`, `cat`, `tail`, `find`, `ps`, `kill`, `grep`, `wc`, `df`, `du`
  * 파이프(`|`) 문자
* 리눅스의 기본적인 디렉토리 구성
  * `/bin`, `/usr/bin`, `/boot`, `/dev`, `/etc`, `/home`, `/lib`, `/mnt`, `/proc`, `/root`, `/sbin`, `/usr/sbin`, `/tmp`, `/usr`, `/var`
* 쉘과 환경변수와 퍼미션
  * sh, bash, zsh
  * `.bash_profile`, `.bashrc`, `.zshrc`
  * `env`, `set`, `unset`, `export`
  * `chmod`, `chown`, `chgrp`
  * setuid, Sticky bit
* 운영체제의 기초
  * 프로세스와 쓰레드
  * 파일 시스템
* 리눅스의 배포판
  * Ubuntu, Debian, Redhat Enterprise, CentOS, Gentoo, Amazon Linux
  * 패키지 시스템: `apt`(.deb), `yum`(.rpm)
* vi
  * `i`, `w`, `q`, `u`, `d`, `p` 명령

## Resources
* https://ubuntu.com/tutorials/command-line-for-beginners#1-overview
* https://www.howtogeek.com/117435/htg-explains-the-linux-directory-structure-explained/
* https://www.tutorialspoint.com/unix/unix-what-is-shell.htm
* https://github.com/ohmyzsh/ohmyzsh/wiki/Installing-ZSH
* https://www.infoworld.com/article/2832405/what-is-systemd-and-why-does-it-matter-to-linux-users.html
* https://thebloggingpot.com/2018/05/23/different-linux-distributions-explained/
* https://developer.ibm.com/technologies/linux/tutorials/l-lpic1-102-5/
* https://developer.ibm.com/technologies/linux/tutorials/l-lpic1-103-8/

## Checklist
* 리눅스의 파이프 문자는 어떤 역할을 하나요?
  --> 첫 번째 명령어 파이프의 결과를 다른 명령어 파이프로 흘러가게 함.
      하나의 표준 출력을 다른 명령의 표준 입력으로 사용

* 리눅스의 쉘은 어떤 역할을 하나요? bash와 zsh는 어떻게 다른가요?
  --> 사용자와 커널 사이에서 명령을 해석하여 전달하는 해석, 번역 기능 + 프로그래밍 기능 + 사용자 환경 설정 기능
  /etc/shells
  bash에서는 파이프 라인의 모든 명령이 하위 쉘에서 실행. zsh에서 파이프 라인의 마지막 명령은 현재 쉘에서 실행

* 리눅스의 권한 체계는 어떻게 이루어져 있나요?
  --> ls -l (ll)
      읽기(r, 4), 쓰기(w, 2), 실행(x, 1)


* 프로세스와 쓰레드는 무엇인가요?
  쓰레드는 프로세스가 할당받은 자원을 이용하는 실행의 단위
  프로세스는 독립된 메모리 영역(code, data, stack, heap의 구조)을 할당, 최소 1개의 쓰레드(메인 쓰레드) 가짐, 운영체제로부터 시스템 자원을 할당받는 작업의 단위(실행된 프로그램), 메모리에 올라와 실행되고 있는 프로그램의 인스턴스

* 현재 실행되고 있는 프로세스들 중 PID가 1인 프로세스는 어떤 역할을 할까요?
  init 프로세스
  파일 시스템의 구조 검사, 마운트, 서버 데몬, 사용자 로그인 시 사용자 위한 쉘 시작   /etc/inittab

* 파일시스템이란 무엇일까요? 어떤 것이 있을까요? 지금 다루는 운영체제는 어떤 파일시스템을 쓰고 있나요?
  컴퓨터에서 파일이나 자료를 쉽게 발견 및 접근할 수 있도록 보관 또는 조직하는 체제
  디스크, 네트워크, 특수 용도의 파일 시스템으로 나뉨
  Windows - NTFS, FAT
  MAC - HFS plus
  Linux - ZFS, XFS, ext

* 리눅스의 배포판이란 무엇일까요? 여러 가지 배포판들은 어떻게 생겨났을까요?
--> 사용자 친화적인 수준, 요구하는 컴퓨터 사양, 리눅스에 대한 깊은 지식 요구 여부 등에 따라 다양한 리눅스 버전(리눅스   배포판)이 존재. 필요로 하는 상황에 따라 만들어진 것으로 보임.
    소프트웨어 사이에서 의존성이 발생. 소프트웨어 조합을 잘 구성해서 하나의 시스템으로 패키징한 것을 배포판이라고 부름




* 리눅스의 패키지 시스템이란 무엇일까요? 이러한 시스템이 생긴 이유는 무엇일까요?
  기존에는 소스 코드를 직접 받아 소프트웨어 설치를 했지만 일반 사용자들도 쉽게 사용할 수 있게 돕기 위해 만들어짐


* vi는 어떤 에디터인가요? vi와 vim은 어떻게 다를까요? vi는 왜 모든 리눅스의 기본 에디터가 되었을까요?
--> vim(Vi Improved) : graphical mode(gvim) 기능 추가
    vi는 유닉스에서만 가능, vim은 windows, mac, amiga 등에서도 가능, 문법 강조 기능 지원
    vim은 X-Windows, Mac, Windows 등 GUI 환경에서 잘 구동
    Emacs가 유료였던 시절에 텍스트 편집이 편한 vi가 인기를 끔, 텍스트 조작하는 기능에 치중

## Quest
* 인스턴스 생성
  * t3.nano 등급으로 EC2 인스턴스를 생성해 봅시다! Amazon Linux 2, Ubuntu, CentOS 세 가지를 각각 생성해 봅니다.(O)
  * EC2 생성 과정에서 .pem 파일이 하나 생기는데, 이는 저에게 슬랙을 통해 공유해 주시면 됩니다.(O)
  * 세 배포판은 어떻게 다른가요?
    --> Amazon Linux2는 Red Hat Enterprise Linux 배포판, Amazon 웹 서비스와의 호환이 좋다, yum이 빠르게 동작
    Ubuntu는 데이안 기반, 업데이트 주기가 짧음, apt(advanced package tool) 패키지 사용, 시장 점유율 가장 높음
    CentOS는 레드헷 기반, 업데이트 주기가 길다, 보안이 강함, 서버 시장에서 대기업에서 선호, yum 패키지 사용

* 리눅스 연습
  * Amazon Linux 2 인스턴스에서 위의 Topics의 기본 커맨드를 연습해 봅니다.(O)
  * 리눅스의 기본 디렉토리들에 어떤 정보들이 있는지 둘러 봅니다.(O)
  * zsh를 설치하고 `.zshrc` 파일을 포함해 여러 가지 설정을 해 봅니다.(O)
  * Topics의 환경변수나 퍼미션 관련 커맨드를 연습해 봅니다.(O)
  * 현재 실행되고 있는 프로세스들과 마운트 된 파일시스템들을 확인해 봅니다.(O)
    ps
    mount
    cat /etc/mtab

  * vi를 열어 여러 가지 기본 명령과 간단한 편집 방법을 연습해 봅니다.(O)
* 생성한 인스턴스 중 Ubuntu와 CentOS는 완전히 종료(Terminate)하고, Amazon Linux 2는 일단 꺼둡니다.(O)
