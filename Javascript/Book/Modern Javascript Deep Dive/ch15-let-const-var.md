### 목차
- [var 키워드로 선언한 변수의 문제점](#var-키워드로-선언한-변수의-문제점)
  - [변수 중복 선언 허용](#변수-중복-선언-허용)
  - [함수 레벨 스코프](#함수-레벨-스코프)
  - [변수 호이스팅](#변수-호이스팅)
- [let 키워드](#let-키워드)
  - [변수 중복 선언 금지](#변수-중복-선언-금지)
  - [블록 레벨 스코프](#블록-레벨-스코프)
  - [변수 호이스팅](#변수-호이스팅-1)
- [const 키워드](#const-키워드)
  - [선언과 초기화](#선언과-초기화)
  - [재할당 금지](#재할당-금지)
  - [const 키워드와 객체](#const-키워드와-객체)
- [var vs let vs const](#var-vs-let-vs-const)
---
> 자바스크립트 Deep Dive

<br>

# 15장 let, const 키워드와 블록 레벨 스코프
## var 키워드로 선언한 변수의 문제점
ES5까지 변수를 선언할 수 있는 방법은 `var` 키워드를 사용하는 것이다.

`var` 키워드로 선언된 변수는 다음과 같은 특징이 있다.

<br>

### 변수 중복 선언 허용
다음과 같이 `var` 키워드로 선언한 변수는 중복 선언이 가능하다.

```js
var x = 1;
var y = 1;

// var 키워드로 선언돤 변수는 같은 스코프 내에서 중복 선언을 허용한다.
// 초기화문이 있는 변수 선언문은 자바스크립트 엔진에 의해 var 키워드가 없는 것처럼 동작한다.
var x = 100;

// 초기화문이 없는 변수 선언문은 무시된다.
var y;

console.log(x); // 100
console.log(y); // 1
```

위 예시에서 `x`와 `y`는 중복 선언되었다.

이때 `var` 키워드로 선언한 변수를 중복 선언하면 초기화문 유무에 따라 다르게 동작한다.

<br>

만약 동일한 이름의 변수가 이미 선언되어 있는 것을 모르고 변수를 중복 선언하고 값까지 할당했다면 먼저 선언된 변수 값이 변경되는 부작용이 발생한다.

### 함수 레벨 스코프
`var` 키워드로 선언한 변수는 함수의 코드 블록만을 지역 스코프로 인정한다.

때문에 함수 외부에서 `var` 키워드로 선언한 변수는 코드 블록 내에서 선언해도 모두 전역 변수가 된다.

```js
var x = 1;

if (true) {
    var x = 10;
}

console.log(x);
```

위 예시에서 `x`는 전역변수이고, `var` 키워드로 선언했기 때문에 if 문 안의 `x`는 전역 변수 `x`와 중복된다.

때문에 의도치 않게 변수값이 변경된다.

<br>

if 문뿐만 아니라 for 문에서도 전역 변수가 된다.

```js
var i = 10;

for (var i = 0; i < 5; i++) {
    console.log(i); // 0 1 2 3 4
}

console.log(i); // 5
```

함수 레벨 스코프는 전역 변수를 남발할 가능성을 높인다.

이로 인해 의도치 않게 전역 변수가 중복 선언되는 경우가 발생한다.

### 변수 호이스팅
`var` 키워드로 변수를 선언하면 변수 호이스팅에 의해 변수 선언문이 스코프의 선두로 끌어 올려진 것처럼 동작한다.

즉, 변수 호이스팅에 의해 var 키워드로 선언한 변수는 변수 선언문 이전에 참조할 수 있다.

단, 할당문 이전에 변수를 참조하면 언제나 `undefined`를 반환한다.

```js
console.log(foo);   // undefined

foo = 123;

console.log(foo);   // 123

var foo;
```

이처럼 변수 선언문 이전에 변수를 참조하는 것은 변수 호이스팅에 의해 에러를 발생시키지 않지만 흐름상 맞지 않고 가독성을 떨어뜨리고 오류를 발생시킬 여지를 남긴다.

## let 키워드
`var` 키워드의 단점을 보완하기 위해 ES6에서 `let`과 `const` 키워드를 도입했다.

### 변수 중복 선언 금지
`var` 키워드는 변수를 중복 선언해도 아무 문제가 발생하지 않았다.

하지만 `let` 키워드로 이름이 같은 변수를 중복 선언하면 문법 에러가 발생한다.

```js
let foo = 123;
let bar = 456;  // Syntax: Identifier 'bar' has already been declared
```

### 블록 레벨 스코프
`var` 키워드는 함수 레벨 스코프를 따른다.

하지만 `let` 키워드는 모든 코드 블럭을 지역 스코프로 인정하는 블록 레벨 스코프를 따른다.

<br>

```js
let foo = 1;    // 전역 변수

{
    let foo = 2;    // 지역 변수
    let bar = 3;    // 지역 변수
}

console.log(foo);   // 1
console.log(bar);   // ReferenceError: bar is not defined
```

위 예시처럼 코드 블럭 내에서 선언된 변수들은 전역 변수로 선언한 변수들에 영향을 주지 않는다.

때문에 전역에서는 `bar` 변수를 참조할 수 없다.

### 변수 호이스팅
`var` 키워드로 선언한 변수와 달리 `let` 키워드로 선언한 변수는 변수 호이스팅이 발생하지 않는 것처럼 동작한다.

<br>

```js
console.log(foo);   // ReferenceError: foo is not defined
let foo;
```

위 예시를 보면 `undefined`가 아닌 오류가 발생한다.

<br>

`var` 키워드는 런타임 이전에 자바스크립트 엔진에 의해 암묵적으로 **선언 단계**와 **초기화 단계**가 한번에 진행된다.

즉, 선언 단계에서 스코프(실행 컨텍스트의 렉시컬 환경)에 변수 식별자를 등록해 자바스크립트 엔진에 변수의 존재를 알린다.

이후 즉시 초기화 단계에서 undefined로 변수를 초기화한다.

따라서 변수 선언문 이전에 변수에 접근해도 스코프에 변수가 존재하기 때문에 에러가 발생하지 않는다.

<br>

하지만 `let` 키워드로 선언한 변수는 **선언 단계**와 **초기화 단계**가 분리되어 진행된다.

즉, 런타임 이전에 자바스크립트 엔진에 의해 암묵적으로 선언 단계가 먼저 실행되지만 초기화 단계에는 변수 선언문에 도달했을 때 실행된다.

때문에 초기화 단계가 실행되기 이전에 변수에 접근하려고 하면 참조 에러가 발생한다.

<br>

이때 스코프의 시작 지점부터 초기화 시작 지점까지 변수를 참조할 수 없는 구간을 **일시적 사각지대**라 부른다.

## const 키워드
`const` 키워드는 상수를 선언하기 위해 사용한다.

하지만 반드시 상수만을 위해 사용하지는 않는다.

### 선언과 초기화
`const` 키워드는 선언과 동시에 초기화해야 한다.

```js
const foo = 1;
```

그렇지 않으면 문법 에러가 발생한다.

```js
const foo;  // SyntaxError: Missing initializer in const declaration
```

`let` 키워드와 마찬가지로 블록 레벨 스코프를 가지고, 변수 호이스팅이 발생하지 않는 것처럼 동작한다.

### 재할당 금지
`var` 또는 `let` 키워드로 선언한 변수는 재할당이 자유로우나 `const` 키워드로 선언한 변수는 재할당이 금지된다.

```js
const foo = 1;
foo = 2;        // TypeError: Assignment to constant variable.
```

### const 키워드와 객체
`const` 키워드로 선언된 변수에 원시 값을 할당한 경우 값을 변경할 수 없다.

하지만 `const` 키워드로 선언된 변수에 객체를 할당한 경우 값을 변경할 수 있다.

<br>

그 이유는 원시 값은 변경 불가능한 값이기 때문에 재할당 없이 변경할 수 있는 방법이 없지만, 객체는 재할당 없이도 직접 변경이 가능하기 때문이다.

## var vs let vs const
변수 선언에는 기본적으로 `const`를 사용하고, 재할당이 필요한 경우 `let`을 사용하는 것이 좋다.

<br>

var와 let, const는 다음과 같이 사용하는 것을 권장한다.

- ES6를 사용한다면 var 키워드를 사용하지 않는다.
- 재할당이 필요한 경우에 한정해 let을 사용한다. 이때 변수의 스코프는 최대한 좁게 만든다.
- 변경이 발생하지 않고 읽기 전용으로 사용하는 원시 값과 객체에는 const 키워드를 사용한다. const 키워드는 재할당을 금지하므로 var, let 키워드보다 안전하다.