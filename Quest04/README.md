# Quest 04. 나의 첫 웹 서비스

## Introduction
* 이번 퀘스트에서는 웹 서비스를 만드는 기초적인 방법과 구성에 대해 알아보겠습니다.

## Topics
* 데몬
* 포트와 소켓
* 프록시(리버스 프록시, 포워드 프록시)

## Resources
* https://www.quora.com/What-is-a-daemon-process-in-Linux
* https://www.nginx.com/resources/glossary/reverse-proxy-server/

## Checklist
* 서버 환경에서 데몬이란 무엇일까요? node.js 서버를 데몬으로 만들려면 어떻게 해야 할까요?

        (1) 서비스의 요청에 대해 응답하는 오랫동안 실행 중인 백그라운드 프로세스. 유닉스 운영체제에서 이름이 'd'로 끝나는 프로세스들(httpd, sshd, named), 백그라운드 프로세스 중에서 부모 프로세스(PPID)가 1이거나 다른 데몬 프로세스인 프로세스

        sshd의 경우 비밀번호를 입력 받아 비밀번호 확인, passwd 파일에서 해당 계정에게 할당된 bash를 실행시키고 대기. ssh 연결 후 화면에 표시된 $는 sshd가 보여준 것이 아니라 sshd가 실행시킨 bash가 보여줌. sshd는 사용자에게 키보드를 통해 직접 입력 받지 않으며 직접 무언가 지시 불가

        - 백그라운드 프로세스
        사용자에게 무언가 키보드로 전달 받지 않고 스스로 동작하는 모든 프로세스, Foreground 프로세스는 표준입출력장치를 통해 대화

        (2) pm2 같은 프로세스 관리자 사용(forever 등)
        npm install -g forver; forever start app.js; forever stop 0

* nginx와 같은 리버스 프록시 서버를 사용하는 이유는 무엇일까요?

        클라이언트를 대신해서 한 대 이상의 서버로부터 자원을 추출하는 프록시 서버의 일종. 자원들이 마치 웹 서버 자체에서 기원한 것처럼 해당 클라이언트로 반환. 클라이언트들을 위해 임의의 서버에 접속하는 중간 매개체인 포워드 프록시와는 반대로, 관련 서버들을 위해 임의의 클라이언트가 해당 서버에 접속하는 중간 매개체.
        인터넷으로부터 요청을 받아와서 이것들을 내부망의 서버로 포워드. 프록시에 요청 하는 대상은 내부망 인지 못할 수 있음.

        실제 서버 IP 감춤. 보안, 성능(캐싱, 트래픽 분산) 등의 효과
        트래픽 분산 - 로드밸런싱으로 앤드 포인트(역할)마다 호출하는 서버 설정

        HTTPS의 인증서 관리를 하나의 프록시 서버가 담당하고 뒤에 동작하고 있는 서버는 HTTP로 서비스 가능

## Quest
* vi를 이용하여, node.js의 3000번 포트로 기본적인 웹서버를 만든 후 띄워 보세요.(O)

        /$ curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.34.0/install.sh | bash
           . ~/.nvm/nvm.sh
           nvm install node
           node -v

        /$cd home
        home$ cd ec2-user
        ~$ mkdir quest
        cd quest
        npm install express
        touch app.js
        vi app.js
        node app.js   // port 3000 connected

* 로컬 머신을 통해 띄운 웹서버의 3000번 포트에 접속해 보세요. AWS 콘솔에서 보안 그룹을 조정하려면 어떻게 해야 할까요?

        해당 EC2 인스턴스의 보안 그룹 인바운드 규칙 설정
        HTTP	TCP	80	0.0.0.0/0	 Allow all IPv4 traffic
        HTTP	TCP	80	::/0	         Allow all IPv6 traffic
        SSH	TCP	22	0.0.0.0/0

        :3000/process/check  // this is /process/check
        :3000/check1  // this is /check1

* 서버의 앞쪽에 nginx를 설치하여 80포트를 통해 외부로 서비스 해 보세요.

        sudo amazon-linux-extras install nginx1
        sudo service nginx start
        sudo service nginx status
        ps -ef | grep nginx
        sudo netstat -ntlp

* 서버에 터미널 접속을 종료해도 서버가 뜰 수 있게 해 보세요.

        npm install -g pm2
        pm2 start app.js
        pm2 stop 0
