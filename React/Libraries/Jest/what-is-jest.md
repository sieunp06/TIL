## Jest
`Jest`는 `React`에서 많이 사용되는 `Javascript 테스트 프레임워크`이다.

자바스크립트 프레임워크이므로 `React` 이외에도 Typescript, Node.Js, Angular, Vue에서도 사용 가능하다.

https://jestjs.io/

## Advances of Jest
### Zero-Setting
`Jest`는 제로 설정을 지향한다. 따로 사용자가 설정을 건드릴 필요가 없다.
이러한 특징은 `Jest`를 사용하여 테스트를 진행할 때 따로 신경 쓸 부분이 없어 테스트에만 집중 할 수 있다.

### Snapshot
`Jest`는 `Snapshot` 기능을 지원한다.

Snapshot 기능을 통해 렌더링 된 컴포넌트의 변경 여부를 검사할 수 있다.

- ❗`Jest`의 `Snapshot` 기능을 이용하여 진행하는 테스트를 일컷는 Snapshot Testing은 단점 또한 존재한다.

### Mocking
`Jest`는 `Mocking`이 가능하다.
Mocking을 사용하여 테스트 범위를 벗어나는 객체들을 모의 객체로 만듦으로써 더욱 쉽게 테스트할 수 있다.

### Separated Test Code
`Jest` 테스트 코드는 완전히 분리되어 있으며, `Jest` 테스트 코드를 동시 실행할 수 있다.
테스트 코드를 동시에 실행할 수 있기 떄문에 더욱 빠른 성능을 제공할 수 있다.

### Simple API
`Jest`는 쉽고 간결한 API로 이루어져 있다.

## How to Install Jest
```
npm install --save-dev jest
```

- ❗ `package.json`의 [Jest 옵션]()을 변경할 수 있다.

## How to Run Jest
```
npm run test
```
```
npx jest --coverage
```
`--coverage` 옵션을 통해 coverage도 쉽게 검사할 수 있다.