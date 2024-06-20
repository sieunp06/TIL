### 목차
- [타입 변환이란?](#타입-변환이란)
- [암묵적 타입 변환](#암묵적-타입-변환)
- [문자열 타입으로 변환](#문자열-타입으로-변환)
- [숫자 타입으로 변환](#숫자-타입으로-변환)
- [불리언 타입으로 변환](#불리언-타입으로-변환)
- [명시적 타입 변환](#명시적-타입-변환)
- [문자열 타입으로 변환](#문자열-타입으로-변환-1)
- [숫자 타입으로 변환](#숫자-타입으로-변환-1)
- [불리언 타입으로 변환](#불리언-타입으로-변환-1)
- [단축 평가](#단축-평가)
- [논리 연산자를 사용한 단축 평가](#논리-연산자를-사용한-단축-평가)
- [객체와 단축 평가](#객체와-단축-평가)
- [옵셔널 체이닝 연산자](#옵셔널-체이닝-연산자)
- [null 병합 연산자](#null-병합-연산자)

---
> 자바스크립트 Deep Dive

<br>

# 09장 타입 변환과 단축 평가
## 타입 변환이란?
개발자가 의도적으로 값의 타입을 변환하는 것을 **명시적 타입 변환** 혹은 **타입 캐스팅**이라고 한다.

```js
var x = 10;

// 명시적 타입 변환
var str = x.toString();
console.log(typeof str, str);   // string 10
```
이때 `x` 변수의 값이 변경된 것은 아니다.

```js
console.log(typeof x, x);   // number 10
```

<br>

개발자의 의도와는 상관없이 표현식을 평가하는 도중에 자바스크립트 엔진에 의해 암묵적으로 타입이 자동 변환되는 것을 **암묵적 타입 변환** 혹은 **타입 강제 변환**이라고 한다.

```js
var x = 10;

// 암묵적 타입 변환
var str = x + '';
console.log(typeof str, str);   // string 10
```

명시적 타입 변환과 마찬가지로 `x` 값이 변경된 것은 아니다.

<br>

명시적 타입 변환과 암묵적 타입 변환의 원시 값은 변경되지 않는데, 그 이유는 원시 값이 변경 불가능한 값이기 때문이다.

타입 변환이란 기존 원시 값을 사용해 다른 타입의 새로운 원시 값을 생성하는 것이다.

즉, 기존 변수 값을 재할당하여 변경하는 것이 아니다.

## 암묵적 타입 변환
> 📌 **암묵적 타입 변환**<br>
> 자바스크립트 엔진이 표현식을 평가할 때 개발자의 의도와는 상관 없이 코드의 문맥을 고려해 암묵적으로 데이터 타입을 강제 변환하는 것을 의미한다.

```js
// 피연산자 모두 문자열 타입이어야 하는 문맥
'10' + 2;   // '102'
```

```js
// 피연산자가 모두 숫자 타입이어야 하는 문맥
5 * '10';   // 50
```

위와 같이 코드의 문맥에 부합하지 않는 다양한 상황이 발생할 수 있다.

이때 자바스크립트는 가급적 에러를 발생시키지 않도록 암묵적 타입 변환을 통해 표현식을 평가한다.

### 문자열 타입으로 변환
```js
1 + '2';    // 12
```

위 예제는 피연산자 중 하나 이상이 문자열이므로 `+` 연산자가 문자열 연결 연산자로 동작하게 된다.

따라서 문자열 연결 연산자의 모든 피연산자는 문자열이어야 하므로 자바스크립트 엔진은 피연산자 중 문자열이 아닌 피연산자를 암묵적으로 타입 변환한다.

### 숫자 타입으로 변환
```js
1 - '1';    // 0
1 * '10';   // 10
1 / 'one';  // NaN
```

위 예제에서 사용한 연산자는 모두 산술 연산자이다.

산술 연산자의 역할은 숫자 값을 만드는 것이기 때문에 산술 연산자의 모든 피연산자는 코드 문맥상 모두 숫자 타입이어야 한다.

때문에 피연산자 중 숫자 타입이 아닌 피연산자를 숫자 타입으로 암묵적 타입 변환한다.

이때, 숫자로 변환할 수 없는 피연산자는 `NaN`이 된다.

<br>

```js
'1' > 0;    // true
```

비교 연산자의 경우도 불리언 값을 만드는 것이 목적이기 때문에 코드의 문맥상 모두 숫자 타입이어야 한다.

때문에 이 또한 숫자 타입으로 피연산자를 암묵적 타입 변환한다.

### 불리언 타입으로 변환
```js
if ('') console.log(x);
```

if 문이나 for 문과 같은 제어문 또는 삼항 조건 연산자의 조건식은 불리언 값, 즉 논리적 참/거짓으로 평가되어야 하는 표현식이다.

때문에 자바스크립트 엔진은 조건식의 평가 결과를 불리언 타입으로 암묵적 타입 변환한다.

```js
if ('') console.log('1');
if (true) console.log('2');
if (0) console.log('3');
if ('str') console.log('4');
if (null) console.log('5');

// 2 4
```

이때 자바스크립트 엔진은 불리언 타입이 아닌 값을 `Truthy` 값과 `Falsy` 값으로 구분한다.

- **Truthy**: 참으로 평가되는 값
- **Falsy**: 거짓으로 평가되는 값

<br>

아래 값들은 false로 평가되는 `Falsy` 값이다.
- false
- undefined
- null
- 0, -0
- NaN
- ''(빈 문자열)

위 Falsy 값 이외의 모든 값은 모두 true로 평가되는 Truthy 값이다.

## 명시적 타입 변환
개발자의 의도에 따라 명시적으로 타입을 변경하는 방법은 다음과 같다.
- 표준 빌트인 생성자 함수(String, Number, Boolean)를 new 연산자 없이 호출하는 방법
- 빌트인 메서드를 사용하는 방법
- 암묵적 타입 변환을 이용하는 방법

<br>

> ⭐ **표준 빌트인 생성자 함수와 빌트인 메서드**<br>
> 표준 빌트인 생성자 함수와 표준 빌트인 메서드는 자바스크립트에서 기본 제공하는 함수다.<br>
><br>
> - 표준 빌트인 생성자 함수는 객체를 생성하기 위한 함수이며 new 연산자와 함께 호출한다.
> - 표준 빌트인 메서드는 자바스크립트에서 기본 제공하는 빌트인 객체의 메서드다.
> <br><br>
> 
> +) 자세한 내용은 [21장 빌트인 객체]()를 확인하면 된다.

### 문자열 타입으로 변환
문자열 타입이 아닌 값을 문자열 타입으로 변환하는 방법은 다음과 같다.
-  **String 생성자 함수**를 new 연산자 없이 호출하는 방법
- **Object.prototype.toString 메서드**를 사용하는 방법
- **문자열 연결 연산자**를 사용하는 방법

<br>

#### 📌 String 생성자 함수를 new 연산자 없이 호출하는 방법

```js
// 숫자 타입 -> 문자열 타입
String(1);          // '1'
String(NaN);        // 'NaN'
String(Infinity)    // 'Infinity'
```
```js
// 불리언 타입 -> 문자열 타입
String(true);       // 'true'
String(false);      // 'false'
```

<br>

#### 📌 Object.prototype.toString 메서드를 사용하는 방법
```js
// 숫자 타입 -> 문자열 타입
(1).toString();         // '1'
(NaN).toString();       // 'NaN'
(Infinity).toString();  // 'Infinity'
```
```js
// 불리언 타입 -> 문자열 타입
(true).toString();      // 'true'
(false).toString();     // 'false'
```

<br>

#### 📌 문자열 연결 연산자를 이용하는 방법
```js
// 숫자 타입 -> 문자열 타입
1 + '';         // '1'
NaN + '';       // 'NaN'
Infinity + '';  // Infinity
```
```js
// 불리언 타입 -> 문자열 타입
true + '';      // true
false + '';     // false
```

### 숫자 타입으로 변환
숫자 타입이 아닌 갑슬 숫자 타입으로 변환하는 방법은 다음과 같다.
- **Number 생성자 함수**를 new 연산자 없이 호출하는 방법
- **parseInt, parseFloat 함수**를 사용하는 방법(문자열만 숫자 타입으로 변환 가능)
- **`+` 단항 산술 연산자**를 이용하는 방법
- **`*` 단항 산술 연산자**를 이용하는 방법

<br>

#### 📌 Number 생성자 함수를 new 연산자 없이 호출하는 방법
```js
// 문자열 -> 숫자
Number('0');        // 0
Number('-1');       // -1
Number('10.53');    // 10.53
```
```js
// 불리언 -> 숫자
Number(true);       // 1
Number(false);      // 0
```

#### 📌 parseInt, parseFloat 함수를 사용하는 방법
`parseInt`와 `parsefloat`을 사용하는 방법은 문자열만 변환 가능하다.

```js
parseInt('0');          // 0
parseInt('-1');         // -1
parseFloat('10.53');    // 10.53
```

#### 📌 + 단항 산술 연산자를 이용하는 방법
```js
// 문자열 -> 숫자
+'0';       // 0
+'-1';      // -1
+'10.53';   // 10.53
```
```js
+true;      // 1
+false;     // 0
```

#### 📌 * 산술 연산자를 이용하는 방법
```js
// 문자열 -> 숫자
'0' * 1;        // 1
'-1' * 1;       // -1
'10.53' * 1;    // 10.53
```
```js
// 불리언 타입 -> 숫자 타입
true * 1;       // 1
false * 1;      // 0
```

### 불리언 타입으로 변환
불리언 타입이 아닌 값을 불리언 타입으로 변환하는 방법은 다음과 같다.
- **Boolean 생성자 함수를 new 연산자 없이 호출**하는 방법
- **!부정 논리 연산자를 두 번 사용**하는 방법

<br>

#### 📌 Boolean 생성자 함수를 new 연산자 없이 호출하는 방법
```js
// 문자열 -> 불리언
Boolean('x');       // true
Boolean('');        // false
Boolean('false');   // true
```
```js
// 숫자 -> 불리언
Boolean(0);         // false
Boolean(1);         // true
Boolean(NaN);       // false
Boolean(Infinity);  // true
```
```js
// null -> 불리언
Boolean(null);      // false
```
```js
// undefined -> 불리언
Boolean(undefined); // false
```
```js
// 객체 타입 -> 불리언
Boolean({});        // true
Boolean([]);        // true
```

#### 📌 ! 부정 논리 연산자를 두 번 사용하는 방법
```js
// 문자열 -> 불리언 
!!'x';      // true
!!'';       // false
!!'false';  // true
```
```js
// 숫자 -> 불리언
!!0;        // false
!!1;        // true
!!NaN;      // false
!!Infinity; // true
```
```js
!!null;     // false
```
```js
!!undefined;    // false
```
```js
!!{};       // true
!![];       // true
```

## 단축 평가
### 논리 연산자를 사용한 단축 평가
[7.5 논리 연산자](https://github.com/sieunp06/TIL/blob/main/Javascript/Book/Modern%20Javascript%20Deep%20Dive/ch07-operator.md#%EB%85%BC%EB%A6%AC-%EC%97%B0%EC%82%B0%EC%9E%90)에서는 논리합(||) 또는 논리곱(&&) 연산자 표현 방식의 평가 결과는 불리언 값이 아닐 수 있다고 언급했다.

논리합 또는 논리곱 연산자 표현식은 언제나 2개의 피연산자 중 어느 한쪽으로 평가된다는 내용이었다.

이는 **암묵적 타입 변환**으로 설명 가능하다.

<br>

#### 📌 논리곱(&&) 연산자
> 💡 **논리곱 연산자**<br>
> 논리곱 연산자는 두 개의 피연산자가 모두 true로 평가될 때 true를 반환한다.

```js
'Cat' && 'Dog'  // 'Dog'
```

첫 번째 피연산자인 `Cat`은 빈 문자열이 아니기 때문에 `Truthy` 값이므로 true로 평가된다.

하지만 논리곱의 경우 결과를 결정하는 것은 첫 번째 피연산자가 아닌 두 번째 피연산자이다.

때문에 두 번째 피연산자인 `Dog`을 반환한다.

#### 📌 논리합(||) 연산자
> 💡 **논리합 연산자**<br>
> 논리합 연산자는 두 개의 피연산자 중 하나만 true로 평가되어도 true를 반환한다.

```js
'Cat' || 'Dog'  // 'Cat'
```

첫 번째 연산자인 `Cat`은 위와 같이 `Truthy` 값이기 때문에 true로 평가된다.

이때 논리곱 연산과 다른 점은 논리합 연산은 둘 중 하나만 true라면 true를 반환한다.

때문에 첫 번째 연산자만 평가해도 위 표현식을 평가할 수 있기 때문에 `Cat`을 반환한다.

#### 📌 단축 평가란?
> 💡 **단축 평가**<br>
> 논리 연산의 결과를 결정하는 피연산자를 타입 변환하지 않고 그대로 반환하는 것이다.<br>
> 또한 표현식을 평가하는 도중에 평가 결과가 확정된 경우 나머지 평가 과정을 생략하는 것을 말한다.

단축 평가는 다음 규칙을 따른다.

|단축 평가 표현식|평가 결과|
|--|--|
|true \|\| anything|true|
|false \|\| anything|anything|
|true && anything|anything|
|false && anything|false|

<br>

단축 평가를 사용하면 if 문을 대체할 수 있다.

```js
var done = true;
var message = '';

// 주어진 조건이 true일 때
if (done) message = '완료';
```

위 if 문을 다음과 같이 대체할 수 있다.

```js
var done = true;
var message = '';

message = done && '완료';
```

논리곱 연산자는 `done`이 true로 평가되고 `'완료'` 또한 `Truthy`로 평가되기 때문에 `'완료'`가 반환되고 message에는 `'완료'`가 할당된다.

이는 논리합도 비슷하게 활용할 수 있다.

### 객체와 단축 평가
#### 📌 객체를 가리키기를 기대하는 변수가 null 또는 undefined가 아닌지 확인하고 프로퍼티를 참조할 때

객체를 가리키기를 기대하는 변수의 값이 객체가 아니라 `null` 또는 `undefined`인 경우, 객체의 프로퍼티를 참조하면 다음과 같이 타입 에러가 발생한다.

```js
var elem = null;
var value = elem.value; // TypeError: Cannot read property 'value' of null
```

하지만 단축 평가를 사용하면 에러가 발생하지 않는다.

```js
var elem = null;

var value = elem && elem.value; // null
```

#### 📌 함수 매개변수에 기본값을 설정할 때
함수를 호출할 때 인수를 전달하지 않으면 매개변수에는 undefined가 할당된다.

```js
function getStringLength(str) {
    // 단축 평가를 사용한 매개변수의 기본값 설정
    str = str || '';
    return str.length;
}

getStringLength();  // 0
getStringLength('hi');  // 2
```

이때 위와 같이 단축 평가를 사용해 매개변수의 기본값을 설정하면 undefined로 인해 발생할 수 있는 에러를 방지할 수 있다.

<br>

하지만 ES6부터 매개변수의 기본값을 설정할 수 있게 되었기 때문에 단축 평가를 통해 기본 값을 설정하지 않아도 된다.

```js
// ES6의 매개변수의 기본값 설정
function getStringLength(str = '') {
    return str.length;
}

getStringLength();  // 0
getStringLength('hi');  // 2
```

### 옵셔널 체이닝 연산자
> 💡 **옵셔널 체이닝 연산자(?.)**<br>
> ES11(ECMAScript2020)에서 도입되었으며, 좌항의 피연산자가 null 또는 undefined인 경우 undefined를 반환하고 그렇지 않으면 우항의 프로퍼티 참조를 이어간다.

```js
var elem = null;

var value = elem?.value;
console.log(value);     // undefined
```

위 예시를 보면 `elem`이 `null` 또는 `undefined`이기 때문에 value는 undefined가 된다.

<br>

옵셔널 체이닝 연산자가 도입되기 이전에는 논리 연산자 `&&`를 사용한 단축 평가를 통해 변수가 `null` 또는 `undefined`인지 확인했다.

```js
var elem = null;

var value = elem && elem.value;
console.log(value);     // null
```

#### 📌 옵셔널 체이닝 연산자를 사용하는 이유
논리 연산자 `&&`은 좌항 피연산자가 false로 평가되는 `Falsy` 값이면 좌항 피연산자를 그대로 반환한다.

```js
var str = '';

var length = str && str.length;
console.log(length);    // ''
```

위 예시에서 `str`은 빈 문자열이기 때문에 `Falsy`기 때문에 `str`을 그대로 반환한다.

하지만 0이나 `''`은 객체로 평가될 때가 있다.

<br>

하지만 옵셔널 체이닝 연산자는 좌항 피연산자가 Falsy 값이라도 null또는 undefined가 아니면 우항의 프로퍼티 참조를 이어간다.

```js
var str = '';

var length = str?.length;
console.log(length);    // 0
```

### null 병합 연산자
> 💡 **null 병합 연산자(??)**<br>
> ES11(ECMAScript2020)에서 도입되었으며, 좌항의 피연산자가 null 또는 undefined인 경우 우항의 피연산자를 반환하고, 그렇지 않으면 좌항의 피연산자를 반환한다.

null 병합 연산자는 변수에 기본값을 설정할 때 유용하다.

```js
var foo = null ?? 'default string';
console.log(foo);   // default string
```

<br>

null 병합 연산자가 도입되기 이전에는 논리 연산자 `||`를 사용한 단축 평가를 통해 변수에 기본값을 설정했다.

```js
var foo = '' || 'default string';
console.log(foo);   // default string
```

하지만 논리 연산자 `||`는 좌항의 피연산자가 Falsy 값이면 우항의 피연산자를 반환한다.

만약 좌항의 피연산자가 Falsy 값인 0이나 `''`도 기본값으로 유효하다면 예기치 않은 동작이 발생할 수 있다.

<br>

이때 null 병합 연산자는 좌항의 연산자가 Falsy 값이라도 null 또는 undefined가 아니면 좌항의 피연산자를 그대로 반환한다.

```js
var foo = '' ?? 'default string';
console.log(foo);   // ''
```
