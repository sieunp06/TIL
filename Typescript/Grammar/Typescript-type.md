### 목차

---
## 기본 타입
`Typescript`에는 다음과 같은 기본 타입들이 존재한다.
- string
- number
- boolean
- null
- undefined
- symbol
- bigint
- object

이때, `object` 타입에는 함수와 배열도 객체기 때문에 해당 타입에 포함된다.

### 문자열 String
`Typescript`에서 문자열은 `string`으로 표현한다.

큰따옴표(`"`)와 작은 따옴표(`'`)를 문자열 데이터를 감싸는데 사용한다.

```ts
let colot: string = "blue";
color = 'red';
```

아래와 같이 템플릿 문자열을 사용할 수 있다.

```ts
let fullName: string = `Bob Bobbington`;
let age: number = 37;
let sentence: string = `Hello, my name is ${ fullName }.
I'll be ${ age + 1 } years old next month.`;
```

이때 `sentence`는 아래 표현도 위와 같은 결과를 가진다.

```ts
let sentence: string = "Hello, my name is " + fullName + ".\n\n" +
    "I'll be " + (age + 1) + " years old next month.";
```

### 숫자 Number
`number`는 16진수와 10진수, 2진수, 8진수 리터럴을 모두 지원한다.
```ts
let demical: number = 6;
let hex: number = 0xf00d;
let binary: number = 0b1010;
let octal: number = 0o744;
```

## 리터럴 타입
리터럴 타입은 타입 자리에 리터럴 값을 사용한다.
```ts
const helloWorld = "Hello World";
```

위 예시는 `helloWorld`를 `const`로 선언했기 때문에 해당 변수는 변경되지 않는다.

때문에 `Typescript`는 해당 변수를 `string`으로 선언하지 않고 `Hello World`를 타입으로 선언한다.

```ts
let hiWorld = "Hi World";
```

만약 위처럼 변수를 `let`으로 선언했다면, 값이 변경될 수 있기 때문에 해당 변수는 `string`으로 선언된다.

## 배열(Array)의 타입
배열의 타입은 두 가지 방법으로 선언할 수 있다.
- 타입 뒤 `[]` 사용
- 제너럴 배열 타입 사용

<br>

```ts
const arr1: string[] = ['1', '2', '3'];
const arr2: Array<number> = [1, 2, 3];
```
이 때, 빈 배열은 `any[]` 배열로 추론된다.

```ts
const arr3 = [];
// const arr3: any[]
```

## 튜플(Tuple)의 타입
튜플은 요소의 타입과 개수가 고정된 배열을 표현할 수 있다.

```ts
let x: [string, number];

x = ["hello", 10];
```

위와 같이 `string`과 `number`로 각각의 요소의 타입을 정할 수 있다.

## 열거형(Enum)의 타입
```ts
enum Color {Red, Green, Blue}
let c: Color = Color.Green;
```

이때 각각의 index로 접근할 수 있다.

```ts
enum Color {Red, Green, Blue}
let colorName: string = Color[2];   // Blue
```

첫번째 값의 index는 `0`이며, 아래 방법으로 시작 index를 변경할 수 있다.

```ts
enum Color {Red = 1, Green, Blue}
let colorName: string = Color[2];   // Green
```

## Any
`Any`는 타입 검사를 하지 않고, 그 값을 통과시키기를 원할 때 사용한다.

```ts
let notSure: any = 4;
notSure = "maybe a string instead";
notSure = false;
```

위 예시는 오류가 발생하지 않는다.

## Void
`Void`는 `Any`의 반대점에 존재하는 타입으로, 어떤 타입도 존재하지 않음을 나타낸다.

```ts
function warnUser(): void {
    console.log("This is my warning message.");
}
```

위 예시처럼 함수의 반환 값이 없을 때, 반환 타입을 작성하기 위해 사용한다.

## Never
`Never`은 절대 발생할 수 없는 타입을 의미하며, 항상 오류를 발생시키거나 절대 반환하지 않는 타입의 반환 타입으로 사용된다.

```ts
function error(message: string): never {
    throw new Error(message);
}
```