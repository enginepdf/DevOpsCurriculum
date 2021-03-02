# Quest 08. 배포 자동화 하기

## Introduction
* 이번 퀘스트에서는 여러 대의 서버에 자동 배포를 구현하는 방법에 대해 알아보겠습니다.

## Topics
* Systems Manager
* Fargate
* Blue/Green Deployment

## Resources
* https://aws.amazon.com/ko/systems-manager/
* https://aws.amazon.com/ko/fargate/?whats-new-cards.sort-by=item.additionalFields.postDateTime&whats-new-cards.sort-order=desc&fargate-blogs.sort-by=item.additionalFields.createdDate&fargate-blogs.sort-order=desc
* https://www.redhat.com/ko/topics/devops/what-is-blue-green-deployment

## Checklist
* AWS의 Systems Manager는 어떤 서비스인가요?

        AWS 인프라에 대한 가시성과 제어 제공
        통합된 사용자 인터페이스 제공 -> 여러 AWS 서비스의 운영 데이터를 보고 AWS 리소스 전체에서 운영 작업 자동화 가능
        EC2 인스턴스, Amazon EKS 클러스터, S3 버킷 또는 RDS 인스턴스와 같은 리소스를 애플리케이션별 그룹화, 모니터링, 문제 해결을 위해 운영 데이터를 보고, 
        사전 승인된 변경 워크플로를 구현, 리소스 그룹에 대한 운영 변경 감사 가능
        리소스 및 애플리케이션 관리 간소화, 운영 문제를 탐지 및 해결하는 시간 단축, 인프라를 대규모로 안전하게 운영 및 관리할 수 있게 해줌

* AWS의 Fargate는 어떤 서비스인가요? 어떤 장점을 가지고 있나요?

        컨테이너에 적합한 서버리스 컴퓨팅 엔진으로, Amazon ECS, EKS에서 모두 작동.
        애플리케이션을 빌드하는 데 보다 쉽게 초점을 맞출 수 있도록 해 줌
        서버를 프로비저닝하고 관리할 필요가 없어 애플리케이션별로 리소스를 지정하고 관련 비용을 지불할 수 있고, 계획적으로 애플리케이션을 격리하여 보안 성능을 향상시킬 수 있음
        적당량의 컴퓨팅을 할당하여 인스턴스를 선택하고 클러스터 용량을 조정할 필요가 없음
        컨테이너를 실행하는 데 필요한 리소스 비용만 지불하여 추가 서버를 사용하기 위해 과도하게 프로비저닝하고 관련 비용 지불하지 않아도 됨.
        자체 커널에서 각 태스크 또는 팟을 실행하며, 이러한 태스크와 팟은 격리된 자체 컴퓨팅 환경에서 제공.
            -> 애플리케이션에서 워크로드를 격리하고 계획적으로 보안 성능 향상 가능

        * 프로비저닝 : 사용자의 요구에 맞게 시스템 자원을 할당, 배치, 배포해 두었다가 필요 시 시스템을 즉시 사용할 수 있는 상태로 미리 준비해 두는 것

* Blue/Green Deployment라는 것은 어떤 개념일까요?

        애플리케이션 또는 마이크로서비스의 이전 버전에 있던 사용자 트래픽을 이전 버전(blue 환경)과 거의 동일한 새 버전(green 환경)으로 점진적으로 이전하는 애플리케이션 릴리스 모델(이때, 두 버전 모두 프로덕션 환경에서 실행 상태 유지)
        프로덕션 트래픽이 blue에서 green으로 완전히 이전되면, blue는 롤백에 대비하여 대기 상태로 두거나 프로덕션에서 가져온 후 업데이트하여 다음 업데이트의 템플릿으로 삼을 수 있음
        (환경에 따라서는 업타임 요구 사항이 다르거나 blue-green과 같은 CI/CD 프로세스를 제대로 수행할 리소스가 없을 수 있음)
        ex) 온라인 상태의 사용자 수가 가장 적은 시간까지 기다리지 않고 사용량이 가장 많은 시점에도 로드 밸런서를 사용하여 각 사용자의 다음 트랜잭션을 blue에서 green으로 리디렉션 가능. 
        모든 프로덕션 트래픽이 green 환경을 거쳐 필터링되면 blue환경이 오프라인 상태가 됨. blue는 재해 복구 옵션이 되어 대기하거나 다음 업데이트를 위한 컨테이너가 될 수 있음.

        * 업타임 : 동작 중이면서 사용 가능한 기계(일반적으로 컴퓨터)의 시간을 백분율로 나타낸 시스템의 신뢰성의 측정(반의어는 다운타임)

