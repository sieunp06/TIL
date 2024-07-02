### 목차
  - [this 키워드](#this-키워드)
  - [함수 호출 방식과 this 바인딩](#함수-호출-방식과-this-바인딩)
    - [1. 일반 함수 호출](#1-일반-함수-호출)
    - [2. 메서드 호출](#2-메서드-호출)
    - [3. 생성자 함수 호출](#3-생성자-함수-호출)
    - [4. Function.prototype.apply/call/bind 메서드에 의한 간접 호출](#4-functionprototypeapplycallbind-메서드에-의한-간접-호출)


---
> 모던 자바스크립트 Deep Dive

<br>

# 22장 this
## this 키워드
**객체**는 상태를 나타내는 프로퍼티와 동작을 나타내는 메서드를 하나의 논리적인 단위로 묶은 복합적인 자료구조다.

<br>

동작을 나타내는 메서드는 자신이 속한 객체의 상태, 즉 프로퍼티를 참조하고 변경할 수 있어야 한다.

이때 메서드가 자신이 속한 객체의 프로퍼티를 참조하려면 먼저 자신이 **속한 객체를 가리키는 식별자를 참조할 수 있어야 한다.**

객체 리터럴 방식으로 생성한 객체의 경우 메서드 내부에서 메서드 자신이 속한 객체를 가리키는 식별자를 재귀적으로 참조할 수 있다.

```js
const circle = {
    // 프로퍼티: 객체 고유의 상태 데이터
    radius: 5,
    // 메서드: 상태 데이터를 참조하고 조작하는 동작
    getDiameter() {
        // 이 메서드가 자신이 속한 객체의 프로퍼티나 다른 메서드를 참조하려면 
        // 자신이 속한 객체인 circle을 참조할 수 있어야 한다.
        return 2 * circle.radius;
    }
};

console.log(circle.getDiameter());  // 10
```

위 예시의 `getDiameter` 메서드 내에서 메서드 자신이 속한 객체를 가리키는 식별자 `circle`을 참조하고 있다.

이 참조 표현식이 평가되는 시점은 `getDiameter` 메서드가 호출되어 함수 몸체가 실행되는 시점이다.

<br>

위 예제에서 객체 리터럴은 `circle` 변수가 할당되기 직전에 평가된다.

따라서 getDiameter 메서드가 호출되는 시점에는 이미 객체 리터럴의 평가가 완료되어 객체가 생성되었고, `circle` 식별자에 생성된 객체가 할당된 이후다.

따라서 내부에서 `circle` 식별자를 참조할 수 있다.

<br>

하지만 자기 자신이 속한 객체를 재귀적으로 참조하는 방식은 일반적이지 않으며 바람직하지도 않다.

```js
function Circle(radius) {
    // 이 시점에는 생성자 함수 자신이 생성할 인스턴스를가리키는 식별자를 알 수 없다.
    ????.radius = radius;
}

Circle.prototype.getDiameter = function () {
    // 이 시점에는 생성자 함수 자신이 생성할 인스턴스를 가리키는 식별자를 알 수 없다.
    return 2 * ????.radius;
};

// 생성자 함수로 인스턴스를 생성하려면 먼저 생성자 함수를 정읳야한다.
const circle = new Circle(5);
```

생성자 함수 내부에서는 프로퍼티 또는 메서드를 추가하기 위해 자신이 생성할 인스턴스를 참조할 수 있어야 한다.

하지만, 생성자 함수에 의한 객체 생성 방식은 먼저 생성자 함수를 정의한 이후 new 연산자와 함께 생성자 함수를 호출하는 단계가 추가로 필요하다.

즉, 생성자 함수로 인스턴스를 생성하려면 먼저 생성자 함수가 존재해야 한다.

<br>

생성자 함수를 정의하는 시점에는 아직 인스턴스를 생성하기 이전이기 때문에 생성자 함수가 생성할 인스턴스를 가리키는 식별자를 알 수 없다.

따라서 자신이 속한 객체 또는 자신이 생성할 인스턴스를 가리키는 특수한 식별자가 필요한데, 자바스크립트는 `this` 식별자를 제공한다.

<br>

> 💡 **this 식별자**<br>
> 자신이 속한 객체 또는 자신이 생성할 인스턴스를 가리키는 자기 참조 변수로, this를 통해 자신이 속한 객체 또는 자신이 생성할 인스턴스의 프로퍼티나 메서드를 참조할 수 있다.

this는 자바스크립트 엔진에 의해 암묵적으로 생성되며, 코드 어디서든 참조할 수 있다.

함수를 호출하면 arguments 객체와 this가 암묵적으로 함수 내부에 전달된다.

이때 arguments와 this는 지역 변수처럼 사용할 수 있다.

<br>

단, this가 가리키는 값, 즉 this 바인딩은 함수 호출 방식에 의해 동적으로 결정된다.

```js
// 객체 리터럴
const circle = {
    radius: 5,
    getDiameter() {
        // this는 메서드를 호출한 객체를 가리킨다.
        return 2 * this.radius;
    }
};

console.log(circle.getDiameter());  // 10
```
```js
// 생성자 함수
function Circle(radius) {
    // this는 생성자 함수가 생성할 인스턴스를 가리킨다.
    this.radius = radius;
}

Circle.prototype.getDiameter = function () {
    // this는 생성자 함수가 생성할 인스턴스를 가리킨다.
    return 2 * this.radius;
};

// 인스턴스 생성
const circle = new Circle(5);
console.log(circle.getDiameter());  // 10
```

생성자 함수 내부의 this는 생성자 함수가 생성할 인스턴스를 가리킨다.

<br>

자바스크립트의 `this`는 함수가 호출되는 방식에 따라 this에 바인딩될 값, 즉 this 바인딩이 동적으로 결정된다.

또한 strict mode 역시 this 바인딩에 영향을 준다.

<br>

`this`는 코드 어디에서든 참조 가능하다.

```js
// this는 어디서든지 참조 가능하다.
// 전역에서 this는 전역 객체 window를 가리킨다.
console.log(this);  // window

function square(number) {
    // 일반 함수 내부에서 this는 전역 객체 window를 가리킨다.
    console.log(this);  // window
    return number * number;
}
square(2);
```
```js
const person = {
    name: 'Lee',
    getName() {
        // 메서드 내부에서 this는 메서드를 호출한 객체를 가리킨다.
        console.log(this);  // {name: 'lee', getName: f}
        return this.name;
    }
};
console.log(person.getName());  // lee
```
```js
function Person(name) {
    this.name = name;
    // 생성자 함수 내부에서 this는 생성자 함수가 생성할 인스턴스를 가리킨다.
    console.log(this);  // Person {name: 'Lee'}
}
const me = new Person('lee');
```

<br>

`this`는 객체의 프로퍼티나 메서드를 참조하기 위한 자기 참조 변수이므로 일반적으로 객체의 메서드 내부 또는 생성자 함수 내부에서만 의미가 있다.

따라서 strict mode가 적용된 일반 함수 내부의 this에서는 undefined가 바인딩된다.

일반 함수 내부에서 this를 사용할 필요가 없기 때문이다.

## 함수 호출 방식과 this 바인딩
`this` 바인딩(this에 바인딩될 값)은 함수 호출 방식, 즉 함수가 어떻게 호출되었는지에 따라 동적으로 결정된다.

> ⭐ **렉시컬 스코프와 this 바인딩은 결정 시기가 다르다.**<br>
> 함수의 상위 스코프를 결정하는 방식인 렉시컬 스코프는 함수 정의가 평가되어 함수 객체가 생성되는 시점에 상위 스코프를 결정한다.<br>
> 하지만 this 바인딩은 함수 호출 시점에 결정된다.

<br>

하지만 동일한 함수도 다양한 방식으로 호출할 수 있다.

이때 함수를 호출하는 방식은 다음과 같다.

1. 일반 함수 호출
2. 메서드 호출
3. 생성자 함수 호출
4. Function.prototype.apply/call/bind 메서드에 의한 간접 호출

<br>

### 1. 일반 함수 호출
일반 함수 호출은 다음과 같은 형태를 가진다.

```js
const foo = function () {
    console.dir(this);
};

foo();
```

<br>

기본적으로 this에는 **전역 객체가 바인딩**된다.

```js
function foo() {
    console.log("foo's this: ", this);  // window
    function bar() {
        console.log("bar's this: ", this);  // window
    }
    bar();
}
foo();
```

위 예제를 보면 전역 함수와 중첩 함수를 **일반 함수로 호출하면 함수 내부의 this에는 전역 객체가 바인딩**된다.

다만 `this`는 객체의 프로퍼티나 메서드를 참조하기 위한 자기 참조 변수이므로 객체를 생성하지 않는 일반 함수에서 this는 의미가 없다.

<br>

따라서 다음 예제처럼 strict mode가 적용된 일반 함수 내부의 this에는 undefined가 바인딩된다.

```js
function foo() {
    'use strict';

    console.log("foo's this:", this);   // undefined
    function bar() {
        console.log("bar's this:", this);   // undefined
    }
    bar();
}
foo();
```

메서드 내에서 정의한 중첩 함수도 일반함수로 호출되면 중첩 함수 내부의 this에는 전역 객체가 바인딩된다.

<br>

콜백 함수가 일반 함수로 호출된다면 콜백 함수 내부의 this에도 전역 객체가 바인딩된다.

어떤 함수라도 일반 함수로 호출되면 this에 전역 객체가 바인딩된다.

```js
var value = 1;

const obj = {
    value: 100,
    foo() {
        console.log("foo's this: ", this);  // {value: 100, foo: f}
        setTimeout(function () {
            console.log("callback's this: ", this); // window
            console.log("callback's this.value: ", this.value); // 1
        }, 100);
    }
};

obj.foo();
```

위처럼 일반 함수로 호출된 모든 함수(중첩 함수, 콜백 함수 포함) 내부의 this에는 전역 객체가 바인딩된다.

때문에 `setTimeout` 함수에 전달된 콜백 함수의 this에는 전역 객체가 바인딩된다.

따라서 `this.value`는 obj 객체의 value 프로퍼티가 아닌 전역 객체의 `value` 프로퍼티, 즉 `window.value`를 참조한다.

<br>

`var` 키워드로 선언한 전역 변수는 전역 객체의 프로퍼티가 되므로 `window.value`는 1이다.

<br>

메서드 내부의 중첩 함수나 콜백 함수의 this 바인딩을 메서드의 this 바인딩과 일치시키기 위한 방법은 다음과 같다.

```js
var value = 1;

const obj = {
    value: 100, 
    foo() {
        // this 바인딩(obj)을 변소 that에 할당한다.
        const that = this;

        // 콜백 함수 내부에서 this 대신 that을 참조한다.
        setTimeout(function () {
            console.log(that.value);    // 100
        }, 100);
    }
};

obj.foo();
```

<br>

화살표 함수를 사용해 this 바인딩을 일치시킬 수도 있다.

```js
var value = 1;

const obj = {
    value: 100,
    foo() {
        setTimeout(() => console.log(this.value), 100); // 100
    }
};

obj.foo();
```

> ⭐ 자세한 내용은 [26.3장 화살표 함수]()를 확인하면 된다.

### 2. 메서드 호출
메서드를 호출하는 방법은 다음과 같은 형태를 가진다.

```js
const foo = function () {
    console.dir(this);
};

const obj = { foo };
obj.foo();  // obj
```

<br>

메서드 내부의 this에는 메서드를 호출한 객체, 즉 메서드를 호출할 때 메서드 이름 앞의 마침표(.) 연산자 앞에 기술한 객체가 바인딩된다.

이때 메서드 내부의 this는 메서드를 소유한 객체가 아닌 메서드를 호출한 객체에 바인딩된다.

```js
const person = {
    name: 'Lee',
    getName() {
        // 메서드 내부의 this는 메서드를 호출한 객체에 바인딩된다.
        return this.name;
    }
};

// 메서드 getName을 호출한 객체는 person이다.
console.log(person.getName());  // Lee
```

위 예제에서 `getName` 메서드는 `person` 객체의 메서드로 정의되었다.

이때 메서드는 프로퍼티에 바인딩된 함수다.

즉, `person` 객체의 `getName` 프로퍼티가 가리키는 함수 객체는 `person` 객체에 포함된 것이 아니라 독립적으로 존재하는 별도의 객체이다.

`getName` 프로퍼티가 함수 객체를 가리키고 있을 뿐이다.

<br>

때문에 `getName` 프로퍼티가 가리키는 함수 객체, 즉 `getName` 메서드는 다른 객체의 프로퍼티에 할당하는 것으로 다른 객체의 메서드가 될 수도 있고 일반 변수에 할당하여 일반 함수로 호출될 수도 있다.

```js
const anotherPerson = {
    name: 'Kim'
};

// getName 메서드를 anotherPerson 객체의 메서드로 할당
anotherPerson.getName = person.getName;

// getName 메서드를 호출한 객체는 anotherPerson이다.
console.log(anotherPerson.getName());   // Kim

// getName 메서드를 변수에 할당
const getName = person.getName;

// getName 메서드를 일반 함수로 호출
console.log(getName()); // ''
```

따라서 메서드 내부의 this는 프로퍼티로 메서드를 가지고 있는 객체와 관계가 없고 메서드를 호출한 객체에 바인딩된다.

<br>

```js
function Person(name) {
    this.name = name;
}

Person.prototype.getName = function () {
    return this.name;
};

const me = new Person('lee');

// getName 메서드를 호출한 객체는 me다.
console.log(me.getName());  // lee

Person.prototype.name = 'Kim'; 

// getName 메서드를 호출한 객체는 Person.prototype이다.
console.log(Person.prototype.getName());    // Kim
```

### 3. 생성자 함수 호출
생성자를 통해 함수를 호출하는 방법은 다음과 같은 형태를 가진다.

```js
const foo = function () {
    console.dir(this);
};

new foo();  // foo {}
```

<br>

생성자 함수 내부의 this에는 생성자 함수가 미래에 생성할 인스턴스가 바인딩된다.

```js
// 생성자 함수
function Circle(radius) {
    // 생성자 함수 내부의 this는 생성자 함수가 생성할 인스턴스를 가리킨다.
    this.radius = radius;
    this.getDiameter = function () {
        return 2 * this.radius;
    };
}

// 반지름인 5인 Circle 객체를 생성
const circle1 = new Circle(5);

// 반지름인 10인 Circle 객체를 생성
const circle2 = new Circle(10);

console.log(circle1.getDiameter()); // 10
console.log(circle2.getDiameter()); // 20
```

만약 new 연산자와 함께 생성자 함수를 호출하지 않으면 생성자 함수가 아니라 일반 함수로 동작한다.

```js
// new 연산자와 함께 호출하지 않으면 생성자 함수로 동작하지 않는다.
// 즉, 일반적인 함수의 호출이다.
const circle3 = Circle(15);

// 일반 함수로 호출된 Circle에는 반환문이 없으므로 암묵적으로 undefined를 반환한다.
console.log(circle3);   // undefined

// 일반 함수로 호출된 Circle 내부의 this는 전역 객체를 가리킨다.
console.log(radius);    // 15
```

### 4. Function.prototype.apply/call/bind 메서드에 의한 간접 호출
`Function.prototype.apply/call/bind` 메서드에 의한 간접 호출은 다음과 같은 형태를 가진다.

```js
const bar = { name: 'bar' };

foo.call(bar);  // bar
foo.apply(bar); // bar
foo.bind(bar);  // bar
```

<br>

`apply`, `call`, `bind` 메서드는 Functional.prototype의 메서드다.

즉, 이들 메서드는 모든 함수가 상속받아 사용할 수 있다.

<br>

#### 📌 Function.prototype.apply와 Function.prototype.call
`Function.prototype.apply` 메서드와 `Function.prototype.call`는 this로 사용할 객체와 인수 리스트를 인수로 전달받아 호출한다.

```js
/**
 * 주어진 this 바인딩과 인수 리스트 배열을 사용하여 함수를 호출한다.
 * @param thisArg - this로 사용할 객체
 * @param argsArray - 함수에게 전달할 인수 리스트의 배열 또는 유사 배열 객체
 * @returns 호출된 함수의 반환값
 */
Function.prototype.apply(thisArg[, argsArray])
```
```js
/**
 * 주어진 this 바인딩과 ,로 구분된 인수 리스트를 사용하여 함수를 호출한다.
 * @param thisArg - this로 사용할 객체
 * @param arg1, arg2, ... - 함수에게 전달할 인수 리스트
 * @returns 호출된 함수의 반환값
 */
Function.prototype.call(thisArg[, arg1[, arg2[, ...]]])
```

<br>

```js
function getThisBinding() {
    return this;
}

// this로 사용할 객체
const thisArg = { a: 1 };

console.log(getThisBinding());  // window

// getThisBinding 함수를 호출하면서 인수로 전달된 객체를 getThisBinding 함수의 this에 바인딩한다.
console.log(getThisBinding.apply(thisArg)); // { a: 1 }
console.log(getThisBinding.call(thisArg));  // { a: 1 }
```
`apply`와 `call` 메서드의 본질적인 기능은 함수를 호출하는 것이다.

이 함수들은 함수를 호출하면서 첫 번째 인수로 전달한 특정 객체를 호출한 함수의 this에 바인딩한다.

<br>

`apply`와 `call` 메서드는 호출할 함수에 인수를 전달하는 방식만 다를 뿐 동일하게 동작한다.

위 예제는 호출할 함수, 즉 `getThisBinding` 함수에 인수를 전달하지 않는다.

