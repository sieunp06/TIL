# Github Actions Runner 빌드 실전 적용기

> if(kakao)dev 2022의 데브옵스 세션의 [Gihub Actions Runner 빌드 실전 적용기](https://if.kakao.com/2022/session/73) 영상을 보고 정리한 글입니다.

## GitHub Actions Runner 빌드 실전 적용기

### Github Actions 이관 작업

kakao -> kakao enterprise

* 총 이관 데이터 크기 : 185GB
* 총 이관 Org 개수 : 172개
* 총 이관 Repo 개수 : 6384개

### 다양한 CI/CD 도구들

기존 사용하고 있던 도구들

* Bitbucket
* Tekton
* Jenkins
* Drone
* Bamboo

\-> 서비스마다 다른 CI/CD 도구를 사용하는 것은 결과적으로 구축 및 운영 비용을 높이고 협업을 어렵게 만듦

\-> 한 가지 CI/CD 도구를 사용할 수는 없을까?

### Github Action

* github enterprise 솔루션에 기본적으로 제공되는 서비스
* 에이전트 설치만으로 중앙에서 운영이 가능함
* github 저장소에서 매우 쉽게 빌드 가능함.

#### Github Actions의 빌드 방식

![Github Action의 빌드 방식](../images/데브옵스/Github-Actions-Runner-빌드-실전-적용기/if\(kakao\)\_devops\_github\_runners.png)

1. github Action의 빌드는 github events를 통해 발생된다.
   * github Events: push, pull, pull request 등 github에서 일어날 수 있는 모든 event들
2.  github Events가 발생되면 github Action Runner는 해당 project 빌드를 시작함.

    * github Action Runner에는 Workflows라는 item이 담기게 됨.
    * github Action Runner는 Workflows를 수행하게 됨.

    ```
    Workflows
        ㄴ Jobs
        ㄴ Steps
    ```

    * Workflows는 크게 Jobs, Steps로 이루어져 있음.
      * Step : Workflow 실행 최소 단위
        * Step이 모두 정상적으로 실행되어야 상위 단위인 Job이 성공하고, Job이 에러 없이 모두 실행되어야 Workflow가 성공하여 빌드가 성공됨.

#### Github Actions를 구성하는 방법

![Github Actions를 구성하는 방법](../images/데브옵스/Github-Actions-Runner-빌드-실전-적용기/if\(kakao\)\_devops\_github\_runners2.png)

* github Actions는 크게 세 가지 os로 구성할 수 있음.
  * MacOS
  * Linux
  * Windows
* 카카오 엔터프라이즈에서는 클라우드 환경에서 provisioning이 번거로운 MacOS와 Windows를 제외하고 Linux를 이용해 github action runner를 구성함.

#### Gihub Actions를 구성하는 방법 - Linux

* Linux로 Github Actions를 구성하는 방법에는 2가지가 있음
  * Virtual Machine
  * Docker\

* 2가지 방법 중 하나를 고르기 위해 고려한 사항들
  * Github Action Runner가 실행되어 종료될 때까지 각 Workflow마다 동일한 환경 보장해야 함.
    * 즉, 이전 빌드가 다음 빌드에 어떠한 영향을 주어서는 안됨.
    * 한번 빌드가 수행된 github action runner는 종료되고, 새로운 github action runner가 제공되어야 함.
  * 빌드 시 리소스가 폭증하는 것을 제어할 수 있어야 함.
    * EX)빌드 실행시 github Action runner에 설정해놓은 메모리를 초과할 경우, OutOfMemory 오류가 발생할 수 있음.
  *   필요에 따라 github Action runner 스케일링이 가능해야 함.

      * 빌드는 매번 똑같은 크기로 빌드되는 것이 아니고, 서비스 오픈 or 업그레이드 시점에 빌드가 몰릴 수 있기 때문에 유연한 스케일링이 필요함.

      \-> Docker를 이용해 github action runner를 구축 및 운영하기로 함

      \-> docker로 구성된 github action runner를 좀 더 쉽게 오케스트레이션하기 위해 쿠버네티스 엔진을 사용하기로 함.

### Docker in Docker vs Docker Out Of Docker

* Docker로 구성된 github action runner에서 다시 Docker를 구동할 경우 구조적인 문제가 발생했음.

#### github action runner의 일반적인 Workflow가 할당되었을 때

*   Job에서 컨테이너를 실행하여 Workflow를 수행할 수도 있음. ![Github Actions를 구성하는 방법](../images/데브옵스/Github-Actions-Runner-빌드-실전-적용기/if\(kakao\)\_devops\_github\_runners4.png)

    * Job에서 컨테이너를 실행하는 Workflow 파일의 일부
    * `build-and-test`라는 Job에서 특정 `Image`를 컨테이너로 실행하거나 서비스에서 빌드시 필요한 DB를 컨테이너로 구동하여 함께 선언할 수 있음.

    ![Github Actions를 구성하는 방법](../images/데브옵스/Github-Actions-Runner-빌드-실전-적용기/if\(kakao\)\_devops\_github\_runners5.png)

    * `Itialize Containers` : Job이 시작될 때 자동으로 수행됨.
    * `Stop Containers` : Job이 끝날 때 자동으로 수행됨.\
      \-> 이 Step들이 실행 중일 때는 사용자 or 관리자가 제어할 수 없음.

