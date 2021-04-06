# Quest 15. 모노리포와 린팅

## Introduction
* 이번 퀘스트에서는 모노리포의 개념, 그리고 코드 파이프라인의 단계 중 하나인 린팅에 대해 다루겠습니다.

## Topics
* Monorepo
* Lerna
* Lint (eslint)

## Resources
* https://medium.com/better-programming/the-pros-and-cons-monorepos-explained-f86c998392e1
* https://github.com/lerna/lerna
* https://en.wikipedia.org/wiki/Lint
* https://eslint.org/

## Checklist
* 모노리포는 어떤 개념인가요? 모노리포를 적용할 때의 장단점은 무엇일까요?

        Mono Repository is an architectural pattern where a single repository will contain all the code for a given project
        you can have several apps in one repo(website code base, mobile app code base, etc)
        <-> Multirepo(Polyrepo)

        장점

            One source of truth

                Instead of having a lot of repositories with their own configs, we can have a single configuration to manage all the projects, making it easier to manage

            Code reuse

                If there is a common code or a dependency that has to be used in different projects, we can actually share them easily

            Transparency

                It gives us visibility of code used in every project. We will be able to check all the code in a single place

            Atomic changes

                We can make a single change and reflect the changes in all the packages, thus making development much quicker

        단점

            Unable to restrict access

                Every member who has access to the repo will have access to view all the code. There’s no way to restrict some parts of the app, which is irrelevant to the user

            Long build times

                Since there is a lot of code in one place, the build time is much longer compared to building separate projects independently

            Git performance

                Since the whole code is in one place, and since a lot of developers commit and work together over the same repo, there are a lot of commits that are tracked by Git. 
                Since the number of commits is huge, Git’s performance becomes slow as the history deepens


* Lerna는 어떤 역할을 하는 소프트웨어인가요?

        Lerna is a tool used to manage JS projects with multiple packages. It is built on Yarn
        Yarn is a JS dependency management tool that also supports monorepos through workspaces

        Monorepo tools provide several functionalities, such as bootstrapping and linking the local packages among themselves, building the code, validating them, publishing them to a package registry, etc

* 린팅은 어떤 개념인가요? 린팅을 할 때의 장점은 무엇일까요? 자바스크립트의 경우 어떻게 구현하는 것이 좋을까요?

        Lint (software), a tool to analyze and find problems in source code
            코드 가독성, 코드 품질 향상, 유지 보수에 도움이 된다고 함
        eslint 사용(npm install eslint)
        eslint --init 
            .eslintrc.js

        ```.eslintrc.js

        module.exports = {
            "env": {
                "browser": true,
                "es6": true,
                "node": true
            },
            "rules": {  // 0 === 'off', 1 === 'warn', 2 === 'error' 
                "semi": ["error", "always"]            
            }
        };
        
        ```

## Quest
* 지금까지 만든 세 개의 자바스크립트 코드베이스(컨테이너 기반 API, 람다 기반 API, 정적 웹사이트)를 Lerna를 이용해 하나의 리포로 합쳐 보세요.
        
        npx lerna init
        
        ```lerna.json
        {
            "npmClient": "npm" || "yarn",
            "bootstrap": {
                "ignore": "" ,
                "npmClientArgs": [ "npm install " ],
            },
            "packages": [ "packages/*" ],
            // "useWorkspaces": true,
            "version": "0.0.0"
        }
        ```

* eslint의 기본 설정을 이용해 이 코드들을 린팅해 보세요. 모든 패키지를 명령 하나로 한 번에 린팅하려면 어떻게 해야 할까요?
        
        ```packages/package.json

            "scripts": {
                "lint": "eslint .js --quiet"
            }
        ```
        
        npx lerna run lint