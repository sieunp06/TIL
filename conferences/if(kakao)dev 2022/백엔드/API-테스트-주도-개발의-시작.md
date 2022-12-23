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
- 