#### Docker in Docker

![Docker in Docker](../images/데브옵스/Github-Actions-Runner-빌드-실전-적용기/if\(kakao\)\_devops\_github\_runners6.png)

* docker Host에서 Runner Container를 생성하고 github action runner와 docker를 통해 두 개의 Job Container를 생성함.
* docker in docker는 docker로 구성된 runner container 안에서 별도의 docker를 실행하여 Job container 등을 구동시키는 방식

\


*   Runner Container 안에서 Job Container가 어떻게 동작할까? ![Docker in Docker](../images/데브옵스/Github-Actions-Runner-빌드-실전-적용기/if\(kakao\)\_devops\_github\_runners7.png)

    * Runner Container 안에서 일반적인 Workflow가 실행되는 과정을 나타낸 그림
    * Workflow는 Mount된 host Runner 작업 디렉토리에 접근할 수 있기 때문에 정상 수행됨.

    \


    ![Docker in Docker](../images/데브옵스/Github-Actions-Runner-빌드-실전-적용기/if\(kakao\)\_devops\_github\_runners8.png)

    * WorkFlow에서 Job Container 두 개를 생성한 모습
    * Runner Container는 Job Container를 구동시킬 수 있지만, Host 영역의 작업 디렉토리는 접근할 수 없기 때문에 해당 WorkFlow는 실패함.

    \


    ![Docker in Docker](../images/데브옵스/Github-Actions-Runner-빌드-실전-적용기/if\(kakao\)\_devops\_github\_runners9.png)

    * Github에서 제공하는 github runner project에서 container operation provider 코드
    * github Action runner는 이미 docker로 구동된 상태에서 그 안에 container가 실행되고 있을 경우 예외를 던짐.
    * 즉, github Action runner에서는 Docker in docker를 지원하지 않음.

#### Docker Out Of Docker

![Docker Out Of Docker](../images/데브옵스/Github-Actions-Runner-빌드-실전-적용기/if\(kakao\)\_devops\_github\_runners10.png)

* Runner Container와 같은 레벨로 Job Container를 구동하는 방식
* Runner Container에 Workflow가 담기게 되면 runner container docker와 host docker가 통신하여 host runner의 마운트 정보를 알게되고 최종적으로 host docker에서 job Container를 생성 및 실행함.

### exit code 137

* runner container에 workflow가 할당되어 작업이 수행되고 있는 상황에서 자체적인 오류 등의 이유로 runner container가 강제적으로 종료되었을 때 나타나는 오류코드

![Docker Out Of Docker](../images/데브옵스/Github-Actions-Runner-빌드-실전-적용기/if\(kakao\)\_devops\_github\_runners11.png)

* Workflow를 통해 Job container 두 개가 실행되는 모습

![Docker Out Of Docker](../images/데브옵스/Github-Actions-Runner-빌드-실전-적용기/if\(kakao\)\_devops\_github\_runners12.png)

* Workflow에서 container가 실행될 때 `Initialize Container`를 수행하여 이전 작업들을 지우는 작업을 함.
* 이때, clean filter lable이 특정 값으로 설정되어 있음.

![Docker Out Of Docker](../images/데브옵스/Github-Actions-Runner-빌드-실전-적용기/if\(kakao\)\_devops\_github\_runners13.png)

* Action runner 초기화 코드
* 먼저 `Initialize` 함수가 호출됨.
* 이때 runner의 root directory를 가져오게 됨.

![Docker Out Of Docker](../images/데브옵스/Github-Actions-Runner-빌드-실전-적용기/if\(kakao\)\_devops\_github\_runners14.png)

* Runner의 각종 설정 정보를 확인하고 나면, docker command manager class의 initialize 함수에서 root directory 경로 앞의 6자리 문자열을 잘라 해쉬값을 생성함.
* 생성한 해쉬값을 docker instance lable 변수에 저장함.

![Docker Out Of Docker](../images/데브옵스/Github-Actions-Runner-빌드-실전-적용기/if\(kakao\)\_devops\_github\_runners15.png)

* 각각의 Job container는 초기에 세팅된 RUNNER\_BINDIR 경로가 모두 동일하기 때문에, 모든 job Container들은 동일한 docker instance lable을 가지게 됨.
* 때문에 Job container1에서 clean up을 수행하게 되면 같은 host에 올라와 있는 Job container2에 영향을 주게 되어 강제 종료됨.

#### 해결법

![Docker Out Of Docker](../images/데브옵스/Github-Actions-Runner-빌드-실전-적용기/if\(kakao\)\_devops\_github\_runners16.png)

* RUNNER\_BINDIR 변수 값을 유일한 값으로 생성되는 Pod 이름으로 변경함.
