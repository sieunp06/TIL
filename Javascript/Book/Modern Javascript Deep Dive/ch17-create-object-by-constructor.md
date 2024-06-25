### 목차
- [Object 생성자 함수](#object-생성자-함수)
- [생성자 함수](#생성자-함수)
  - [객체 리터럴에 의한 객체 생성 방식의 문제점](#객체-리터럴에-의한-객체-생성-방식의-문제점)
  - [생성자 함수에 의한 객체 생성 방식의 장점](#생성자-함수에-의한-객체-생성-방식의-장점)
  - [생성자 함수의 인스턴스 생성 과정](#생성자-함수의-인스턴스-생성-과정)
  - [내부 메서드 \[\[Call\]\]과 \[\[Construct\]\]](#내부-메서드-call과-construct)
  - [constructor와 non-constructor의 구분](#constructor와-non-constructor의-구분)
  - [new 연산자](#new-연산자)
  - [new.target](#newtarget)


---
> 자바스크립트 Deep Dive

<br>

# 17장 생성자 함수에 의한 객체 생성
## Object 생성자 함수
`new` 연산자와 함께 `Object` 생성자 함수를 호출하면 빈 객체를 생성하여 반환한다.

빈 객체를 생성한 이후 프로퍼티 또는 메서드를 추가하여 객체를 완성할 수 있다.

```js
// 빈 객체의 생성
const person = new Object();

// 프로퍼티 추가
person.name = 'Lee';
person.sayHello = function () {
    console.log('Hi! My name is ' + this.name);
};

console.log(person);    // {name: "Lee", sayHello: f}
person.sayHello();  // Hi! My name is Lee
```

<br>

> 💡 **생성자 함수**<br>
> new 연산자와 함께 호출하여 객체(인스턴스)를 생성하는 함수.<br>
> 이때 생성자 함수에 의해 생성된 객체를 인스턴스라 한다.

<br>

자바스크립트는 `Object` 생성자 함수 이외에도 String, Number, Boolean, Function, Array, Date, RegExp, Promise 등의 빌트인 생성자 함수를 제공한다.

```js
// String 생성자 함수에 의한 String 객체 생성
const strObj = new String('Lee');
console.log(typeof strObj); // object
console.log(strObj);        // String {"Lee"}
```
```js
// Number 생성자 함수에 의한 Number 객체 생성
const numObj = new Number(123);
console.log(typeof numObj); // object
console.log(numObj);        // Number {123}
```
```js
// Boolean 생성자 함수에 의한 Function 객체(함수) 생성
const func = new Function('x', 'return x * x');
console.log(typeof func);   // function
console.log(func);          // f anonymous(x)
```
```js
// Array 생성자 함수에 의한 Array 객체(배열) 생성
const arr = new Array(1, 2, 3);
console.log(typeof arr);    // object
console.log(arr);           // [1, 2, 3]
```
```js
// RegExp 생서앚 함수에 의한 RegExp 객체(정규 표현식) 생성
const regExp = new RegExp(/ab+c/i);
console.log(typeof regExp); // object
console.log(regExp);        // /ab+c/i
```
```js
// Date 생성자 함수에 의한 Date 객체 생성
const date = new Date();
console.log(typeof date);   // object
console.log(date);          // Mon May 4 2020 ~~
```

이때 반드시 `Object` 생성자 함수를 사용해 빈 객체를 생성해야 하는 것은 아니다.

객체 리터럴을 사용하는 방법이 Object 생성자 함수를 사용해 객체를 사용하는 것보다 간편하다.

때문에 Object 생성자 함수를 사용해 객체를 생성하는 방식은 특별한 이유가 없다면 유용해 보이지 않는다.

## 생성자 함수
### 객체 리터럴에 의한 객체 생성 방식의 문제점
#### 📌 객체 리터럴에 의한 객체 생성 방식은 단 하나의 객체만 생성한다.

객체 리터럴 객체 생성 방식은 동일한 프로퍼티를 갖는 객체를 여러 개 생성해야 하는 경우 매번 같은 프로퍼티를 기술해야 하기 때문에 비효율적이다.

```js
const circle1 = {
    radius: 5,
    getDiameter() {
        return 2 * this.radius;
    }
};

console.log(circle1.getDiameter()); // 10

const circle2 = {
    radius: 10,
    getDiameter() {
        return 2 * this.radius;
    }
};

console.log(circle2.getDiameter()); // 20
```

위 예시에서는 원을 표현한 객체인 `circle1` 객체와 `circle2` 객체는 프로퍼티 구조가 동일하다.

<br>

위처럼 객체 리터럴에 의해 객체를 생성하는 경우 프로퍼티 구조가 동일함에도 매번 같은 프로퍼티와 메서드를 기술해야 한다.

### 생성자 함수에 의한 객체 생성 방식의 장점
#### 📌 생성자 함수에 의한 객체 생성 방식은 구조가 동일한 객체 여러 개를 간편하게 생성할 수 있다.
생성자 함수 객체 생성 방식은 객체 리터럴 객체 생성 방식과 달리 템플릿(클래스)처럼 생성자 함수를 사용하여 프로퍼티 구조가 동일한 객체 여러 개를 간편하게 생성할 수 있다.

```js
// 생성자 함수
function Circle(radius) {
    // 생성자 함수 내부의 this는 생성자 함수가 생성할 인스턴스를 가리킨다.
    this.radius = radius;
    this.getDiameter = function () {
        return 2 * this.radius;
    };
}

// 인스턴스의 생성
const circle1 = new Circle(5);
const circle2 = new Circle(10);

console.log(circle1.getDiameter()); // 10
console.log(circle2.getDiameter()); // 20
```

<br>

#### ✨ 생성자 함수란?
> ⭐ **생성자 함수**<br>
> 이름 그대로 객체(인스턴스)를 생성하는 함수다.

자바스크립트에서의 생성자 함수는 자바와 같은 클래스 기반 객체지향 언어의 생성자와는 다르게 형식이 정해져 있는 것이 아니라 일반 함수와 동일한 방법으로 생성자 함수를 정의하고 new 연산자와 함께 호출한다.

new 연산자와 함께 생성자 함수를 호출하지 않으면 생성자 함수가 아니라 일반 함수로 동작한다.

```js
// new 연산자와 함께 호출하지 않으면 생성자 함수로 동작하지 않는다.
const circle3 = Circle(15);

// 일반 함수로서 호출된 Circle은 반환문이 없으므로 암묵적으로 undefined를 반환한다.
console.log(circle3);   // undefined

// 일반 함수로서 호출된 Circle 내의 this는 전역 객체를 가리킨다.
console.log(radius);    // 15
```

### 생성자 함수의 인스턴스 생성 과정
생성자 함수의 역할은 다음과 같다.
- 프로퍼티 구조가 동일한 인스턴스를 생성하기 위한 템플릿(클래스)으로서 동작하여 인스턴스를 생성하는 것
- 생성된 인스턴스를 초기화(인스턴스 프로퍼티 추가 및 초기화 할당)하는 것

이때 생성자 함수가 인스턴스를 생성하는 것은 필수이고, 생성된 인스턴스를 초기화하는 것은 옵션이다.

<br>

```js
// 생성자 함수
function Circle(radius) {
    // 인스턴스 초기화
    this.radius = radius;
    this.getDiameter = function () {
        return 2 * this.radius;
    };
}

// 인스턴스 생성
const circle = new Circle(5);
```

자바스크립트 엔진은 암묵적인 처리를 통해 인스턴스를 생성하고 반환한다.

new 연산자와 함께 생성자 함수를 호출하면 자바스크립트 엔진은 다음과 같은 과정을 거쳐 인스턴스를 생성하고 초기화한 후 이를 반환한다.

<br>

#### 📌 1. 인스턴스 생성과 this 바인딩
암묵적으로 빈 객체가 생성된다.

이 빈 객체가 생성자 함수가 생성한 인스턴스다.

즉, 인스턴스는 this에 바인딩된다.

이 과정은 런타임 이전에 실행된다.

<br>

> ⭐ **바인딩**<br>
> 식별자와 값을 연결하는 과정을 말한다.<br>
> EX) 변수 선언은 변수 이름과 확보된 메모리 공간의 주소를 바인딩하는 것이다.

<br>

```js
function Circle(radius) {
    // 1. 암묵적으로 인스턴스가 생성되고 this에 바인딩된다.
    console.log(this);  // Circle {}

    this.radius = radius;
    this.diaeter = function () {
        return 2 * this.radius;
    };
}
```

<br>

#### 📌 2. 인스턴스 초기화
생성자 함수에 기술되어 있는 코드가 한 줄씩 실행되어 this에 바인딩되어 있는 인스턴스를 초기화한다.

즉, this에 바인딩되어 있는 인스턴스에 프로퍼티나 메서드를 추가하고 생성자 함수가 인수로 전달받은 초기값을 인스턴스 프로퍼티에 할당하여 초기화하거나 고정값을 할당한다.

```js
function Circle(radius) {
    // 1. 암묵적으로 인스턴스가 생성되고 this에 바인딩된다.

    // 2. this에 바인딩되어 있는 인스턴스를 초기화한다.
    this.radius = radius;
    this.getDiameter = function () {
        return 2 * this.radius;
    };
}
```
<br>

#### 📌 3. 인스턴스 반환
생성자 함수 내부에서 모든 처리가 끝나면 완성된 인스턴스가 바인딩된 this를 암묵적으로 반환한다.

```js
function Circle(radius) {
    // 1. 암묵적으로 인스턴스가 생성되고 this에 바인딩된다.

    // 2. this에 바인딩되어 있는 인스턴스를 초기화한다.
    this.radius = radius;
    this.getDiameter = function () {
        return 2 * this.radius;
    };

    // 3. 완성된 인스턴스가 바인딩된 this가 암묵적으로 반환된다.
}

// 인스턴스 생성. Circle 생성자 함수는 암묵적으로 this를 반환한다.
const circle = new Circle(1);
console.log(circle);    // Circle {radius: 1, getDiameter: f}
```

<br>

이때 this가 아닌 다른 객체를 명시적으로 반환하면 this가 반환되지 못하고 return 문에 명시한 객체가 반환된다.

```js
function Circle(radius) {
    // 1. 암묵적으로 인스턴스가 생성되고 this에 바인딩된다.

    // 2. this에 바인딩되어 있는 인스턴스를 초기화한다.
    this.radius = radius;
    this.getDiameter = function () {
        return 2 * this.radius;
    };

    // 3. 완성된 인스턴스가 바인딩된 this가 암묵적으로 반환된다.
    // 명시적으로 객체를 반환하면 this 반환이 무시된다.
    return {};
}

// 인스턴스 생성. 
// Circle 생성자 함수는 명시적으로 반환한 객체를 반환한다.
const circle = new Circle(1);
console.log(circle);    // {}
```

<br>

반면 명시값으로 원시 값을 반환하면 원시 값 반환은 무시되고 암묵적으로 this가 반환된다.

```js
function Circle(radius) {
    // 1. 암묵적으로 인스턴스가 생성되고 this에 바인딩된다.

    // 2. this에 바인딩되어 있는 인스턴스를 초기화한다.
    this.radius = radius;
    this.getDiameter = function () {
        return 2 * this.radius;
    };

    // 3. 완성된 인스턴스가 바인딩된 this가 암묵적으로 반환된다.
    // 명시적으로 원시 값을 반환하면 원시 값 반환은 무시되고 암묵적으로 this가 반환된다.
    return 100;
}

// 인스턴스 생성. 
// Circle 생성자 함수는 명시적으로 반환한 객체를 반환한다.
const circle = new Circle(1);
console.log(circle);    // Circle {radius: 1, getDiameter: f}
```

생성자 함수 내부에서 명시적으로 this가 아닌 다른 값을 반환하는 것은 생성자 함수의 기본 동작을 훼손한다.

따라서 생성자 함수 내부에서 return 문을 반드시 생략해야 한다.

### 내부 메서드 [[Call]]과 [[Construct]]
함수 선언문 또는 함수 표현식으로 정의한 함수는 일반적인 함수로서 호출할 수 있는 것은 물론 생성자 함수로서 호출할 수 있다.

생성자 함수로서 호출한다는 것은 `new` 연산자와 함께 호출하여 객체를 생성하는 것을 의미한다.

<br>

함수는 객체기 때문에 일반 객체와 동일하게 동작할 수 있다.

함수 객체는 일반 객체가 가지고 있는 내부 슬롯과 내부 메서드를 모두 가지고 있기 때문이다.

<br>

```js
// 함수는 객체이다.
function foo() {}

// 함수는 객체이므로 프로퍼티를 소유할 수 있다.
foo.prop = 10;

// 함수는 객체이므로 메서드를 소유할 수 있다.
foo.method = function () {
    console.log(this.prop);
};

foo.method();   // 10
```

함수는 객체지만 일반 객체와는 다르다.

일반 객체는 호출할 수 없지만, 함수는 호출할 수 있다.

<br>

따라서 함수는 일반 객체가 가지고 있는 내부 슬롯과 내부 메서드는 물론, 함수로서 동작하기 위해 함수 객체만을 위한 `[[Environment]]`, `[[FormalParameter]]` 등의 내부 슬롯과 [[Call]], [[Construct]] 같은 내부 메서드를 가지고 있다.

<br>

- `[[Call]]`: 함수가 일반 함수로서 호출되면 호출된다.
- `[[Construct]]`: new 연산자와 함께 생성자 함수로서 호출되면 호출된다.

```js
function foo() {}

// 일반적인 함수로서 호출: [[Call]] 호출
foo();

// 생성자 함수로서 호출: [[Construct]] 호출
new foo();
```

<br>

- **callable**: 내부 메서드 `[[Call]]`을 갖는 함수 객체로, 함수를 말한다.
- **construct**: 내부 메서드 `[[Construct]]`를 갖는 함수 객체로, 생성자 함수로서 호출할 수 있는 함수를 말한다.
- **non-constructor**: `[[Construct]]`를 갖지 않는 함수 객체로, 객체를 생성자 함수로서 호출할 수 없는 함수를 말한다.

### constructor와 non-constructor의 구분
자바스크립트 엔진은 함수 정의를 평가하여 함수 객체를 생성할 때 함수 정의 방식에 따라 `constructor`과 `non-constructor`를 구분한다.
- **constructor**: 함수 선언문, 함수 표현식, 클래스(클래스도 함수다!)
- **non-constructor**: 메서드(ES6 메서드 축약 표현), 화살표 함수

<br>

```js
// 일반 함수 정의: 함수 선언문, 함수 표현식
function foo() {}
const bar = function () {};
// 프로퍼티 x의 값으로 할당된 것은 일반 함수로 정의된 함수다. 이는 메서드로 인정하지 않는다.
const baz = {
    x: function () {}
};

// 일반 함수로 정의된 함수만이 constructor다.
new foo();  // -> foo {}
new bar();  // -> bar {}
new baz.x();    // x {}

// 화살표 함수 정의
const arrow = () => {};

new arrow();    // TypeError: arrow is not a constructor

// 메서드 정의: ES6의 메서드 축약 표현만 메서드로 인정한다.
const obj = {
    x() {}
};

new obj.x();    // TypeError: obj.x is not a constructor
```

<br>

함수를 프로퍼티 값으로 사용하면 이를 메서드라고 부른다.

하지만 ECMAScript 사양에서 메서드란 ES6의 메서드 축약 표현만을 의미한다.

즉, 함수가 어디에 할당되어 있는지에 따라 메서드인지를 판단하는 것이 아니라 함수 정의 방식에 따라 `constructor`와 `non-constructor`를 구분한다.

<br>

```js
function foo() {}

// 일반 함수로서 호출
// [[Call]]이 호출된다. 모든 함수 객체는 [[Call]]이 구현되어 있다.
foo();

// 생성자 함수로서 호출
// [[Constructor]]가 호출된다. 이때 [[Constructor]]를 갖지 않는다면 에러가 발생한다.
new foo();
```

### new 연산자
일반 함수와 생성자 함수의 특별한 형식적 차이는 없다.

new 연산자와 함께 함수를 호출하면 해당 함수는 생성자 함수로 동작한다.

즉, 함수 객체의 내부 메서드 `[[Call]]`이 호출되는 것이 아니라 `[[Constructor]]`가 호출된다.

단, new 연산자와 함께 호출하는 함수는 `non-constructor`가 아닌 `constructor`이어야 한다.

<br>

```js
// 생성자 함수로서 정의하지 않은 일반 함수
function add(x, y) {
    return x + y;
}

// 생성자 함수로서 정의하지 않은 일반 함수를 new 연산자와 함께 호출
let inst = new add();

// 함수가 객체를 반환하지 않았기 때문에 반환문이 무시된다.
// 따라서 빈 객체가 생성되어 반환된다.
console.log(inst);  // {}

// 객체를 반환하는 일반 함수
function createUser(name, role) {
    return { name, role };
}
```
<br>

```js
// 객체를 반환하는 일반 함수
function createUser(name, role) {
    return { name, role };
}

// 일반 함수를 new 연산자와 함께 호출
inst = new createUser('lee', 'admin');
// 함수가 생성한 객체를 반환한다.
console.log(inst);  // {name: "lee", role: "admin"}
```

<br>

반대로 new 연산자 없이 생성자 함수를 호출하면 일반 함수로 호출된다.

즉, 함수 객체의 내부 메서드 `[[Construct]]`가 호출되는 것이 아니라 `[[Call]]`이 호출된다.

```js
// 생성자 함수
function Circle(radius) {
    this.radius = radius;
    this.getDiameter = function () {
        return 2 * this.radius;
    };
}

// new 연산자 없이 생성자 함수 호출하면 일반 함수로서 호출된다.
const circle = Circle(5);

console.log(circle);    // undefined

// 일반 함수 내부의 this는 전역 객체 window를 가리킨다.
console.log(radius);    // 5
console.log(getDiameter()); // 10

circle.getDiameter();
// TypeError: Cannot read property 'getDiameter' of undefined
```

### new.target
생성자 함수가 new 연산자 없이 호출되는 것을 방지하기 위해 파스칼 케이스 컨벤션을 사용한다 하더라도 실수는 언제나 발생할 수 있다.

때문에 ES6에서는 `new.target`을 지원한다.

<br>

`new.target`은 `this`와 유사하게 `constructor`인 모든 함수 내부에서 암묵적인 지역 변수와 같이 사용되며 `메타 프로퍼티`라고 부른다.

<br>

함수 내부에서 `new.target`을 사용하면 new 연산자와 함께 생성자 함수로서 호출되었는지 알 수 있다.

new 연산자와 함께 생성자 함수로서 호출되면 함수 내부의 `new.target`은 함수 자신을 가리킨다.

new 연산자 없이 일반 함수로서 호출된 함수 내부의 `new.target`은 undefined이다.

<br>

```js
// 생성자 함수
function Circle(radius) {
    // 이 함수가 new 연산자와 함께 호출되지 않았다면 new.target은 undefined다.
    if (!new.target) {
        // new 연산자와 함께 생성자 함수를 재귀 호출하여 생성된 인스턴스를 반환한다.
        return new Circle(radius);
    }
    this.radius = radius;
    this.getDiameter = function () {
        return 2 * this.radius;
    };
}

// new 연산자 없이 생성자 함수를 호출하여도 new.target을 통해 생성자 함수로서 호출된다.
const circle = Circle(5);
console.log(circle.getDiameter());
```

<br>

대부분의 빌트인 생성자 함수는 new 연산자와 함께 호출되었는지를 확인한 후 적절한 값을 반환한다.

예를 들어, Object와 Function 생성자 함수는 new 연산자 없이 호출해도 new 연산자와 함께 호출했을 때와 동일하게 동작한다.

