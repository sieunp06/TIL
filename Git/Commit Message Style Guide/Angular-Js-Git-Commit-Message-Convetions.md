# Angular.Js Git Commit Message Conventions
### 목차
1. [Commit Message 형식](#commit-message-형식)
    - [\<type>](#type)

---
## Commit Message 형식
`Angular.Js Commit Message Convetions`는 아래의 구성을 가지고 있다.

```
<type>(<scope>): <subject>
<BLANK LINE>
<body>
<BLANK LINE>
<footer>
```

각 줄은 가독성을 위해 100자를 넘지 않아야 한다.

### \<type>
`<type>`에는 이 커밋을 통해 어떤 변화가 있는지 한 눈에 알 수 있도록 해당 커밋의 타입을 앞머리에 붙인다.

- feat: feature
- fix: bug fix
- docs: docummentation
- style: formatting, missing semi colons
- refactor
- test: when adding missing tests
- chore: maintain

### \<scope>
`<scope>`에는 커밋 변화가 일어난 곳을 작성한다.

### \<subject>
`<subject>`에는 변경 사항에 대한 간단한 설명을 작성하며, 아래 세 가지 규칙을 지켜야 한다.

- 현재 시제, 명령어로 작성한다.
- 첫 번째 글자를 대문자로 작성하지 않는다.
- 마지막에 점(.)을 붙이지 않는다.

### \<body>
`<body>`에는 변경에 대한 동기와 전과 비교하여 어떤 것이 달라졌는지에 대해 작성한다.

이때, `<subject>`와 동일하게 현재 시제, 명령어로 작성한다.

### \<footer>
#### breaking changes
모든 변경점들을 변경점, 변경 사유, migration 지시와 함께 `footer`에 작성한다.

#### referencing issues
해결 된 이슈는 `footer`의 독립된 줄에 Closes라는 키워드와 함께 작성한다.

```
Closes #234
```

해결 된 이슈가 여러 개라면 이런 식으로 작성해도 된다.

```
Closes #234, #235, #236
```

> + Angular 9에서는 Closes 대신 Fixes를 사용하기도 한다.

---
## references
- [https://gist.github.com/stephenparish/9941e89d80e2bc58a153](https://gist.github.com/stephenparish/9941e89d80e2bc58a153)