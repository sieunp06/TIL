### 목차
- [일급 객체](#일급-객체)
- [함수 객체의 프로퍼티](#함수-객체의-프로퍼티)
- [arguments 프로퍼티](#arguments-프로퍼티)
- [caller 프로퍼티](#caller-프로퍼티)
- [length 프로퍼티](#length-프로퍼티)
- [name 프로퍼티](#name-프로퍼티)
- [\_\_proto\_\_ 접근자 프로퍼티](#__proto__-접근자-프로퍼티)
- [prototype 프로퍼티](#prototype-프로퍼티)

---
> 모던 자바스크립트 Deep Dive

<br>

# 18장 함수와 일급 객체
## 일급 객체
다음과 같은 조건을 만족하는 객체를 `일급 객체`라고 한다.
- 무명의 리터럴로 생성할 수 있다. 즉, 런타임에 생성이 가능하다.
- 변수나 자료구조(객체, 배열)에 저장할 수 있다.
- 함수의 매개변수에 전달할 수 있다.
- 함수의 반환값으로 사용할 수 있다.

<br>

자바스크립트의 함수는 위 조건들을 모두 만족하기 때문에 일급 객체라고 할 수 있다.

```js
// 1. 함수는 무명의 리터럴로 생성할 수 있다.
// 2. 함수는 변수에 저장할 수 있다.
// 런타임(할당 단계)에 함수 리터럴이 평가되어 함수 객체가 생성되고 변수에 할당된다.
const increse = function (num) {
    return ++num;
};

const decrease = function (num) {
    return --num;
};

// 2, 함수는 객체에 저장할 수 있다.
const auxs = { increase, decrease };

// 3. 함수의 매개변수에 전달할 수 있다.
// 4. 함수의 반환값으로 사용할 수 있다.
function makeCounter(aux) {
    let num = 0;

    return function () {
        num = aux(num);
        return num;
    };
}

// 3. 함수는 매개변수에게 함수를 전달할 수 있다.
const increaser = makeCounter(auxs.increase);
const decreaser = makeCounter(auxs.decrease);
```

함수가 일급객체라는 것은 함수를 객체와 동일하게 사용할 수 있다는 의미이다.

하지만 함수는 객체이지만 일반 객체와는 차이가 있다.

<br>

일반 객체는 호출할 수 없지만, 함수 객체는 호출할 수 있다.

또한 함수 객체는 일반 객체에는 없는 함수 고유의 프로퍼티를 소유한다.

## 함수 객체의 프로퍼티
`square` 함수의 모든 프로퍼티 어트리뷰트를 확인하면 다음과 같다.

```js
function square(number) {
    return number * number;
}

console.log(Object.getOwnPropertyDescriptors(square));
/*
{
    length: ...,
    name: ...,
    arguments: ...,
    caller: ...,
    prototype: ...
}
*/

console.log(Object.getOwnPropertyDescriptor(square, '__proto__'));  // undefined

console.log(Object.getOwnPropertyDescriptor(Object.prototype, `__proto__`));
// {get: f, set: f, enumerable: false, configurable: true}
```

`arguments`, `caller`, `length`, `name`, `prototype` 프로퍼티는 모두 함수 객체의 데이터 프로퍼티며, 일반 객체에는 없는 함수 객체 고유의 프로퍼티다.

하지만 `__proto__`는 접근자 프로퍼티며, 함수 객체 고유의 프로퍼티가 아니라 `Object.prototype` 객체의 프로퍼티를 상속 받은 것을 알 수 있다.

### arguments 프로퍼티
함수 객체의 `aruments` 프로퍼티 값은 `arguments` 객체다.

해당 객체는 함수 호출 시 전달된 인수들을 정보를 담고 있는 순회 가능한 유사 배열 객체이며, 함수 내부에서 지역 변수처럼 사용된다.

즉, 함수 외부에서는 참조할 수 없다.

<br>

함수 객체의 `arguments` 프로퍼티는 ES3부터 표준에서 폐지되었기 때문에 `Function.arguments`와 같은 사용법은 권장되지 않으며, 함수 내부에서 지역 변수처럼 사용할 수 있는 `arguments` 객체를 참조하도록 한다.

<br>

#### ✨ arguments 객체
선언된 매개변수의 개수와 함수를 호출할 때 전달하는 인수의 개수를 확인하지 않는 자바스크립트의 특성 상 함수가 호출되면 인수 개수를 확인하고 이에 따라 함수의 동작을 달리 정의할 수 있을 수 있다.

이때 유용한 것이 `arguments` 객체이다.

<br>

`arguments` 객체는 매개변수의 개수를 확정할 수 없는 **가변 인자 함수**를 구현할 때 유용하다.

```js
function sum() {
    let res = 0;

    // arguments 객체는 length 프로퍼티가 있는 유사 배열 객체이므로 for 문으로 순회할 수 있다.
    for (let i = 0; i < arguments.length; i++) {
        res += arguments[i];
    }

    return res;
}

console.log(sum());         // 0
console.log(sum(1, 2));     // 3
console.log(sum(1, 2, 3));  // 6
```

<br>

이때 주의할 점은 `arguments` 객체는 배열이 아닌 유사 배열 객체이다.

즉, 배열 메서드를 사용할 경우 에러가 발생한다.

`Function.prototype.call`, `Function.prototype.apply`를 사용해 간접 호출할 수는 있지만 번거롭다.

```js
function sum() {
    // arguments 객체를 배열로 변환
    const array = Array.prototype.call(arguments);
    return array.reduce(function (pre, cur) {
        return pre + cur;
    }, 0);
}

console.log(sum(1, 2));             // 3
console.log(sum(1, 2, 3, 4, 5));    // 15
```

> ⭐ 간접 호출은 [22.2.4장 Function.prototype.apply/call/bind 메서드에 의한 간접 호출]()을 참고하면 된다.

<br>

이러한 번거로움을 해결하기 위해 ES6에서는 `Rest` 파라미터를 도입했다.

```js
// ES6 Rest parameter
function sum(...args) {
    return args.reduce((pre, cur) => pre + cur, 0);
}

console.log(sum(1, 2));             // 3
console.log(sum(1, 2, 3, 4, 5));    // 15
```

> ⭐ arguments 객체와 Rest 파라미터에 대해서는 [26.4장 Rest 파라미터]()를 참고하면 된다.

### caller 프로퍼티
`caller` 프로퍼티는 ECMAScript 사양에 포함되지 않은 비표준 프로퍼티다.

### length 프로퍼티
`length` 프로퍼티는 함수를 정의할 때 선언한 매개변수의 개수를 가리킨다.

```js
function foo() {}
console.log(foo.length);    // 0

function bar(x) {
    return x;
}
console.log(bar.length);    // 1
```

<br>

이때 `arguments` 객체의 length 프로퍼티와 함수 객체의 length 프로퍼티 값은 다를 수 있다.

arguments 객체의 length 프로퍼티는 인자의 개수를 가리키고, 함수 객체의 length 프로퍼티는 매개변수의 개수를 가리킨다.

### name 프로퍼티
`name` 프로퍼티는 함수의 이름을 나타내며, ES6에서 정식 표준이 되었다.

```js
// 기명 함수 표현식
var namedFunc = function foo() {};
console.log(namedFunc.name);    // foo

// 익명 함수 표현식
var anonymousFunc = function() {};
// ES5: 빈 문자열
// ES6: 함수 객체를 가리키는 변수 이름
console.log(anonymousFunc.name);    // anonymousFunc

// 함수 선언문
function bar() {}
console.log(bar.name);  // bar
```

### __proto\_\_ 접근자 프로퍼티
모든 객체는 `[[Prototype]]` 내부 슬롯을 갖는다.

> 💡 **[[Prototype]] 내부 슬롯**<br>
> 객체지향 프로그래밍의 상속을 구현하는 프로토타입 객체를 가리킨다.

<br>

`__proto__` 프로퍼티는 `[[Prototype]]` 내부 슬롯이 가리키는 프로토타입 객체에 접근하기 위해 사용하는 접근자 프로퍼티다.

내부 슬롯에는 직접 접근할 수 없고 간접적인 접근 방법을 제공하는 경우에 한하여 접근할 수 있다.

```js
const obj = { a: 1 };

// 객체 리터럴 방식으로 생성한 객체의 프로토타입 객체는 Object.prototype이다.
console.log(obj.__proto__ === Object.prototype);    // true

// 객체 리터럴 방식으로 생성한 객체는 프로토타입 객체인 Object.prototype의 프로퍼티를 상속받는다.
console.log(obj.hasOwnProperty('a'));   // true
console.log(obj.hasOwnProperty('__proto__'));   // false
```

### prototype 프로퍼티
`prototype` 프로퍼티는 생성자 함수로 호출할 수 있는 함수 객체, 즉 constructor만이 소유하는 프로퍼티다.

일반 객체와 생성자 함수로 호출할 수 없는 non-constructor에는 prototype 프로퍼티가 없다.

```js
// 함수 객체는 prototype 프로퍼티를 소유한다.
(function () {}).hasOwnProperty('prototype');   // true

// 일반 객체는 prototype 프로퍼티를 소유하지 않는다.
({}).hasOwnProperty('prototype');   // false
```
`prototype` 프로퍼티는 함수가 객체를 생성하는 생성자 함수로 호출될 때 생성자 함수가 생성할 인스턴스의 프로토타입 객체를 가리칸다.