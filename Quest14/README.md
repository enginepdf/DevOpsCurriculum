# Quest 14. 코드로 인프라 관리하기

## Introduction
* 이번 퀘스트에서는 인프라를 코드로 관리하는 법에 대해 알아보겠습니다.

## Topics
* IaC
* Terraform
  * State
  * Import
  * AWS Provider

## Resources
* https://en.wikipedia.org/wiki/Infrastructure_as_code
* https://www.terraform.io/intro/index.html
* https://www.44bits.io/ko/post/terraform_introduction_infrastrucute_as_code
* https://www.terraform.io/docs/state/index.html
* https://registry.terraform.io/providers/hashicorp/aws/latest/docs

## Checklist
* IaC(Infra as Code)는 어떤 개념인가요? 이러한 개념이 왜 생기게 되었을까요?

        클라우드를 이용한 서비스 제공이 증가함에 따라 개발 환경도 많은 변화가 있었는데 기존에 분리되어 있던 개발과 운영 환경을 통합하여 관리하고자 하는 문화인 DevOps를 도입하기 위해 인프라에 사용되는 모든 설정들을 코드로 관리하는 IaC 환경이 필요하게 됨(가시성, 자동화를 이용하여 수작업에 따른 에러 발생과 같은 복잡성 등을 줄이고 효율성, 생산성 향상)
           웹 콘솔을 사용해 리소스 관리를 하게 되면 리소스가 많아지기 시작하면 전체 리소스를 파악할 수 없는 시점이 오게 됨
            -> 테라폼 같은 IaC 툴을 쓰면 프로비저닝하고자 하는 상태를 코드로 명확히 기록해두어 웹 콘솔만 사용할 때보다 파악이 쉬워지고 세심한 관리 가능해짐
               저장소에서 이력 추적 가능, 깃허브에서 팀원들과 코드 리뷰 진행 가능(+ 작성한 코드에 대한 이유 등에 대한 투명성)
               CI를 사용해 코드 리뷰가 된 사항을 자동적으로 플랜 및 적용하는 것도 가능하다고 함

        Infrastructure as code (IaC) is the process of managing and provisioning computer data centers through machine-readable definition files, 
        rather than physical hardware configuration or interactive configuration tools. 
        The IT infrastructure managed by this process comprises both physical equipment, such as bare-metal servers, as well as virtual machines, and associated configuration resources. 
        The definitions may be in a version control system. It can use either scripts or declarative definitions, rather than manual processes, but the term is more often used to promote declarative approaches

        All continuous configuration automation (CCA) tools can be thought of as an extension of traditional IaC frameworks.
        They leverage IaC to change, configure, and automate infrastructure, and they also provide visibility, efficiency and flexibility in how infrastructure is managed. 
        These additional attributes provide enterprise-level security and compliance

        IaC can be a key attribute of enabling best practices in DevOps – Developers become more involved in defining configuration and Ops teams get involved earlier in the development process. 
        Tools that utilize IaC bring visibility to the state and configuration of servers and ultimately provide the visibility to users within the enterprise, aiming to bring teams together to maximize their efforts. 
        Automation in general aims to take the confusion and error-prone aspect of manual processes and make it more efficient, and productive. 
        Allowing for better software and applications to be created with flexibility, less downtime, and an overall cost effective way for the company. 
        IaC is intended to reduce the complexity that kills efficiency out of manual configuration. 
        Automation and collaboration are considered central points in DevOps; Infrastructure automation tools are often included as components of a DevOps toolchain

        Generally, DevOps tools fit into one or more activities, which supports specific DevOps initiatives: Plan, Create, Verify, Package, Release, Configure, Monitor, and Version Control

        * DevOps toolchain

            A DevOps toolchain is a set or combination of tools that aid in the delivery, development, and management of software applications 
            throughout the systems development life cycle, as coordinated by an organisation that uses DevOps practices.

        * There are generally two approaches to IaC: declarative (functional) vs imperative (procedural).

            The difference between the declarative and the imperative approach is essentially 'what' versus 'how'
            The declarative approach focuses on what the eventual target configuration should be; the imperative focuses on how the infrastructure is to be changed to meet this.
            The declarative approach defines the desired state and the system executes what needs to happen to achieve that desired state. 
            Imperative defines specific commands that need to be executed in the appropriate order to end with the desired conclusion

        * Continuous configuration automation (CCA) 
        
            It is the methodology or process of automating the deployment and configuration of settings and software 
            for both physical and virtual data center equipment


