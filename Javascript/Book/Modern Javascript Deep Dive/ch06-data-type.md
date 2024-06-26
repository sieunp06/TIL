### 목차
- [숫자 타입](#숫자-타입)
    - [📌 모든 수는 실수로 처리한다.](#-모든-수는-실수로-처리한다)
    - [📌 Infinity, -Infinity, NaN](#-infinity--infinity-nan)
- [문자열 타입](#문자열-타입)
- [템플릿 리터럴](#템플릿-리터럴)
- [멀티라인 문자열](#멀티라인-문자열)
- [표현식 삽입](#표현식-삽입)
- [불리언 리터럴](#불리언-리터럴)
- [undefined 타입](#undefined-타입)
- [null 타입](#null-타입)
- [심벌 타입](#심벌-타입)

---
> 자바스크립트 Deep Dive

<br>

# 06장 데이터 타입
> 💡 **데이터 타입**<br>
> 값의 종류를 말한다.

자바스크립트의 모든 값은 데이터 타입을 갖고 있으며, 총 7개의 데이터 타입을 제공한다.
- 원시타입
  - 숫자 타입
  - 문자열 타입
  - 불리언 타입
  - undefined 타입
  - null 타입
  - 심벌 타입
- 객체 타입

## 숫자 타입
int, long, float, double 등 숫자 타입을 제공하는 C와 Java와 달리 자바스크립트는 하나의 숫자 타입만을 제공한다.

```js
var integer = 10;   // 정수
var double = 10.12; // 실수
var negative = -20; // 음의 정수
```

#### 📌 모든 수는 실수로 처리한다.
ECMAScript 사양에 따르면 숫자 타입의 값은 배정밀도 64비트 부동소수점 형식을 따른다.

즉 모든 수를 실수로 처리하며, 정수로만 표현하기 위한 데이터 타입이 별도로 존재하지 않는다.

```js
console.log(1 === 1.0); // true
console.log(4 / 2);     // 2
console.log(3 / 2);     // 1.5
```

모든 수를 실수로 처리하기 때문에 위와 같이 정수로 표시되는 수끼리 나누더라도 실수가 나올 수 있다.

#### 📌 Infinity, -Infinity, NaN
자바스크립트의 숫자 타입은 세 가지 특별한 값을 표현할 수 있다.
- **Infinity**: 양의 무한대
- **-Infinity**: 음의 무한대
- **NaN**: 산술 연산 불가(not a number)

```js
console.log(10 / 0);    // Infinity
console.log(10 / -0);   // -Infinity
console.log(1 * 'String');  // NaN
```

이때 자바스크립트는 대소문자를 구별하기 때문에 NaN을 nan, NAN, Nan 등으로 표현하면 에러가 발생한다.

자바스크립트 엔진은 NAN, Nan, nan을 값이 아닌 식별자로 인식하기 때문이다.

```js
var x = nan;    // ReferenceError: nan is not defined
```

위 처럼 참조오류가 발생하게 된다.

## 문자열 타입
문자열 타입은 텍스트 데이터를 나타내는 데 사용한다.

0개 이상의 16비트 유니코드 문자(UTF-16)의 집합으로 전 세계 대부분의 문자를 표현할 수 있다.

<br>

다음과 같은 기호로 텍스트를 감싸는 방법으로 문자열 타입을 나타낼 수 있다.
- 작은 따옴표('')
- 큰 따옴표("")
- 백틱(``)

이중 가장 일반적인 표기법은 **작은 따옴표**를 사용하는 것이다.

```js
var string;
string = '문자열';  // 작은 따옴표
string = "문자열";  // 큰 따옴표
string = `문자열`;  // 백틱(ES6)
```

<br>

이때 다음과 같이 작은 따옴표 내의 큰 따옴표와 큰 따옴표 내의 작은 따옴표는 문자열로 인식된다.

```js
string = '작은 따옴표로 감싼 문자열 내의 "큰 따옴표"는 문자열로 인식된다.';
string = "큰 따옴표로 감싼 문자열 내의 '작은 따옴표'는 문자열로 인식된다.";
```

<br>

> 🤔 **자바스크립트의 문자열은 원시 타입이며, 변경 불가능한 값이다.**<br>
> +) 자세한 내용은 [11.1.2절 문자열과 불변성]()을 확인하면 된다.

## 템플릿 리터럴
템플릿 리터럴은 ES6부터 도입된 새로운 문자열 표기법이다.

<br>

템플릿 리터럴은 다음 문자열 처리 기능을 제공한다.
- 멀티라인 문자열
- 표현식 삽입
- 태그드 템플릿

이때 템플릿 리터럴은 런타임에 일반 문자열로 변환되어 처리된다.

또한 백틱(``)을 사용하여 표현한다.

```js
var template = `Template literal`;
console.log(template);
```

### 멀티라인 문자열
일반 문자열 내에서는 줄바꿈(개행)이 허용되지 않는다.

```js
var str = 'Hello
world';
// SyntaxError: Invalid or unexpected token
```

따라서 일반 문자열 내에서 개행을 사용하려면 이스케이프 시퀸스를 사용해야 한다.

<br>

템플릿 리터럴을 사용하면 이스케이프 시퀀스를 사용하지 않아도 줄바꿈이 허용되며, 모든 공백도 있는 그대로 적용된다.

```js
var template = `<ul>
    <li><a href="#">Home</a></li>
</ul>`;

console.log(template);
```

### 표현식 삽입
문자열은 문자열 연산자 `+`를 사용해 연결할 수 있다.

```js
var first = 'Ung-mo';
var last = 'Lee';

// ES5: 문자열 연결
console.log('My name is ' + first + ' ' + last + '.');
```

템플릿 리터럴을 사용하면 문자열 연산자 `+`를 사용하는 것보다 더 가독성 있게 표현할 수 있다.

```js
var first = 'Ung-mo';
var last = 'Lee';

console.log(`My name is ${first} ${last}.`);
```

## 불리언 리터럴
불리언 타입은 값의 논리적 참, 거짓을 나타내는 true와 false뿐이다.

```js
var foo = true;
console.log(foo);   // true

foo = false;
console.log(foo)    // false
```

## undefined 타입
undefined는 자바스크립트 엔진이 변수를 초기화하는 데에 사용한다.

## null 타입
프로그래밍 언어에서 null은 변수에 값이 없다는 것을 의도적으로 명시할 때 사용한다.

변수에 null을 할당하는 것은 변수가 이전에 참조하던 값을 더 이상 참조하지 않겠다는 의미이다.

즉 이전에 할당되어 있는 값에 대한 참조를 명시적으로 제거하는 것을 의미한다.

```js
var foo = 'Lee';

// 이전 참조를 제거
foo = null;
```

## 심벌 타입
> 💡 **심벌**<br>
> ES6에서 추가된 7번째 타입으로, 변경 불가능한 원시 타입의 값이다.<br>
> 또한 다른 값과 중복되지 않는 유일무이한 값이다.

```js
var key = Symbol('key');
console.log(typeof key);    // symbol
```

위와 같이 심벌 타입은 `Symbol` 함수를 호출해 생성한다.

이때 생성된 심벌 값은 외부에 노출되지 않으며, 다른 값과 절대 중복되지 않는 유일무이한 값이다.

때문에 심벌 타입은 주로 이름이 충돌할 위험이 없는 객체의 유일한 프로퍼티 키를 만들기 위해 사용한다.

```js
// 객체 생성
var obj = {};
var key = Symbol('key');

// 이름이 충돌할 위험이 없는 유일무이한 값인 심벌을 프로퍼티 값으로 사용한다.
obj[key] = 'value';
console.log(obj[key]);  // value
```

> ⭐ 더 자세한 내용은 [33장 7번째 타입 Symbol]()을 확인하면 된다.

