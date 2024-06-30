### 목차
- [strict mode란?](#strict-mode란)
- [strict mode의 적용](#strict-mode의-적용)
- [strict mode를 사용할 때 주의해야 할 것들](#strict-mode를-사용할-때-주의해야-할-것들)
  - [전역에 strict mode를 적용하는 것은 피하자](#전역에-strict-mode를-적용하는-것은-피하자)
  - [함수 단위로 strict mode를 적용하는 것도 피하자](#함수-단위로-strict-mode를-적용하는-것도-피하자)
- [strict mode가 발생시키는 에러](#strict-mode가-발생시키는-에러)
  - [암묵적 전역](#암묵적-전역)
  - [변수, 함수, 매개변수의 삭제](#변수-함수-매개변수의-삭제)
  - [매개변수 이름의 중복](#매개변수-이름의-중복)
  - [with 문의 사용](#with-문의-사용)
- [strict mode 적용에 의한 변화](#strict-mode-적용에-의한-변화)
- [일반 함수의 this](#일반-함수의-this)
- [arguments 객체](#arguments-객체)


---
> 모던 자바스크립트 Deep Dive

<br>

# 20장 strict mode
## strict mode란?
```js
function foo () {
    x = 10;
}
foo();

console.log(x); // 
```

`foo` 함수 내에 선언하지 않은 값 `x` 변수에 값 10을 할당했다.

이때 `x` 변수를 찾아야 `x`에 값을 할당할 수 있기 때문에 자바스크립트 엔진은 `x` 변수가 어디 선언되었는지 스코프 체인을 통해 검색하기 시작한다.

1. 먼저 `foo` 함수의 스코프에서 `x` 변수의 선언을 검색한다.
   - `foo` 함수의 스코프에는 `x` 변수의 선언이 없기 때문에 검색에 실패한다.
2. 자바스크립트 엔진은 `x` 변수를 검색하기 위해 `foo` 함수 컨텍스트의 상위 스코프에서 `x` 변수의 선언을 검색한다.
3. 전역 스코프에서 `x` 변수의 선언이 존재하지 않기 때문에 `ReferenceError`를 발생시킬 것 같지만, 자바스크립트 엔진은 암묵적으로 전역 객체에 x 프로퍼티를 동적 생성한다.
    - 이때, 전역 객체의 `x` 프로퍼티는 전역 변수처럼 사용할 수 있다.


위 현상을 **암묵적 전역**이라고 한다.

<br>

이러한 암묵적 전역은 오류를 발생시키는 원인이 될 가능성이 높다.

따라서 반드시 `var`, `let`, `const` 키워드를 사용하여 변수를 선언한 다음 사용해야 한다.

<br>

하지만 오타나 문법으로 인한 실수가 있을 수 있다.

이러한 잠재적인 오류를 발생시키기 어려운 개발 환경을 만들고 그 환경에서 개발하는 것이 좀 더 근본적인 해결책이다.

<br>

#### 📌 strict mode의 추가
> 💡 **strict mode**<br>
> ES5에서 적용되었으며, 자바스크립트 언어의 문법을 좀 더 엄격히 적용하여 오류를 발생시킬 가능성이 높거나 자바스크립트 엔진의 최적화 작업에 문제를 일으킬 수 있는 코드에 대해 명시적인 에러를 발생시킨다.

`ESLint`와 같은 린트 도구를 사용해도 strict mode와 유사한 효과를 얻을 수 있다.

## strict mode의 적용
`strict mode`를 적용하려면 전역의 선두 또는 함수의 몸체의 선두에 `'use strict';`를 추가한다.

```js
'use strict';

function foo () {
    x = 10; // ReferencesError: x is not defined
}
foo();
```

함수 몸체의 선두에 추가하면 해당 함수와 중첩 함수에 strict mode가 적용된다.

```js
function foo () {
    'use strict';

    x = 10; // ReferenceError: x is not defined
}
foo();
```

<br>

코드의 선두에 `'use strict';`를 위치시키지 않으면 strict mode가 제대로 적용되지 않는다.

```js
function foo() {
    x = 10; // 에러를 발생시키지 않는다.
    'use strict';
}
foo();
```

## strict mode를 사용할 때 주의해야 할 것들
### 전역에 strict mode를 적용하는 것은 피하자
전역에 적용한 strict mode는 스크립트 단위로 적용된다.

strict mode 스크립트와 non-strict mode 스크립트를 혼용하는 것은 오류를 발생시킬 수 있다.

특히 외부 서드파트 라이브러리를 사용하는 경우, 라이브러리가 non-strict mode인 경우도 있기 때문에 전역에 strict mode를 적용하는 것은 바람직하지 않다.

<br>

이런 경우, 즉시 실행 함수로 스크립트 전체를 감싸서 스코프를 구분하고 즉시 실행 함수의 선두에 strict mode를 적용한다.

```js
// 즉시 실행 함수의 선두에 strict mode 적용
(function () {
    'use strict';

    // Do something ...
}());
```

### 함수 단위로 strict mode를 적용하는 것도 피하자
앞에서 함수 단위로 strict mode를 적용할 수 있다.

하지만 어떤 한수에는 strict mode를 적용하고 어떤 함수는 strict mode를 적용하지 않는 것은 바람직하지 않으며, 모든 함수에 strict mode를 적용하는 것은 번거롭다.

또한 strict mode가 적용한 함수가 참조할 함수 외부의 컨텍스트에 strict mode를 적용하지 않는다면 이 또한 문제가 될 수 있다.

```js
(function () {
    // non-strict mode
    var let = 10;   // 에러가 발생하지 않는다.
    function foo() {
        'use strict';

        let = 20;   // SyntaxError:
    }
    foo();
} ());
```

따라서 strict mode는 즉시 실행 함수로 감싼 스크립트 단위로 적용하는 것이 바람직하다.

## strict mode가 발생시키는 에러
다음은 strict mode를 적용했을 때 에러가 발생하는 대표적인 사례이다.

### 암묵적 전역
선언하지 않은 변수를 참조하면 ReferenceError가 발생한다.

```js
(function () {
    'use strict';

    x = 1;
    console.log(x); // ReferneceError: x is not defined
} ());
```

### 변수, 함수, 매개변수의 삭제
delete 연산자로 변수, 함수, 매개변수를 삭제하면 SyntaxError가 발생한다.

```js
(function () {
    'use strict';

    var x = 1;
    delete x;       // SyntaxError: Delete of an unqualified identifier in strict mode

    function foo(a) {
        deelte a;   // SyntaxError: Delete of an unqualified identifier in strict mode
    }
    delete foo;     //SyntaxError: Delete of an unqualified identifier in strict mode
} ());
```

### 매개변수 이름의 중복
중복된 매개변수 이름을 사용하면 SyntaxError가 발생한다.

```js
(function () {
    'use strict';

    // SyntaxError: Duplicate parameter name not allowed in this context
    function foo(x, x) {
        return x + x;
    }
    console.log(foo(1, 2));
} ());
```

### with 문의 사용
`with` 문을 사용하면 SyntaxError가 발생한다.

`with` 문은 전달된 객체를 스코프 체인에 추가한다.

동일한 객체의 프로퍼티를 반복해서 사용할 때, 객체의 이름을 생략할 수 있어 코드가 간단해지는 효과가 있지만 성능과 가독성이 나빠지는 문제가 있다.

따라서 `with` 문은 사용하지 않는 겻이 좋다.

```js
(function () {
    'use strict';

    // SyntaxError: Strict mode may not include a with statement
    with({ x: 1 }) {
        console.log(x);
    }
} ());
```

## strict mode 적용에 의한 변화
### 일반 함수의 this
`strict mode`에서 함수를 일반 함수로서 호출하면 this에 undefiend가 바인딩된다.

생성자 함수가 아닌 일반 함수 내부에서는 this를 사용할 필요가 없기 때문이다.

이때 에러는 발생하지 않는다.

```js
(function () {
    'use strict';

    function foo() {
        console.log(this);  // undefined
    }
    foo();

    function Foo() {
        console.log(this);  // Foo
    }
    new Foo();
}());
```

### arguments 객체
strict mode에서는 매개변수에 전달된 인수를 재할당하여 변경해도 arguments 객체에 반영되지 않는다.

```js
(function (a) {
    'use strict';
    // 매개변수에 전달된 인수를 재할당하여 변경
    a = 2;

    // 변경된 인수가 arguments 객체에 반영되지 않는다.
    console.log(arguments); // { 0: 1, length: 1}
}(1));
```