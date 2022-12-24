> if(kakao)dev 2022의 백엔드 세션의 [API 테스트 주도 개발의 시작](https://if.kakao.com/2022/session/24) 영상을 보고 정리한 글입니다.
# API 테스트 주도 개발의 시작
## API란?
- Aplication Programming Interface의 약자
- 대부분의 엔터프라이즈 애플리케이션은 그 규모에 따라 다양한 요구사항을 수용하기 위해 다른 시스템과 통신해야 함.
- 이런 시스템 간의 통신을 위해 따라야 하는 규칙을 정의함.
## API 개발자는?
- 다른 애플리케이션이 프로그래밍으로 통신할 수 있도록 API를 개발함.
- 여러 아키텍쳐를 사용하여 API를 설계 및 개발할 수 있음

### RESTful API
![if(kakao) back api_testing1](../images/if(kakao)_back_api_testing1.png)
- 시스템 간 인터넷을 통해 정보를 안전하게 교환할 수 있도록 원칙을 정의함.
- http 프로토콜의 메소드와 상태코드를 통해 일관된 방식으로 요청과 응답을 주고 받을 수 있고, 다른 요청과 분리되어서 응답을 주기 때문에 상태가 없음.
- 응답은 json 뿐만 아니라 클라이언트 요구에 따라 다양한 포맷으로 줄 수도 있음.
- 백엔드 측면에서 RESTful API로 구현된 서버들을 묶어 계층을 만들 수 있고, 이를 통해 복잡한 기능을 클라이언트 영역없이 제공할 수 있음.
- 엔터프라이즈 애플리케이션 구축 시 꼭 필요한 요소가 됨.

## API 테스트
- API를 테스트하는 것!
- 시스템이 제공하는 API가 기능, 성능, 보안, 신뢰에 대한 기대치를 충족하는지 확인하는 작업


### UI test VS API test
![if(kakao) back api_testing2](../images/if(kakao)_back_api_testing2.png)
- UI를 통해 Business Layer를 테스트하는 것보다 API를 통해 Business Layer를 테스트 하는 것이 더 빠르고 정확함.
- Presentation Layer에서 UI testing을 진행하고, Business Layer에서는 API testing을 진행하는 것이 더 효율적임.

### API test VS Unit test
![if(kakao) back api_testing3](../images/if(kakao)_back_api_testing3.png)
- API test와 Unit test 모두 타깃의 기본 기능을 테스트 하지만, API test의 경우 기능 뿐만 아니라 시스템 간의 통합, 보안, 성능 등 추가적인 이슈들을 쉽게 확인할 수 있음.

### 모든 테스트들은 상호보완적이다.
![if(kakao) back api_testing4](../images/if(kakao)_back_api_testing4.png)
- 테스트라는 큰 범주에서 동일한 목표를 가지고 있음.

## API Test Automation
- 모든 테스트는 비용을 아끼기 위해 자동화하는 것이 필수적임.
### 테스트를 자동화 한다는 것은
![if(kakao) back api_testing5](../images/if(kakao)_back_api_testing5.png)
- 사람의 노력 없이 테스트를 계속 수행하게 만드는 것.
- CI/CD 단계에서 빌드 후 API 테스트를 실행시킴으로서 간단하게 자동화할 수 있음.
### API 테스트를 CI/CD 단계에 통합시키는 방법
- 개발 단계에서 API 테스트를 Regression Test(회귀 테스트)로 만든다.
    - Regression Test란?
        - 최근의 변경사항이 원래 동작하던 기능에 영향을 주지 않음을 확인하기 위한 테스트
- Regression Test로 만든 API test를 애플리케이션이 빌드된 후에 수행하도록 하고, 테스트의 성공 여부로 배포를 함.
- 배포된 이후 스케쥴링을 위해 테스트를 지속적으로 수행함으로써 API report를 받을 수 있음.

## TestLAB
카카오에서 개발 중인 API testing 기능 
### 현재의 TestLAB
- RESTful API test를 쉽게 작성할 수 있음.
- 작성한 test 하나로 기능, 성능 테스트를 수행할 수 있음.
- 엔진을 자체 개발했기 때문에 기능 테스트와 성능 테스트를 동일한 테스트 명세로 테스트할 수 있도록 구현할 수 있었음.
- 카카오 내에서 발생한 요구사항을 유연하게 대응할 수 있는 기반 마련할 수 있었음.
- 여러 테스트케이스의 Assertion과 Script를 지원함.
    - Assertion 기능으로 Scripting을 하지 않고 테스트를 작성할 수 있음.
    - 제공하는 Assertion기능이 부족할 때, Script를 통해 더 자세한 테스트를 작성할 수 있음.
- CI/CD에 통합될 수 있도록 Trigger API를 지원하고, Scheduling을 통하여 API 모니터링 기능을 제공함.
- 기존 Postman을 사용 중이라면, export와 postman collection 파일을 testLAB에 가져오기 기능으로 재활용할 수 있음.

### Test Suite
- Test Suite의 특징
    - 테스트 시나리오와 시나리오가 돌아가는 환경을 속성으로 가지고 있음.
    - 이 속성으로 기능과 성능 테스트를 수행함.
    - 또한 기능 테스트를 스케쥴링하거나 CI/CD에 통합할 수 있음.

## API Test Driven
![if(kakao) back api_testing7](../images/if(kakao)_back_api_testing7.png)
- TestLAB의 개발 방향성.
- API test 자동화를 넘어 애플리케이션 개발을 동료들과 함께 API test로 시작하는 프로세스
- 간단하게 애플리케이션 개발 주기에 맞게 API 테스트를 작성하고 성공시키며 CI/CD에서 실행시키면 됨.
- 클라이언트가 필요로 하는 기능과 서버가 제공하는 기능이 모두 API test로 작성되서 추가적인 설명이나 정의서가 필요 없음.
    - 커뮤니케이션 및 개발 비용을 줄일 수 있음.
- 기획 단계에서부터 테스트가 작성되기 때문에 생산성이 향상되고, 배포 이후 운영 단계에서 스케쥴링을 통해 자동적으로 실행되기 때문에 프로덕션 품질을 보장할 수 있음.
### API Test Driven 개발 주기
![if(kakao) back api_testing6](../images/if(kakao)_back_api_testing6.png)
- 기획
    - 최소 클라이언트와 서버로 구성되어 개발됨.
    - 개발팀은 클라이언트와 서버의 통신을 실패하는 API test 작성을 통해 설계함.
    - 애플리케이션 요구사항에 맞게 메소드와 URL 패스 요청 바디 등을 테스트 케이스 입력으로 넣고, 예상되는 상태 코드와 응답 바디 등을 검증하도록 함.
    - 비교적 간단한 API 테스트인 CRUD부터 복잡한 시나리오까지 API 테스트를 작성함.
    - 요구사항의 스팩을 정의하는 것과 같음. 문서 대체 가능
- 구현
    - 기획 단계에서 작성된 API test를 통해 협업을 진행함.
    - API test 스펙으로 개발 주기에 맞춰 mock 서버를 실행시킴
    - 클라이언트 개발자는 mock 서버를 통해 클라이언트 개발을 시행하고, API 개발자는 개발 진행 중에 수시로 테스트 통과 여부를 확인함.
    - 개발 환경에 배포된 서버가 모든 API 테스트를 성공할 수 있도록 수정 보완함.
- 빌드, 테스트, 배포
    - API 테스트를 자동화함.
    - CI/CD에서 기능 테스트를 통해 배포 진행 여부를 판단할 수 있음.
    - 추가로 기능 테스트 뿐만 아니라 성능, 보안 테스트 등을 배포 전에 진행함.
- 운영
    - API 테스트를 스케쥴링 설정하여 모니터링 함.
    - 올바른 상태코드, 동작 시간이 지연되고 있는지 등을 확인함.
    - 실제 서비스를 운영하는 단계에서 모니터링의 용도로 사용할 수 있음.

## TestLAB RoadMap
- TestSuite 생성 자동화
- 기능/성능 테스트외 다양한 테스트(보안, 화질 테스트 등) 타입 지원
- Test 자동화/운영 강화
    - CI/CD 통합 및 모니터링 강화를 통한 사용성 강화
- Mock Server와 Document 지원
- Jira, Github 등의 협업 툴과의 연동 지원