* 테라폼은 어떤 소프트웨어인가요? 다른 IaC와 비교하여 어떤 장점을 가지고 있을까요?

        Hashicorp에서 오픈소스로 개발 중인 infrastructure 관리 도구
        서비스 실행에 필요한 환경을 구축하는 도구 - 프로비저닝 도구(Chef, Ansible)
        IaC(Infrastructure as Code)를 지향하고 있는 도구로 GUI나 웹 콘솔을 사용해 서비스 실행에 필요한 리소스를 관리하는 대신 필요한 리소스들을 선언적인 코드로 작성해 관리할 수 있도록 함

        resource : 실제로 생성할 인프라 자원
        provider : Infrastructure Provider(AWS, Azure, Google Cloud Platform)
        backend : 테라폼의 상태를 저장할 공간 지정하는 부분 
        module : 공통적으로 활용할 수 있는 인프라 코드를 한 곳으로 모아서 정의하는 부분
        remote state : By default, Terraform stores state locally in a file named terraform.tfstate. 
        When working with Terraform in a team, use of a local file makes Terraform usage complicated because each user must make sure they always have the latest state data 
        before running Terraform and make sure that nobody else runs Terraform at the same time.

        With remote state, Terraform writes the state data to a remote data store, which can then be shared between all members of a team. 
        Terraform supports storing state in Terraform Cloud, HashiCorp Consul, Amazon S3, Azure Blob Storage, Google Cloud Storage, Alibaba Cloud OSS, and more.

        Remote state is implemented by a backend, which you can configure in your configuration's root module.

        - 장점 
          
            CloudFormation은 AWS에 종속적이지만 Terraform은 다양한 Provider 지원(클라우드에 비종속적)
            remote state를 사용하면 비밀번호 같은 중요 정보를 노출시키지 않아도 되고 협업에 도움 됨(remote state, backend.tf)
            배우면서 생각했는데 쉽고 어렵고의 관점보다는 ORM으로 Mysql 처리하는 것처럼 1:1 매핑을 위한 키워드 등을 알아야 한다는 단점이 있지만, 코드로 인프라 관리 가능(자동화)

            속도와 안전 

              코드로 실행 되어 수작업보다 빠르고 실수 방지 가능

            문서화

              코드를 통해 인프라 파악 가능

            형상 관리

              Git 등을 이용해 변경 기록 쉽게 볼 수 있음

            리뷰 및 테스트

              배포 전에 코드 리뷰와 테스트를 통해 문제 발생을 예방
            
            재사용

              한번 terraform 코드 사용해서 인프라 구축하면 그 후 추가하는 인프라들을 구성하기 쉬워짐

* 테라폼의 State는 무엇일까요? 기존에 AWS 콘솔을 통해 정의된 리소스를 테라폼의 State에 가져오려면 어떻게 해야 할까요?

        Terraform must store state about your managed infrastructure and configuration. This state is used by Terraform to map real world resources to your configuration,
        keep track of metadata, and to improve performance for large infrastructures

        This state is stored by default in a local file named "terraform.tfstate", but it can also be stored remotely, which works better in a team environment.

        The primary purpose of Terraform state is to store bindings between objects in a remote system and resource instances declared in your configuration. 
        When Terraform creates a remote object in response to a change of configuration, it will record the identity of that remote object 
        against a particular resource instance, and then potentially update or delete that object in response to future configuration changes.

        Terraform expects a one-to-one mapping between configured resource instances and remote objects. 
        Normally that is guaranteed by Terraform being the one to create each object and record its identity in the state, or to destroy an object and then remove the binding for it

        - 기존에 AWS 콘솔에서 정의된 리소스를 테라폼의 state로 가져오기

          terraform import [options] ADDRESS ID

            Import will try and find the infrastructure resource identified with ID and import the state into terraform.tfstate with resource id ADDRESS

            local의 .terraform에 해당 리소스의 상태 정보를 저장해주는 역할
            Apply 전까지는 backend에 저장되지 않음
            import 이후 plan을 하면 로컬에 해당 코드가 없어 리소스가 삭제 또는 변경되다는 결과를 받음

          variable "access_key" {}
          variable "secret_key" {}

          provider "aws" {
            access_key = "${var.access_key}"
            secret_key = "${var.secret_key}"
          }

          This example will import an AWS instance into the aws_instance resource named foo:
            $ terraform import aws_instance.foo i-abcd1234

          The example below will import an AWS instance into the aws_instance resource named bar into a module named foo:
            $ terraform import module.foo.aws_instance.bar i-abcd1234

          The example below will import an AWS instance into the first instance of the aws_instance resource named baz configured with count:
            $ terraform import 'aws_instance.baz[0]' i-abcd1234

## Quest
* 지금까지 구축했던 다음의 인프라를 모두 삭제하고(O) 테라폼 코드로 재구축해 보세요.
  * VPC
  * Fargate 기반의 API 서비스
  * 람다 기반의 서비스
  * S3와 Cloudfront 기반의 정적 웹사이트