## Quest
* AWS의 Systems Manager를 이용하여, 로컬 CLI 컨테이너 이미지를 배포하고 리모트 서버에서 그 이미지를 교체하여 띄울 수 있게 해 보세요. 한 개의 명령으로 이 모든 것이 이루어질 수 있게 하면 가장 좋습니다!   // AWS CLI -> 컨테이너 이미지 배포  aws ssm ~~   docker stop <image> docker run <image>

        https://docs.aws.amazon.com/ko_kr/systems-manager/latest/userguide/walkthrough-cli.html

        EC2 Instance(with SSM Agent) --> SSM

        IAM > Create role > AmazonEC2RoleforSSM > role name : AWSEC2RoleforSSM  --> apply to EC2 instances to manage

        AWS Systems Manager > Managed Instance > check the instance on the list

        EC2 > Instances > Launch instances > AMI(Amazon Linux2) > IAM role : AWSEC2RoleforSSM > Configure Security Group : open port 3000 inbound


        Systems Manager > Session Manager > choose the instance you want to access(배포의 경우 기본적인 설치는 되어 있다고 생각)
                        -->  sudo su
                             sudo yum update -y
                             curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.34.0/install.sh | bash
                             . ~/.nvm/nvm.sh
                             nvm install node
                             node -v

                             sudo amazon-linux-extras install docker
                             docker version

                             sudo yum install git
                             git version

                             sudo service docker start
        (aws ssm start-session --target i-0f184c6a223a7885d    // connecting to the instance(with Instance ID) )                     


        (Local CLI on your notebook)

                https://docs.aws.amazon.com/systems-manager/latest/userguide/session-manager-working-with-install-plugin.html#install-plugin-macos

                aws configure

                curl "https://s3.amazonaws.com/session-manager-downloads/plugin/latest/mac/sessionmanager-bundle.zip" -o "sessionmanager-bundle.zip"
                unzip sessionmanager-bundle.zip
                sudo ./sessionmanager-bundle/install -i /usr/local/sessionmanagerplugin -b /usr/local/bin/session-manager-plugin     // Installation successful!

                aws ssm send-command \
	                --document-name "AWS-RunShellScript" \
	                --targets '[{"Key":"InstanceIds","Values":["i-0f184c6a223a7885d"]}]' \
	                --parameters '{"commands":["#!/bin/bash","docker pull dockerdap12/quest05:1.0","docker run -d -p 3000:3000 --name quest dockerdap12/quest05:1.0"]}'

                instance public ip:3000/check1    // this is /check1
                instance public ip:3000/process/check    // this is /process/check
     
        --> 이미지를 교체하고 싶다면 commands 배열 내부에 기존 docker image를 stop 시키고 docker run <image>하면 됨.

        실행 중일 때 다시 같은 이미지 실행하면 자동 종료 후 실행하는 docker 명령

        ```build.sh

        #!/bin/bash
        build

        ```

        ```deploy.sh

        #!/bin/bash
        push
        command

        ```

* 이번에는 EC2 대신 Fargate를 이용하여 같은 서비스를 구현해 보세요.(O) 수동으로 배포하려면 어떻게 해야 할까요?

        A container -> B container(updated) -> B container error occurs 
                                            -> roll back to A or make C container 
                                 
        Fargate(ALB를 통해 task 2개로 요청 전달)

        ALB/check1    // this is /check1
        ALB/process/check     // this is /process/check

        Docker Hub 혹은 ECR에 이미지 업데이트 내용을 푸시하고 Fargate 서비스에서 roll update가 되도록 한다. push, force update


        * 수동으로 배포한다는 것 : 1) 도커 이미지를 만들어서 푸시 2) AWS 콘솔을 이용해서 이미지를 업데이트하고 태스크 버전을 올려주는 것


* Fargate에도 처음에 EC2에 한 배포 자동화를 구현해 보세요.
        
        https://docs.aws.amazon.com/ko_kr/AmazonECS/latest/developerguide/ECS_CLI_installation.html
        https://docs.aws.amazon.com/ko_kr/AmazonECS/latest/developerguide/ecs-cli-tutorial-fargate.html
        https://blog.ull.im/engineering/2019/01/01/aws-cli-ecs-fargate-service-discovery.html


        aws ecs create-cluster
        aws ecs register-task-definition
        aws ecs create-service
        --> 배포 전(또는 AWS 웹 상에서 먼저 세팅)

        ```build.sh

        #!/bin/bash
        docker build -t ECR . // docker build -t <dockerhub> . or docker-compose build -t <dockerhub> .

        ```

        ```deploy.sh

        #!/bin/bash

        docker push ECR-URL // 또는 docker push <image> or docker-compose push <image>
        aws ecs update-service \
                --region \
                --profile \
                --cluster <Cluster name> \
                --service <Service name> \
                --force-new-deployment
        
        ```
