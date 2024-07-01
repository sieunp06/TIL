### 목차
  - [자바스크립트 객체의 분류](#자바스크립트-객체의-분류)
  - [표준 빌트인 객체](#표준-빌트인-객체)
  - [원시값과 래퍼 객체](#원시값과-래퍼-객체)
  - [전역 객체](#전역-객체)
    - [전역 객체의 특징](#전역-객체의-특징)
    - [빌트인 전역 프로퍼티](#빌트인-전역-프로퍼티)
    - [빌트인 전역 함수](#빌트인-전역-함수)


---
> 모던 자바스크립트 Deep Dive

<br>

# 21장 빌트인 객체
## 자바스크립트 객체의 분류
자바스크립트 객체는 다음 세 가지 객체로 분류할 수 있다.

#### 📌 표준 빌트인 객체
> 💡 **표준 빌트인 객체**<br>
> ECMAScript 사양에 정의된 객체를 말하며, 애플리케이션 전역의 공통 기능을 제공한다.

표준 빌트인 객체는 `ECAMScript` 사양에 정의된 객체이므로 자바스크립트 실행 환경과 관계없이 언제나 사용할 수 있으며, 전역 객체의 프로퍼티로서 제공된다.

따라서 별도의 선언 없이 전역 변수처럼 언제나 참조할 수 있다.

#### 📌 호스트 객체
> 💡 **호스트 객체**<br>
> ECMAScript 사양에 정의되어 있지 않지만 자바스크립트 실행 환경에서 추가로 제공되는 객체를 말한다.

브라우저 환경에서는 DOM, BOM, Canvas, XMLHttpRequest, fetch 등과 같은 클라이언트 사이드 Web API를 호스트 객체로 제공한다.

또한 Node.js 환경에서는 Node.js 고유의 API를 호스트 객체로 제공한다.

#### 📌 사용자 정의 객체
> 💡 **사용자 정의 객체**<br>
> 표준 빌트인 객체와 호스트 객체처럼 기본 제공되는 객체가 아닌 사용자가 직접 정의한 객체를 말한다.

## 표준 빌트인 객체
자바스크립트는 Object, String, Number, Boolean, Symbol, Date, Math, RegExp, Array, Map/Set, WeakMap/WeakSet, Function, Promise, Reflect 등 40여 개의 표준 빌트인 객체를 제공한다.

이때 Math, Reflect, JSON을 제외한 표준 빌트인 객체는 모두 인스턴스를 생성할 수 있는 생성자 함수 객체다.

<br>

생성자 함수 객체인 표준 빌트인 객체는 **프로토타입 메서드**와 **정적 메서드**를 제공하고 생성자 함수 객체가 아닌 표준 빌트인 객체는 정적 메서드만 제공한다.

EX) 표준 빌트인 객체인 String, Number, Function, Array, Date는 생성자 함수로 호출하여 인스턴스를 생성할 수 있다.

```js
// String 생성자 함수에 의한 String 객체 생성
const strObj = new String('Lee');   // String {"Lee"}
console.log(typeof strObj);         // object

// Number 생성자 함수에 의한 Number 객체 생성
const numObj = new Number(123); // Number {123}
console.log(typeof numObj);     // object

// Boolean 생성자 함수에 의한 Boolean 객체 생성
const boolObj = new Boolean(true);  // Boolean {true}
console.log(typeof func);           // function

// Function 생성자 함수에 의한 Function 객체(함수) 생성
const func = new Function('x', 'return x * x'); // f annonymous(x)
console.log(typeof func);   // function

// Array 생성자 함수에 의한 Array 객체(배열) 생성
const arr = new Array(1, 2, 3); // (3) [1, 2, 3]
console.log(typeof arr);        // object

// RegExp 생성자 함수에 의한 RegExp 객체(정규 표현식) 생성
const regExp = new RegExp(/ab+c/i); // /ab+c/i
console.log(typeof regExp);         // object

// Date 생성자 함수에 의한 Date 객체 생성
const date = new Date();    // Fri May ...
console.log(typeof date);   // object
```

<br>

생성자 함수인 표준 빌트인 객체가 생성한 인스턴스의 프로토타입은 표준 빌트인 객체의 prototype 프로퍼티에 바인딩된 객체다.

EX) 표준 빌트인 객체인 String을 생성자 함수로서 호출하여 생성한 String 인스턴스의 프로토타입은 String.prototype이다.

```js
// String 생성자 함수에 의한 String 객체 생성
const strObj = new String('Lee');       // String {"Lee"}

// String 생성자 함수를 통해 생성한 strObj 객체의 프로토타입은 String.prototype이다.
console.log(Object.getPrototypeOf(strObj) === String.prototype);    // true
```

표준 빌트인 객체의 prototype 프로퍼티에 바인딩된 객체는 다양한 기능의 빌트인 프로토타입 메서드를 제공한다.

그리고 표준 빌트인 객체는 인스턴스 없이도 호출 가능한 빌트인 정적 메서드를 제공한다.

EX) 표준 빌트인 객체인 Number의 prototype 프로퍼티에 바인딩된 객체, Number.prototype은 다양한 기능의 빌트인 프로토타입 메서드를 제공한다.

이 프로토타입 메서드는 모든 Number 인스턴스가 상속을 통해 사용할 수 있다.

그리고 표준 빌트인 객체인 Number는 인스턴스 없이 정적으로 호출할 수 있는 정적 메서드를 제공한다.

```js
// Number 생성자는 함수에 의한 Number 객체 생성
const numObj = new Number(1.5); // Number { 1.5 }

// toFixed는 Number.prototype의 프로토타입 메서드다.
// Number.prototype.toFixed는 소수점 자리를 반올림하여 문자열로 반환한다.
console.log(numObj.toFixed());  // 2

// isInteger는 Number의 정적 메서드다.
// Number.isInteger는 인수가 정수(integer)인지 검사하여 그 결과를 Boolean으로 반환한다.
console.log(Number.isInteger(0.5)); // false
```

## 원시값과 래퍼 객체
문자열이나 숫자, 불리언 등의 원시값이 있는데도 문자열, 숫자, 불리언 객체를 생성하는 String, Number, Boolean 등의 표준 빌트인 생성자 함수가 존재하는 이유는 무엇일까?

```js
const str = 'hello';

// 원시 타입인 문자열이 프로퍼티와 메서드를 갖고 있는 객체처럼 동작한다.
console.log(str.length);    // 5
console.log(str.toUpperCase()); // HELLO
```

이는 원시값인 문자열, 숫자, 불리언 값의 경우 이들 원시값에 대해 마치 객체처럼 마침표 표기법(또는 대괄호 표기법)으로 접근하면 자바스크립트 엔진이 일시적으로 원시값을 연관된 객체로 변환해 주기 때문이다.

이처럼 문자열, 숫자, 불리언 값에 대해 객체처럼 접근하면 생성되는 임시 객체를 **래퍼 객체**라고 한다.

<br>

예를 들어, 문자열에 대해 마침표 표기법으로 접근하면 그 순간 래퍼 객체인 String 생성자 함수의 인스턴스가 생성되고 문자열은 래퍼 객체의 `[[StringData]]` 내부 슬롯에 할당된다.

```js
const str = 'hi';

// 원시 타입인 문자열이 래퍼 객체인 String 인스턴스로 변환된다.
console.log(str.length);    // 2
console.log(str.toUpperCase());     // HI

// 래퍼 객체로 프로퍼티에 접근하거나 메서드를 호출한 후, 다시 원시값으로 되돌린다.
console.log(typeof str);    // string
```
이때 문자열 래퍼 객체인 String 생성자 함수의 인스턴스는 String.prototype의 메서드를 상속받아 사용할 수 있다.

<br>

그 후 래퍼 객체의 처리가 종료되면 래퍼 객체의 `[[StringData]]` 내부 슬롯에 할당된 원시값으로 원래의 상태, 즉 식별자가 원시값을 갖도록 되돌리고 래퍼 객체는 가비지 컬렉션의 대상이 된다.

```js
// 1. 식별자 str은 문자열을 값으로 가지고 있다.
const str = 'hello';

// 2. 식별자 str은 암묵적으로 생성된 래퍼 객체를 가리킨다.
// 식별자 str의 값 'hello'는 래퍼 객체의 [[StringData]] 내부 슬롯에 할당한다.
// 래퍼 객체에 name 프로퍼티가 동적 추가된다.
str.name = 'lee';

// 3. 식별자 str은 다시 원래의 문자열, 즉 래퍼 객체의 [[StringData]] 내부 슬롯에 할당된 원시값을 갖는다.

// 4. 새롭게 생성된 래퍼 객체에는 name 프로퍼티가 존재하지 않는다.
console.log(str.name);  // undefined

// 5. 식별자 str은 다시 원래의 문자열, 즉 래퍼 객체의 [[StringData]] 내부 슬롯에 할당된 원시값을 가진다.
// 이때 4 에서 생성된 래퍼 객체는 아무도 참조하지 않는 상태이므로 가비지 컬렉션의 대상이 된다
console.log(typeof str, str);
```

<br>

문자열, 숫자, 불리언, 심벌은 암묵적으로 생성되는 래퍼 객체에 의해 마치 객체처럼 사용할 수 있다.

표준 빌트인 객체인 String, Number, Boolean, Symbol의 프로토타입 메서드 또는 프로퍼티를 참조할 수 있다.

따라서 String, Number, Boolean 생성자 함수를 new 연산자와 함께 호출하여 문자열, 숫자, 불리언 인스턴스를 생성할 필요가 없으며 권장하지도 않는다.

<br>

문자열, 숫자, 불리언, 심벌 이외의 원시값, 즉 null과 undefined는 래퍼 객체를 생성하지 않는다.

따라서 null과 undefined 값을 객체처럼 사용하면 에러가 발생한다.

## 전역 객체
> 💡 **전역 객체**<br>
> 코드가 실행되기 이전 단계에 자바스크립트 엔진에 의해 어떤 객체보다도 먼저 생성되는 특수한 객체로, 어떤 객체도 속하지 않은 최상위 객체이다.

전역 객체는 자바스크립트 환경에 따라 지칭하는 이름이 제각각이다.

브라우저 환경에서는 window가 전역 객체를 가리키지만 Node.js 환경에서는 global이 전역 객체를 가리킨다.

<br>

전역 객체는 표준 빌트인 객체(Object, String, Number, Function, Array 등)와 환경에 따른 호스트 객체(클라이언트 Web API 또는 Node.js의 호스트 API), 그리고 var 키워드로 선언한 전역 변수와 전역 함수를 프로퍼티로 갖는다.

즉, 전역 객체는 계층적 구조상 어떤 객체도 속하지 않은 모든 빌트인 객체의 최상위 객체다.

이때, 최상위 객체라는 것은 프로토타입 상속 관계상에서 최상위 객체라는 이미가 아니다.

전역 객체 자신은 어떤 객체의 프로퍼티도 아니며 객체의 계층적 구조상 표준 빌트인 객체와 호스트 객체를 프로퍼티로 소유한다는 것을 말한다.

<br>

### 전역 객체의 특징
- 전역 객체는 개발자가 의도적으로 생성할 수 없다. 즉, 전역 객체를 생성할 수 있는 생성자 함수가 제공되지 않는다.
- 전역 객체의 프로퍼티를 참조할 때 window 또는 global을 생략할 수 있다.
- 전역 객체는 Object, String, Number, Boolean, Function, Array, RegExp, Date, Math, Promise 같은 모든 표준 빌트인 객체를 프로퍼티로 가지고 있다.
- 자바스크립트 실행 환경에 따라 추가적으로 프로퍼티와 메서드를 갖는다. 브라우저 환경에서는 클라이언트 사이드 Web API를 호스트 객체로 제공하고 Node.js 환경에서는 Node.js 고유의 API를 호스트 객체로 제공한다.
- var 키워드로 선언한 전역 변수와 선언하지 않은 변수에 값을 할당한 암묵적 전역, 그리고 전역 함수는 전역 객체의 프로퍼티가 된다.
- let이나 const 키워드로 선언한 전역 변수는 전역 객체의 프로퍼티가 아니다. 즉, window.foo와 같이 접근할 수 없다. let이나 const 키워드로 선언한 전역 변수는 보이지 않는 개념적인 블록 내에 존재하게 된다.
- 브라우저 환경의 모든 자바스크립트 코드는 하나의 전역 객체 window를 공유한다. 여러 개의 script 태그를 통해 자바스크립트 코드를 분리해도 하나의 전역 객체 window를 공유하는 것은 변함이 없다. 이는 분리되어 있는 자바스크립트 코드가 하나의 전역을 공유한다는 의미이다.

### 빌트인 전역 프로퍼티
> 💡 **빌트인 전역 프로퍼티**<br>
> 전역 객체의 프로퍼티를 말한다.<br>
> 주로 애플리케이션 전역에서 사용하는 값을 제공한다.

#### 📌 Infinity
`Infinity` 프로퍼티는 무한대를 나타내는 숫자값 Infinity를 갖는다.

```js
// 전역 프로퍼티는 window를 생략하고 참조할 수 있다.
console.log(window.Infinity === Infinity);  // true

// 양의 무한대
console.log(3 / 0); // Infinity

// 음의 무한대
console.log(-3/0);  // -Infinity

// Infinity는 숫자값이다.
console.log(typeof Infinity);   // number
```

#### 📌 NAN
`NAN` 프로퍼티는 숫자가 아님을 나타내는 숫자값 NaN을 갖는다.

NaN 프로퍼티는 Number.NaN 프로퍼티와 같다.

```js
console.log(window.NaN);    // NaN
console.log(Number('zxy')); // NaN
console.log(1 * 'string');  // NaN
console.log(typeof NaN);    // number
```

#### 📌 undefined
`undefined` 프로퍼티는 원시 타입 undefined를 값으로 갖는다.

```js
console.log(window.undefined);  // undefined

var foo;
console.log(foo);   // undefined
console.log(typeof foo);    // undefined
```
### 빌트인 전역 함수
빌트인 전역 함수에는 다음과 같은 메서드들이 존재한다.
- eval(사용 금지)
- isFinite
- isNaN
- parseFloat
- parseInt
- encodeURI / decodeURI
- encodeURIComponent / decodeURIComponent
  