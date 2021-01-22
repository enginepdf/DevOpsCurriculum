# Quest 06. 컨테이너

## Introduction
* 이번 퀘스트에서는 컨테이너 기술과 그 활용에 대해 알아보겠습니다.

## Topics
* 컨테이너 기술
* Docker
* docker-compose

## Resources
* https://www.docker.com/101-tutorial
* https://docker-curriculum.com/
* https://docs.docker.com/compose/

## Checklist
* 컨테이너는 어떻게 동작하나요? 다른 배포판을 사용할 수 있게 하는 원리가 무엇일까요?

        Cgroup(Control Group), namespace와 같은 커널 기반의 기술을 이용해서 프로세스 완벽하게 격리하여 분리된 환경을 만들고 실행
        Cgroup : 시스템의 CPU 시간, 시스템 메모리, 네트워크 대역폭과 같은 자원 제한, 격리 할 수 있는 커널 기능
        namespace : 시스템 리소스를 프로세스 전용 자원처럼 보이게 하고, 다른 프로세스와 격리시키는 기능

* 도커 컨테이너에 호스트의 파일시스템이나 네트워크 포트를 연결하려면 어떻게 해야 할까요?

        (1) 파일 시스템 연결 : docker -v <Host system directory>:<container directory> <image>
        (2) 네트워크 포트 연결 : docker run -d -p <Host Port>:<Container Port> <image>   // option -d for detached mode, background

* 도커 컨테이너에서 런타임에 환경변수를 주입하려면 어떻게 해야 할까요?

        (1) Dockerfile에서 ENV <ENV name> <ENV value>
        (2) sudo docker run -e <ENV name>=<ENV value> -e <ENV name>=<ENV value> ...(-e <ENV name>=<ENV value>)

* 도커 컨테이너의 stdout 로그를 보려면 어떻게 해야 할까요?
    
        docker logs <container-id>

* 실행중인 도커 컨테이너에 들어가 bash 등의 쉘을 실행하고 로그 등을 보려면 어떻게 해야 할까요?

        docker exec -ti <docker-image id> /bin/bash
        docker run -ti bash -c <CMD>
        docker attach <container id or name>

## Quest
* 도커를 설치하고 그 사용법을 익혀 보세요.(O)

        // https://docs.aws.amazon.com/ko_kr/AmazonECS/latest/developerguide/docker-basics.html
        sudo yum update -y
        sudo amazon-linux-extras install docker
        sudo service docker start
        sudo usermod -a -G docker ec2-user  // ec2-user 사용하지 않고도 도커 명령 실행 위해
        docker info
        // sudo chkconfig docker on    // make docker auto-start
        // sudo reboot

* Quest 05에서 작업한 서버를 컨테이너 기반으로 띄울 수 있게 수정해 보세요. (docker-compose는 사용하지 않습니다)

        git clone https://github.com/enginepdf/Quest05  // docker build https://github.com/enginepdf/Quest05.git
        cd Quest05
        sudo service nginx start
        sudo service docker start
        docker build -t quest05:1.0 .
        docker images     // docker rmi <image>:<tag>   to remove the image
        ps aux | grep docker
        docker run -d -p 3000:3000 --name quest05 quest05:1.0
        docker ps -a

        /check   // this is /process/check
        /check1  // this is /check1


        // docker stop quest05
        // docker restart quest05


* 컨테이너를 Docker Hub에 올리고, EC2에서 해당 컨테이너를 띄워서 서비스 해 보세요.

        sudo service nginx start
        sudo service docker start
        docker login
        docker build -t <Docker Hub ID>/<image>:<tag> .   // docker build <image> . 
        docker push <Docker ID>/<Image name>:<tag>
        docker rmi <Docker ID>/<Image name>:<tag>
        
        docker pull <Docker ID>/<Image name>:<tag>
        docker images
        docker run -d -p 3000:3000 --name quest05 <Docker ID>/quest05:1.0
        docker ps -a
        /check   // this is /process/check
        /check1  // this is /check1


* docker-compose를 사용하여, 빌드와 서버 업/다운을 쉽게 할 수 있도록 고쳐 보세요.

        sudo curl -L "https://github.com/docker/compose/releases/download/1.25.3/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
        sudo chmod +x /usr/local/bin/docker-compose
        docker-compose --version
        docker-compose up --build  
        docker-compose ps
        docker-compose down
