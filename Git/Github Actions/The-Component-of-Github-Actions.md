## Workflows

`Workflow`는 하나 이상의 작업을 실행할 구성 가능한 자동화 프로세스이다.

`.github/workflows` 경로에 `YAML` 파일로 정의되어야 하며, 한 레포지토리에 여러 Workflow를 가질 수 있다.

- Workflow에 다른 Workflow를 재사용할 수 있다.

## Events

`Events`는 Workflow의 실행을 야기시키는 특별한 상황을 말한다.

- ex) pull request, push

이떄 이러한 특정한 상황 말고도, `schedule` 즉, 특정한 시간 등도 Event에 해당한다.

## Jobs

`Jobs`는 같은 `runner`에 실행되고 있는 workflow의 단계의 집합이다.

각각의 단계는 순서대로 실행되며 서로에게 종속적이다.

서로 다른 `Job`은 서로 종속적이지 않고, 병렬로 실행된다.

## Actions

`Action`은 복잡하지만 자주 사용되는 반복적인 작업을 수행하기 위한 custom application이다.

- 직접 작성할 수도 있고, Github Marketplace에서 다른 사용자가 작성한 것을 사용할 수 있다.

## Runners

`Runners`는 Workflow가 트리거되었을 때, 이를 실행하는 서버이다.

각각의 runner는 하나의 job만 실행할 수 있다.

Github에서는 다음 OS를 지원한다.

- Ubuntu Linux
- Microsoft Windows
- macOS

각 Workflow는 새로 프로비저닝된 가상 시스템에서 실행된다.
---
## references
- [https://docs.github.com/ko/actions/learn-github-actions/understanding-github-actions](https://docs.github.com/ko/actions/learn-github-actions/understanding-github-actions)