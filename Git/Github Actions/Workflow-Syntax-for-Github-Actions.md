### 목차
- [Workflow를 위한 YAML 문법](#workflow를-위한-yaml-문법)
- [name](#name)
- [run-name](#run-name)
  - [run-name의 예시](#run-name의-예시)
- [on](#on)
  - [activity types](#activity-types)
  - [filter](#filter)
  - [여러 이벤트에서 activity types와 filters 사용하기](#여러-이벤트에서-activity-types와-filters-사용하기)
- [on 자세히 알아보기](#on-자세히-알아보기)
  - [on.\<event\_name\>.types](#onevent_nametypes)
  - [on.\<pull\_request|pull\_request\_target\>.\<branches|branches-ignore\>](#onpull_requestpull_request_targetbranchesbranches-ignore)
    - [branch 포함하기: branches](#branch-포함하기-branches)
    - [branch 제외하기: branches-ignore](#branch-제외하기-branches-ignore)
    - [branch를 추가하고 제외하기](#branch를-추가하고-제외하기)
  - [on.push.\<branches|tags|branches-ignore|tag-ignore\>](#onpushbranchestagsbranches-ignoretag-ignore)
    - [브랜치와 태그 포함하기](#브랜치와-태그-포함하기)
    - [브랜치와 태그 제거하기](#브랜치와-태그-제거하기)
    - [브랜치와 태그 포함하고 제거하기](#브랜치와-태그-포함하고-제거하기)
  - [on.\<push|pull\_request|pull\_request\_target\>.\<path|paths\_ignore\>](#onpushpull_requestpull_request_targetpathpaths_ignore)
    - [경로 추가하기](#경로-추가하기)
    - [경로 제거하기](#경로-제거하기)
    - [경로 추가하거나 제거하기](#경로-추가하거나-제거하기)
  - [on.schedule](#onschedule)
- [permissions](#permissions)
- [jobs](#jobs)
  - [jobs.\<job\_id\>](#jobsjob_id)
  - [jobs.\<job\_id\>.name](#jobsjob_idname)
  - [jobs.\<job\_id\>.permissions](#jobsjob_idpermissions)
- [references](#references)

---
## Workflow를 위한 YAML 문법
Workflow는 `YAML` 문법을 사용하며, `.yml`이나 `.yaml` 확장자를 가지고 있어야 한다.

또한 `.github/workflows` 경로에 존재해야 한다.

## name
`name`은 workflow의 이름을 의미한다.

Github는 레포지토리의 Actions 탭에 Workflow의 `name`을 표시한다.

> 💡 만약 `name`을 생략한다면, Workflow 파일의 경로를 표시한다.

## run-name
`run-name`은 Workflow에서 생성되는 Workflow 실행을 위한 이름이다.

Github은 레포지토리의 Actions 탭의 Workflow 실행 리스트에 Workflow의 run name을 표시한다.

> 💡 만약 `run-name`을 생략하거나 whitespace라면, `run-name`은 Workflow 실행에 대한 특정 이벤트로 설정된다.

### run-name의 예시
```yaml
run-name: Deploy to ${{ inputs.deploy_target }} by @${{ github.actor }}
```

## on
`on`은 Workflow가 자동으로 실행될 경우를 정의한다.

<br>

이떄, 하나 이상의 `event`를 Workflow를 실행시키는 경우로 정의할 수 있다.

- Workflow가 존재하는 레포지토리의 아무 브랜치에 `push`가 발생했을 때, Workflow를 실행한다.
    ```yaml
    on: push
    ```
- 아무 브랜치에 `push`가 발생했을 경우이거나 누군가 레포지토리를 `fork`했을 때, Workflow를 실행한다.
    ```yaml
    on: [push, fork]
    ```
    만약 `push`와 `fork`가 동시에 발생하면, Workflow가 두 번 실행된다.

### activity types
몇몇 `Events`는 `activity types`을 가지고 있으며, Workflow가 실행되는 경우를 더욱 자세하게 설정할 수 있도록 한다.

<br>

만약 Workflow의 `Event`를 `label`로 설정한다면, 아마 `label`이 생성, 수정, 삭제될 경우 모두 Workflow가 실행된다.

`activity types`는 특정 Event 중에서도 특정 상황에서만 Workflow가 실행될 수 있도록 한다.
```yaml
on:
    label:
        types:
            - created
```
예를 들어, 위 예시에서는 `label` 이벤트 중 label이 `created` 되었을 때만 Workflow를 실행하도록 한다.

<br>

만약 여러 개의 `activity types`를 정의하고 싶다면, 다음과 같이 작성하면 된다.
```yaml
on:
    issues:
        types:
            - opened
            - labeled
```
이때 여러 개의 `Event`를 지정했을 때와 동일하게 두 `activity type`이 동시에 발생했을 경우, Workflow가 여러 번 실행된다.

즉, 위 예시에서 만약 두 개의 label을 가진 하나의 이슈이 오픈되었을 경우, Workflow가 세 번 실행된다.

### filter
Workflow의 실행을 특정 파일, 태그, 브랜치로 제한할 수도 있다.

```yaml
on:
    push:
        branches:
            - main
            - 'releases/**'
```

위 예시는 `push`가 발생했을 때 Workflow를 실행하는데, 그 중에서도 `main` 브랜치와 `release/**` 브랜치에 `push`가 발생했을 경우 Workflow를 실행한다.

### 여러 이벤트에서 activity types와 filters 사용하기
Workflow에서 여러 Event를 정의하고, 각각의 Event 안에 activity type과 filter를 정의했다면, 각각의 이벤트를 분리해야 한다.

모든 이벤트에 콜론(:)을 붙여야 하며, activity types나 filters가 없는 이벤트도 마찬가지이다.

```yaml
on:
    label:
        types:
            - created
    push:
        brances:
            - main
    page_build:
```

## on 자세히 알아보기
### on.<event_name>.types
`on.<event_name>.types`는 Workflow 실행을 야기시키는 이벤트의 `activity type`을 정의할 때 사용한다.

만약 굳이 `activity types`를 지정하지 않아도 된다면, 생략해도 된다.

```yaml
on: 
    label:
        types: [created, edited]
```

위 예시는 `label` 이벤트에서 `created`, `edited` 상황일 경우에만 Workflow를 실행한다.

### on.<pull_request|pull_request_target>.<branches|branches-ignore>
`pull_request`나 `pull_request_target` 이벤트를 사용할 때, 특정 브랜치에 pull request가 발생했을 때 Workflow를 실행시킬지 정할 수 있다.

브랜치를 포함하거나 제외시키려면 `branches` 필터나 `branches-ignore` 필터를 사용하면 된다.

이때, Workflow 내의 같은 이벤트에서 `branches`와 `branches-ignore` 필터를 한번에 사용할 수 없다.

또한, `*`, `**`, `+`, `?`, `!` 등의 전역 패턴을 사용할 수 있다.

#### branch 포함하기: branches
`branches` 필터와 일치하는 브랜치에 `pull_request`가 발생하면 Workflow가 실행된다.
```yaml
on:
    pull_request:
        branches:
            - main
            - 'mona/octocat'
            - 'release/**'
```

#### branch 제외하기: branches-ignore
`branches-ignore` 필터와 패턴이 일치하는 브랜치에 `pull-request`가 발생하면, Workflow가 발생하지 않는다.
```yaml
on:
    pull_request:
        branches-ignore:
            - 'mona/octocat'
            - 'release/**-alpha'
```

#### branch를 추가하고 제외하기
위에서 `branches` 필터와 `branches-ignore` 필터는 같은 이벤트에서 동시에 사용하면 안된다고 했지만, `!` 사용하면 두 필터를 한번에 사용하는 것과 같은 의미를 갖게 된다.

```yaml
on:
    pull_request:
        branches:
            - 'releases/**'
            - '!release/**-alpha'
```
`!`를 포함하고 있는 브랜치는 브랜치를 제외시킨다는 의미와 같다.

즉, `release/**-alpha` 패턴을 띄고 있는 브랜치들은 해당 브랜치에 `pull_request`가 발생해도 Workflow가 발생하지 않는다.

### on.push.<branches|tags|branches-ignore|tag-ignore>
특정 브랜치나 태그에 `push` 이벤트가 발생했을 때, Workflow를 실행할 수 있게 한다.

`branches`와 `branches-ignore` 필터를 한 이벤트에 동시에 사용할 수 없고, 이는 `tags`와 `tags-ignore`도 마찬가지이다.

#### 브랜치와 태그 포함하기
```yaml
on:
    push:
        branches:
            - main
            - 'mona/octocat'
            - 'releases/**'
        tags:
            - v2
            - v1.*
```

#### 브랜치와 태그 제거하기
```yaml
on:
    push:
        branches-ignore:
            -'mona/octocat'
            - 'release/**-alpha'
        - tags-ignore:
            - v2
            - v1.*
```

#### 브랜치와 태그 포함하고 제거하기
`branches`와 `branches-ignore` 그리고 `tags`와 `tags-ignore`을 한 이벤트에서 동시에 사용할 수 없지만, 다음과 같이 작성하면 된다.
```yaml
on:
    push:
        branches:
            - 'release/**'
            - '!release/**-alpha'
```

### on.<push|pull_request|pull_request_target>.<path|paths_ignore>
`push` 이벤트와 `pull_request` 이벤트를 사용할 때, 파일 경로가 변경되었을 때 Workflow를 실행한다.

`path` 필터와 `path-ignore` 필터는 Workflow를 실행시킬 대상이 되는 경로를 추가하거나 제거한다.

이때 `path` 필터와 `path-ignore` 필터는 한 이벤트 내에서 함께 사용할 수 없다.

#### 경로 추가하기
```yaml
on:
    push:
        paths:
            - '**.js'
```

#### 경로 제거하기
```yaml
on:
    push:
        paths-ignore:
            - 'docs/**'
```

#### 경로 추가하거나 제거하기
한 이벤트 내에서 `path` 필터와 `path-ignore` 필터를 같이 사용할 수 없지만, 다음과 같이 사용할 수 있다.
```yaml
on:
    push:
        paths-ignore:
            - 'sub-project/**'
            - '!sub-project/docs/**'
```

### on.schedule

## permissions


## jobs
Workflow의 실행은 병렬적으로 실행되는 하나 이상의 `job`으로 이루어진다.

### jobs.<job_id>
`jobs.<job_id>`는 `job`에게 고유 식별자를 부여한다.

`job_id`는 String 값이며, `job_id`가 key인 Map이다.

```yaml
jobs:
    my_first_job:
        name: My First Job
    my_second_job:
        name: My Second Job
```

- `job_id`는 문자 or _로 시작해야 하며, 영숫자 문자 또는 - 또는 _만 포함해야 한다.

### jobs.<job_id>.name
`jobs.<job_id>.name`은 해당 job의 이름을 나타낸다.

이 값은 Github의 UI에 표시된다.

### jobs.<job_id>.permissions


---
## references
- [https://docs.github.com/ko/actions/using-workflows/workflow-syntax-for-github-actions](https://docs.github.com/ko/actions/using-workflows/workflow-syntax-for-github-actions)