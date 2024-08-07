### 목차
  - [클래스는 프로토타입의 문법적 설탕인가?](#클래스는-프로토타입의-문법적-설탕인가)
    - [클래스와 생성자 함수의 차이점](#클래스와-생성자-함수의-차이점)
  - [클래스 정의](#클래스-정의)
  - [클래스 호이스팅](#클래스-호이스팅)
  - [인스턴스 생성](#인스턴스-생성)
  - [메서드](#메서드)
    - [constructor](#constructor)
    - [프로토타입 메서드](#프로토타입-메서드)
    - [정적 메서드](#정적-메서드)
    - [정적 메서드와 프로토타입 메서드의 차이](#정적-메서드와-프로토타입-메서드의-차이)
    - [클래스에서 정의한 메서드의 특징](#클래스에서-정의한-메서드의-특징)
  - [클래스의 인스턴스 생성 과정](#클래스의-인스턴스-생성-과정)
    - [1. 인스턴스 생성과 this 바인딩](#1-인스턴스-생성과-this-바인딩)
    - [2. 인스턴스 초기화](#2-인스턴스-초기화)
    - [3. 인스턴스 반환](#3-인스턴스-반환)
  - [프로퍼티](#프로퍼티)
    - [인스턴스 프로퍼티](#인스턴스-프로퍼티)
    - [접근자 프로퍼티](#접근자-프로퍼티)
    - [클래스 필드 정의 제안](#클래스-필드-정의-제안)
    - [private 필드 정의 제안](#private-필드-정의-제안)
    - [static 필드 정의 제안](#static-필드-정의-제안)
  - [상속에 의한 클래스 확장](#상속에-의한-클래스-확장)
    - [클래스 상속과 생성자 함수 상속](#클래스-상속과-생성자-함수-상속)
    - [extends 키워드](#extends-키워드)
    - [동적 상속](#동적-상속)
    - [서브클래스의 constructor](#서브클래스의-constructor)
    - [super 키워드](#super-키워드)

---
> 모던 자바스크립트 Deep Dive

<br>

# 25장 클래스
## 클래스는 프로토타입의 문법적 설탕인가?
자바스크립트는 프로토타입 기반 객체지향 언어이다.

프로토타입 기반 객체지향 언어는 클래스가 필요 없는 객체지향 프로그래밍 언어다.

ES5에서는 클래스 없이도 다음과 같이 생성자 함수와 프로토타입을 통해 객체지향 언어의 상속을 구현할 수 있다.

<br>

```js
// ES5 생성자 함수
var Person = (function () {
    // 생성자 함수
    function Person(name) {
        this.name = name;
    }

    // 프로토타입 메서드
    Person.prototype.sayHi = function () {
        console.log('Hi! My name is ' + this.name);
    };

    // 생성자 함수 반환
    return Person;
} ());

// 인스턴스 생성
var me = new Person('lee');
me.sayHi(); // Hi! My name is lee
```

ES6의 클래스가 기존의 프로토타입 기반 객체지향 모델을 폐지하고 새롭게 클래스 기반 객체지향 모델을 제공하는 것이 아니다.

사실 클래스는 함수이며, 기존 프로토타입 기반 패턴을 클래스 기반 패턴처럼 사용할 수 있도록 하는 **문법적 설탕**이라고 볼 수 있다.

<br>

단, 클래스와 생성자 함수는 모두 프로토타입 기반의 인스턴스를 생성하지만 정확히 동일하게 동작하지는 않는다.

클래스는 생성자 함수보다 엄격하며 생성자 함수에서는 제공하지 않는 기능도 제공한다.

<br>

### 클래스와 생성자 함수의 차이점

클래스는 생성자 함수와 매우 유사하게 동작하지만 다음과 같이 몇 가지 차이가 있다.

1. 클래스를 new 연산자 없이 호출하면 에러가 발생한다. 하지만 생성자 함수를 new 연산자 없이 호출하면 일반 함수로서 호출된다.
2. 클래스는 상속을 지원하는 extends와 super 키워드를 제공한다. 하지만 생성자 함수는 extends와 super 키워드를 제공하지 않는다.
3. 클래스는 호이스팅이 발생하지 않는 것처럼 동작한다. 하지만 함수 선언문으로 정의된 생성자 함수는 함수 호이스팅이, 함수 표현식으로 정의한 생성자 함수는 변수 호이스팅이 발생한다.
4. 클래스 내의 모든 코드에는 암묵적으로 strict mode가 지정되어 실행되며 strict mode를 해제할 수 없다. 하지만 생성자 함수는 암묵적으로 strict mode가 지정되지 않는다.
5. 클래스의 constructor, 프로토타입 메서드, 정적 메서드는 모두 프로퍼티 어트리뷰트 [[Enumerable]] 값이 false다. 다시 말해, 열거되지 않는다.

<br>

생성자 함수와 클래스는 프로토타입 기반의 객체지향을 구현했다는 점에서 매우 유사하다.

하지만 클래스는 생성자 함수 기반의 객체 생성 방식보다 견고하고 명료하다.

특히 클래스의 `extends`와 `super` 키워드는 상속 관계 구현을 더욱 간결하고 명료하게 한다.

## 클래스 정의
클래스는 `class` 키워드를 사용하여 정의한다.

```js
// 클래스 선언문
class Person {}
```

<br>

일반적이지는 않지만 함수와 마찬가지로 표현식으로 클래스를 정의할수도 있다.

이때 클래스는 함수와 마찬가지로 이름을 가질 수도 있고, 갖지 않을 수도 있다.

```js
// 익명 클래스 표현식
const Person = class {};

// 기명 클래스 표현식
const Person = class MyClass {};
```

<br>

클래스를 표현식으로 정의할 수 있다는 것은 클래스가 값으로 사용할 수 있는 **일급 객체**라는 것을 의미한다.

클래스는 함수다. 따라서 클래스는 값처럼 사용할 수 있는 일급 객체다.

<br>

클래스 몸체에는 0개 이상의 메서드만 정의할 수 있다.

클래스 몸체에서 정의할 수 있는 메서드는 다음과 같다.
- **constructor(생성자)**
- **프로토타입 메서드**
- **정적 메서드**

<br>

```js
// 클래스 선언문
class Person {
    // 생성자
    constructor(name) {
        // 인스턴스 생성 및 초기화
        this.name = name;   // name 프로퍼티는 public이다.
    }

    // 프로토타입 메서드
    sayHi() {
        console.log(`Hi! My name is ${this.name}`);
    }

    // 정적 메서드
    static sayHello() {
        console.log('Hello!');
    }
}

// 인스턴스 생성
const me = new person('lee');

// 인스턴스의 프로퍼티 참조
console.log(me.name);   // lee
// 프로토타입 메서드 호출
me.sayHi(); // Hi! My name is lee
// 정적 메서드 호출
Person.sayHello();  // Hello!
```

## 클래스 호이스팅
클래스는 함수로 평가된다.

```js
// 클래스 선언문
class Person {}

console.log(typeof Person); // function
```

클래스 선언문으로 정의된 클래스는 함수 선언문과 같이 소스코드 평과 과정, 즉 런타임 이전에 먼저 평가되어 함수 객체를 생성한다.

이때 클래스가 평가되어 생성된 함수 객체는 생성자 함수로서 호출할 수 있는 함수, 즉 constructro다.

생성자 함수로서 호출할 수 있는 함수는 함수 정의가 평가되어 함수 객체를 생성하는 시점에 프로토타입도 더불어 생성된다.

프로토타입과 생성자 함수는 단독으로 존재할 수 없고 언제나 쌍으로 존재하기 때문이다.

<br>

단, 클래스는 클래스 정의 이전에 참조할 수 없다.

```js
console.log(Person);    // ReferencesError

// 클래스 선언문
class Person {}
```

클래스 선언문은 마치 호이스팅이 발생하지 않는 것처럼 보이나 그렇지 않다.

```js
const Person = '';

{
    // 호이스팅이 발생하지 않는다면 ''이 출력되어야 한다.
    console.log(Person);
    // ReferenceError

    // 클래스 선언문
    class Person {}
}
```

클래스 선언문도 변수 선언, 함수 정의와 마찬가지로 호이스팅이 발생한다.

단, 클래스는 `let`, `const` 키워드로 선언한 변수처럼 호이스팅된다.

따라서 클래스 선언문 이전에 일시적 사각지대에 빠지게 되기 때문에 호이스팅이 발생하지 않는 것처럼 동작한다.

<br>

`var`, `let`, `const`, `function`, `function*`, `class` 키워드를 사용하여 선언된 모든 식별자는 호이스팅된다.

모든 선언문은 런타임 이전에 먼저 실행되기 때문이다.

## 인스턴스 생성
클래스는 생성자 함수이며 `new` 연산자와 함께 호출되어 인스턴스를 생성한다.

```js
class Person {}

// 인스턴스 생성
const me = new Person();
console.log(me);    // Person {}
```

함수는 new 연산자의 사용 여부에 따라 일반 함수로 호출되거나 인스턴스 생성을 위한 생성자 함수로 호출되지만, 클래스는 인스턴스를 생성하는 것이 유일한 존재 이유이므로 반드시 new 연산자와 함께 호출해야 한다.

```js
class Person {}

// 클래스를 new 연산자 없이 호출하면 타입 에러가 발생한다.
const me = Person();
// TypeError
```

<br>

클래스 표현식으로 정의된 클래스의 경우 아래 예제와 같이 클래스를 가리키는 식별자(Person)를 이용해 인스턴스를 생성하지 않고 기명 클래스 표현식의 클래스 이름(MyClass)을 사용해 인스턴스를 생성하면 에러가 발생한다.

```js
const Person = class MyClass {};

// 함수 표현식과 마찬가지로 클래스를 가리키는 식별자로 인스턴스를 생성해야 한다.
const me = new person();

// 클래스 이름 MyClass는 함수와 동일하게 클래스 몸체 내부에서만 유효한 식별자다.
console.log(MyClass);   // ReferencesError

const you = new Person();   // ReferencesError
```

이는 기명 함수 표현식과 마찬가지로 클래스 표현식에서 사용한 클래스 이름은 외부 코드에서 접근 불가능하기 때문이다.

## 메서드
클래스의 몸체에서는 0개 이상의 메서드만 선언할 수 있다.

클래스의 몸체에서 정의할 수 있는 메서드는 다음과 같다.
- constructor(생성자)
- 프로토타입 메서드
- 정적 메서드

<br>

### constructor
> 💡 **constructor**<br>
> 인스턴스를 생성하고 초기화하기 위한 특수한 메서드다.

이때 constructor는 이름을 변경할 수 없다.

```js
class Person {
    // 생성자
    constructor(name) {
        // 인스턴스 생성 및 초기화
        this.name = name;
    }
}
```

<br>

클래스는 평가되어 함수 객체가 된다.

때문에 클래스도 함수 객체 고유의 프로퍼티를 모두 갖고 있다.

함수와 동일한게 프로토타입과 연결되어 있으며, 자신의 스코프체인을 구성한다.

<br>

모든 함수 객체가 가지고 있는 prototype 프로퍼티가 가리키는 프로토타입 객체의 constructor 프로퍼티는 클래스 자신을 가리키고 있다.

이는 클래스가 인스턴스를 생성하는 생성자 함수라는 것을 의미한다.

즉, new 연산자와 함께 클래스를 호출하면 클래스는 인스턴스를 생성한다.

<br>

이때 construct는 메서드로 해석되는 것이 아니라 클래스가 평가되어 생성한 함수 객체 코드의 일부가 된다.

다시 말해, 클래스 정의가 평가되면 constructor의 기술된 동작을 하는 함수 객체가 생성된다.

<br>

#### 📌 constructor과 생성자 함수
constructor과 생성자 함수는 유사하지만 몇 가지 차이점이 있다.

<br>

constructor는 클래스 내에 최대 한 개만 존재할 수 있다.

만약 클래스가 2개 이상의 constructor를 포함하면 문법 에러가 발생한다.

```js
class Person {
    constructor() {}
    constructor() {}
}
// SyntaxError
```

또한 constructor는 생략할 수 있다.

```js
class Person {}
```

이때 constructor를 생략하면 다음과 같은 빈 constructor가 암묵적으로 정의된다.

constructor를 생략한 빈 constructor에 의해 빈 객체를 생성한다.

```js
class Person {
    // constructor는 생략하면 아래와 같이 빈 constructor가 암묵적으로 정의된다.
    constructor() {}
}

// 빈 객체가 생성된다.
const me = new Person();
console.log(me);    // Person {}
```

<br>

프로퍼티가 추가되어 초기화된 인스턴스를 생성하려면 constructor 내부에서 this에 인스턴스 프로퍼티를 추가한다.

```js
class Person {
    constructor() {
        // 고정값으로 인스턴스 초기화
        this.name = 'lee';
        this.address = 'Seoul';
    }
}

// 인스턴스 프로퍼티가 추가된다.
const me = new Person();
console.log(me);    // Person {name: 'Lee', address: 'Seoul'}
```

인스턴스를 생성할 때 클래스 외부에서 인스턴스 프로퍼티 초기화값을 전달하려면 다음과 같이 constructor에 매개변수를 선언하고 인스턴스를 생성할 때 초기값을 전달한다.

이때 초기값은 constructor의 매개변수에게 전달된다.

```js
class Person {
    constructor(name, address) {
        this.name = name;
        this.address = address;
    }
}

// 인수로 초기값을 전달한다. 초기값은 constructot에 전달된다.
const me = new Person('lee', 'Seoul');
console.log(me);    // Person {name: "lee", address: "Seoul"}
```

위 예시처럼 constructor 내에서는 인스턴스의 생성과 동시에 인스턴스 프로퍼티 추가를 통해 인스턴스의 초기화를 실행한다.

따라서 인스턴스를 초기화하려면 constructor를 생략해서는 안된다.

<br>

또한 constructor는 별도의 반환문을 갖지 않아야 한다.

new 연산자와 함께 클래스가 호출되면 생성자 함수와 동일하게 암묵적으로 this, 즉 인스턴스를 반환하기 때문이다.

만약 this가 아닌 다른 객체를 명시적으로 반환하면 this, 즉 인스턴스가 반환되지 못하고 return 문에 명시한 객체가 반환된다.

```js
class Person {
    constructor(name) {
        this.name = name;

        // 명시적으로 객체를 반환하면 암묵적인 this 반환이 무시된다.
        return {};
    }
}

// constructor에서 명시적으로 반환한 빈 객체가 반환된다.
const me = new Person('Lee');
console.log(me);    // {}
```

하지만 명시적으로 원시값을 반환하면 원시값 반환은 무시되고 암묵적으로 this가 반환된다.

### 프로토타입 메서드
생성자 함수를 사용하여 인스턴스를 생성하는 경우 프로토타입 메서드를 생성하기 위해서는 다음과 같이 명시적으로 프로토타입에 메서드를 추가해야 한다.

```js
// 생성자 함수
function Person(name) {
    this.name = name;
}

// 프로토타입 메서드
Person.prototype.sayHi = function () {
    console.log(`Hi! My name is ${this.name}`);
};

const me = new Person('lee');
me.sayHi(); // Hi! My name is lee
```

클래스 몸체에서 정의한 메서드는 생성자 함수에 의한 객체 생성 방식과는 다르게 클래스의 prototype 프로퍼티에 메서드를 추가하지 않아도 기본적으로 프로토탕비 메서드가 된다.

```js
class Person {
    // 생성자
    constructor(name) {
        this.name = name;
    }

    // 프로토타입 메서드
    sayHi() {
        console.log(`Hi My name is ${this.name}`);
    }
}

const me = new Person('Lee');
me.sayHi(); // Hi! My name is lee
```

위처럼 클래스 몸체에서 정의한 메서드는 인스턴스의 프로토타입에 존재하는 프로토타입 메서드가 된다.

인스턴스는 프로토타입 메서드를 상속받아 사용할 수 있다.

<br>

프로토타입 체인은 기존의 모든 객체 생성 방식(객체 리터럴, 생성자 함수, Object.create 메서드 등)뿐만 아니라 클래스에 의해 생성된 인스턴스에도 동일하게 적용된다.

생성자 함수의 역할을 클래스가 할 뿐이다.

<br>

결국 클래스는 생성자 함수와 같이 인스턴스를 생성하는 생성자 함수라고 볼 수 있다.

다시 말해, 클래스는 생성자 함수와 마찬가지로 프로토타입 기반의 객체 생성 메커니즘이다.

### 정적 메서드
정적 메서드는 인스턴스를 생성하지 않아도 호출할 수 있는 메서드를 말한다.

생성자 함수의 경우 정적 메서드를 생성하기 위해서는 다음과 같이 명시적으로 생성자 함수에 메서드를 추가해야 한다.

```js
// 생성자 함수
function Person(name) {
    this.name = name;
}

// 정적 메서드
Person.sayHi = function () {
    console.log('Hi!');
};

// 정적 메서드 호출
Person.sayHi(); // Hi
```

<br>

클래스에서는 메서드에 `static` 키워드를 붙이면 정적 메서드(클래스 메서드)가 된다.

```js
class Person {
    // 생성자
    constructor(name) {
        this.name = name;
    }

    // 정적 메서드
    static sayHi() {
        console.log('Hi');
    }
}
```

<br>

정적 메서드는 클래스에 바인딩된 메서드가 된다. 

클래스는 함수 객체로 평가되므로 자신의 프로퍼티/메서드를 소유할 수 있다.

클래스는 클래스 정의(클래스 선언문이나 클래스 표현식)가 평가되는 시점에 함수 객체가 선언되므로 인스턴스와 달리 별다른 생성 과정이 필요하지 않다.

따라서 정적 메서드는 클래스 정의 이후 인스턴스를 생성하지 않아도 호출할 수 있다.

```js
// 정적 메서드는 클래스로 호출한다.
// 정적 메서드는 인스턴스 없이도 호출할 수 있다.
Person.sayHi(); // Hi
```

정적 메서드는 인스턴스로 호출할 수 없다.

정적 메서드가 바인딩된 클래스는 인스턴스의 프로토타입 체인상에 존재하지 않기 때문이다.

다시 말해, 인스턴스의 프로토타입 체인 상에는 클래스가 존재하지 않기 때문에 클래스의 메서드를 상속 받을 수 없다.

```js
// 인스턴스 생성
const me = new Person('lee');
me.sayHi(); // TypeError
```

### 정적 메서드와 프로토타입 메서드의 차이
정적 메서드와 프로토타입 메서드의 차이는 다음과 같다.
- 정적 메서드와 프로토타입 메서드는 자신이 속해있는 프로토타입 체인이 다르다.
- 정적 메서드는 클래스로 호출하고 프로토타입 메서드는 인스턴스로 호출한다.
- 정적 메서드는 인스턴스 프로퍼티를 참조할 수 없지만 프토토타입 메서드는 인스턴스 프로퍼티를 참조할 수 있다.

<br>

```js
class Square {
    // 정적 메서드
    static area(width, height) {
        return width * height;
    }
}

console.log(Square.area(10, 10));   // 100
```

정적 메서드 `area`는 2개의 인수를 전달받아 면적을 계산한다.

이때 해당 메서드는 인스턴스 프로퍼티를 참조하지 않는다.

만약 인스턴스 프로퍼티를 참조해야 한다면 정적 메서드 대신 프로토타입 메서드를 사용해야 한다.

```js
class Square {
    constructor(width, height) {
        this.width = width;
        this.height = height;
    }

    // 프로토타입 메서드
    area() {
        return this.width * this.height;
    }
}

const square = new Square(10, 10);
console.log(square.area()); // 100
```

메서드 내부의 this는 메서드를 소유한 객체가 아니라 메서드를 호출한 객체, 즉 메서드 이름 앞의 마침표 연산자 앞에 기술한 객체를 바인딩한다.

<br>

프로토타입 메서드는 인스턴스로 호출해야 하므로 프로토타입 메서드 내부의 this는 프로토타입 메서드를 호출한 인스턴스를 가리킨다.

위 예제에서는 `square` 객체로 프로토타입 메서드 area를 호출했기 때문에 area 내부의 this는 square 객체를 가리킨다.

<br>

정적 메서드는 클래스로 호출해야 하므로 정적 메서드의 내부의 this는 인스턴스가 아닌 클래스를 가리킨다.

즉, 프로토타입 메서드와 정적 메서드 내부의 this 바인딩이 다르다.

<br>

표준 빌트인 객체인 Math, Number, JSON, Object, Reflect 등은 다양한 정적 메서드를 가지고 있다.

이 정적 메서드는 애플리케이션 전역에서 사용할 유틸리티 함수다.

### 클래스에서 정의한 메서드의 특징
클래스에서 정의한 메서드는 다음과 같은 특징을 갖는다.

1. function 키워드를 생략한 메서드 축약 표현을 사용한다.
2. 객체 리터럴과는 다르게 클래스에 메서드를 정의할 때는 콤마가 필요 없다.
3. 암묵적으로 strict mode로 실행된다.
4. for ... in 문이나 Object.keys 메서드 등으로 열거할 수 없다.
    - 즉, 프로퍼티의 열거 가능 여부를 나타내며, 불리언 값을 갖는 프로퍼티 어트리뷰트 [[Enumerable]] 값이 false이다.
5. 내부 메서드 [[Construct]]를 갖지 않는 non-constructor다. 따라서 new 연산자와 함께 호출할 수 없다.

## 클래스의 인스턴스 생성 과정
new 연산자와 함께 클래스를 호출하면 생성자 함수와 마찬가지로 클래스의 내부 메서드 [[Construct]]가 호출된다.

생성자 함수의 인스턴스 생성 과정과 유사하게 인스턴스가 생성된다.

### 1. 인스턴스 생성과 this 바인딩
new 연산자와 함께 클래스를 호출하면 constructor의 내부 코드가 실행되기에 앞서 암묵적으로 빈 객체가 생성된다.

이 빈 객체가 클래스가 생성한 인스턴스다.

<br>

이때 클래스가 생성한 인스턴스의 프로토타입으로 클래스의 prototype 프로퍼티가 가리키는 객체가 설정된다.

그리고 암묵적으로 생성된 빈 객체, 즉 인스턴스는 this에 바인딩된다.

따라서 constructor 내부의 this는 클래스가 생성한 인스턴스를 가리킨다.

### 2. 인스턴스 초기화
constructor의 내부 코드가 실행되어 this에 바인딩되어 있는 인스턴스를 초기화한다.

즉, this에 바인딩되어 있는 인스턴스에 프로퍼티를 추가하고 constructor가 인수로 전달받은 초기값으로 인스턴스의 프로퍼티 값을 초기화한다.

만약 constructor가 생략되었다면 해당 과정도 생략된다.

### 3. 인스턴스 반환
클래스의 모든 처리가 끝내면 완성된 인스턴스가 바인딩된 this가 암묵적으로 반환된다.

```js
class Person {
    // 생성자
    constructor(name) {
        // 1. 암묵적으로 인스턴스가 생성되고 this에 바인딩된다.
        console.log(this);  // Person {}
        console.log(Object.getPrototypeOf(this) === Person.prototype);  // true

        // 2. this에 바인딩되어 있는 인스턴스를 초기화한다.
        this.name = name;

        // 3. 완성된 인스턴스가 바인딩된 this가 암묵적으로 반환된다.
    }
}
```

## 프로퍼티
### 인스턴스 프로퍼티
인스턴스 프로퍼티는 constructor 내부에서 정의해야 한다.
```js
class Person {
    constructor(name) {
        // 인스턴스 프로퍼티
        this.name = name;
    }
}

const me = new Person('Lee');
console.log(me);    // Person {name: 'Lee'}
```

constructor 내부 코드가 실행되기 이전에 constructor 내부의 this에는 이미 클래스가 암묵적으로 생성한 인스턴스인 빈 객체가 바인딩되어 있다.

생성자 함수에서 생성자 함수가 생성할 인스턴스의 프로퍼티를 정의하는 것과 마찬가지로 constructor 내부에서 this에 인스턴스 프로퍼티를 추가한다.

이로써 클래스가 암묵적으로 생성한 빈 객체, 즉 인스턴스에 프로퍼티가 추가되어 인스턴스가 초기화된다.

```js
class Person {
    constructor(name) {
        // 인스턴스 프로퍼티
        this.name = name;   // name 프로퍼티는 public 하다.
    }
}

const me = new Person('Lee');

// name은 public하다.
console.log(me.name);   // lee
```

constructor 내부에서 this에 추가한 프로퍼티는 언제나 클래스가 생성한 인스턴스의 프로퍼티가 된다.

ES6의 클래스는 다른 객체지향 언어처럼 private, public, protected 키워드와 같은 접근 제한자를 지원하지 않는다.

따라서 인스턴스 프로퍼티는 언제나 public하다.

<br>

이때 private한 프로퍼티를 정의할 수 있는 사양히 제안 중에 있다.

> ⭐ 자세한 내용은 [25.7.4장 private 필드 정의 제안]()을 확인하면 된다.

### 접근자 프로퍼티
접근자 프로퍼티는 자체적으로는 값(`[[Value]]`)을 갖지 않고 다른 데이터 프로퍼티의 값을 읽거나 저장할 때 사용하는 접근자 함수로 구성된 프로퍼티다.

```js
const person = {
    // 데이터 프로퍼티
    firstName: 'Ungmo',
    lastName: 'lee',

    // fullName은 접근자 함수로 구성된 접근자 프로퍼티다.
    // getter 함수
    get fullName() {
        return `${this.firstName} ${this.lastName}`;
    },
    // setter 함수
    set fullName() {
        // 배열 디스트럭처링 할당: 36.1 배열 디스트럭처링 할당 참고
        [this.firstName, this.lastName] = name.split(' ');
    }
};

// 데이터 프로퍼티를 통한 프로퍼티 값의 참조
console.log(`${person.firstName} ${person.lastName}`);  // Ungmo lee

// 접근자 프로퍼티를 통한 프로퍼티 값의 저장
// 접근자 프로퍼티 fullName에 값을 저장하면 setter 함수가 호출된다.
person.fullName = 'Heegun Lee';
console.log(person);    // {firstName: 'Heegun', lastName: 'Lee'}

// 접근자 프로퍼티를 통한 프로퍼티 값의 참조
// 접근자 프로퍼티 fullName에 접근하면 getter 함수가 호출된다.
console.log(person.fullName);   // Heegun lee

// fullName은 접근자 프로퍼티다.
// 접근자 프로퍼티는 get, set, enumerable, configurable 프로퍼티  어트리뷰트를 가진다.
console.log(Object.getOwnPropertyDescriptor(person, 'fullName'));
```

위와 같은 객체 리터럴을 클래스로 표현하면 다음과 같다.

```js
class Person {
    constructor(firstName, lastName) {
        this.firstName = firstName;
        this.lastName = lastName;
    }

    // fullName은 접근자 함수로 구성된 접근자 프로퍼티다.
    // getter 함수
    get fullName() {
        return `${this.firstName} ${this.lastName}`;
    }

    // setter 함수
    set fullName(name) {
        [this.firstName, this.lastName] = name.split(' ');
    }
}

const me = new person('Ungmo', 'lee');

// 데이터 프로퍼티를 통한 프로퍼티 값의 참조
console.log(`${me.firstName} ${me.lastName}`);  // Ungmo lee

// 접근자 프로퍼티를 통한 프로퍼티 값의 저장
me.fullName = 'Heegun lee';
console.log(me);    // {fistName: "Heegun", lastName: 'Lee'}

// 접근자 프로퍼티를 통한 프로퍼티 값의 참조
// 접근자 프로퍼티 fullName에 접근하면 getter 함수가 초기화된다.
console.log(me.fullName);   // Heegun lee
```

### 클래스 필드 정의 제안
> 💡 **클래스 필드**<br>
> 클래스 기반 객체지향 언어에서 클래스가 생성할 인스턴스의 프로퍼티를 가리키는 용어.

<br>

자바의 클래스 필드는 마치 클래스 내부에서 변수처럼 사용된다.

```java
public class Person {
    // 1. 클래스 필드 정의
    // 클래스 필드는 클래스 몸체에 this 없이 선언해야 한다.
    private String firstName = "";
    private Stirng lastName = "";

    // 생성자
    Person(String firstName, String lastName) {
        // 3. this는 언제나 클래스가 생성할 인스턴스를 가리킨다.
        this.firstName = firstName;
        this.lastName = lastName;
    }

    public String getFullName() {
        // 2. 클래스 필드 참조
        // this 없이도 클래스 필드를 참조할 수 있다.
        return firstName + " " + lastName;
    }
}
```

자바스크립트의 클래스에서 인스턴스 프로퍼티를 선언하고 초기화하려면 반드시 constructor 내부에서 this에 프로퍼티를 추가해야 한다.

하지만 자바의 클래스에서는 위 예제 1번과 같이 클래스 필드를 마치 변수처럼 클래스 몸체에 this 없이 선언한다.

<br>

또한 자바스크립트의 클래스에서 인스턴스 프로퍼티를 참조하려면 반드시 this를 사용하여 참조해야 한다.

하지만 자바의 클래스에서는 위 예제 2와 같이 this를 생략해도 클래스 필드를 참조할 수 있다.

<br>

클래스 기반 객체지향 언어의 this는 언제나 클래스가 생성할 인스턴스를 가리킨다.

위 예제의 3번과 같이 this는 주로 클래스 필드가 생성자 또는 메서드의 매개변수 이름과 동일할 때 클래스 필드임을 명확히 하기 위해 사용한다.

<br>

자바스크립트의 클래스 몸체에는 메서드만 선언할 수 있다.

따라서 클래스 몸체에 자바와 유사하게 클래스 필드를 선언하면 문법 에러가 발생한다.

```js
class Person {
    // 클래스 필드 정의
    name = 'lee';
}

const me = new person('lee');
```

하지만 위처럼 최신 브라우저 또는 최신 Node.js에서 실행하면 문법 에러가 발생하지 않고 정상 동작한다.

그 이유는 `Class field declarations`가 제안되어 있기 때문이다.

<br>

클래스 몸체에서 클래스 필드를 정의하는 경우 `this`에 클래스 필드를 바인딩해서는 안된다.

`this`는 클래스의 constructor와 메서드 내에서만 유효하다.

```js
class Person {
    // this에 클래스 필드를 바인딩해서는 안 된다.
    this.name = ''; // SyntaxError
}
```

클래스 필드를 참조하는 경우, 자바와 같은 클래스 기반 객체지향 언어에서는 this를 생략할 수 있으나 자바스크립트에서는 this를 반드시 사용해야 한다.

```js
class Person {
    // 클래스 필드
    name = 'lee';

    constructor() {
        console.log(name);  // ReferenceError
    }
}

new Person();
```

또한 클래스 필드에 초기값을 할당하지 않으면 undefined를 갖는다.

```js
class Person {
    // 클래스 필드를 초기화하지 않으면 undefined를 갖는다.
    name;
}

const me = new Person();
console.log(me);    // Person {name: undefined}
```

인스턴스를 생성할 때 외부의 초기값으로 클래스 필드를 초기화해야 할 필요가 있다면 constructor에서 클래스 필드를 초기화해야 한다.

```js
class Person {
    name;

    constructor(name) {
        // 클래스 필드 초기화
        this.name = name;
    }
}

const me = new person('lee');
console.log(me);
```

또한 일급 객체인 함수를 클래스 필드에 할당할 수 있다.

```js
class Person {
    // 클래스 필드에 문자열을 할당
    name = 'lee';

    // 클래스 필드에 함수를 할당
    getName = function () {
        return this.name;
    }

    // 화살표 함수로 정의할 수 있다.
    // getName = () => this.name;
}

const me = new Person();
console.log(me);    // Person {name: "lee", getName: f}
console.log(me.getName());  // lee
```

하지만 이처럼 클래스 필드에 함수를 할당하는 경우, 이 함수는 프로토타입 메서드가 아닌 인스턴스 메서드가 된다.

모든 클래스 필드는 인스턴스 프로퍼티가 되기 때문이다.

따라서 클래스 필드에 함수를 할당하는 것은 권장하지 않는다.

### private 필드 정의 제안
[24.5장 캡슐화와 정보 은닉]()에서 자바스크립트는 캡슐화를 완전하게 지원하지 않는다.

ES6의 클래스도 생성자 함수와 마찬가지로 다른 클래스 기반 객체지향 언어에서는 지원하는 `private`, `public`, `protected` 키워드와 같은 접근 제한자를 지원하지 않는다.

따라서 인스턴스 프로퍼티는 인스턴스를 통해 클래스 외부에서 언제나 참조할 수 있다.

즉, 언제나 `public`이다.

```js
class Person {
    constructor(name) {
        this.name = name;   // 인스턴스 프로퍼티는 기본적으로 public이다.
    }
}

const me = new Person('lee');
console.log(me.name);   // lee
```

클래스 필드 정의 제안을 사용하더라도 클래스 필드는 기본적으로 public하기 때문에 외부에 그대로 노출된다.

```js
class Person {
    name = 'lee';   // 클래스 필드도 기본적으로 public이다.
}

// 인스턴스 생성
const me = new Person();
console.log(me.name);   // lee
```

<br>

`private` 필드를 참조할 때도 `#`을 붙여야 한다.

```js
class Person {
    // private 필드 정의
    #name = '';

    constructor(name) {
        // private 필드 참조
        this.#name = name;
    }
}

const me = new Person('lee');

// private 필드 #name은 클래스 외부에서 참조할 수 없다.
console.log(me.#name);
// SyntaxError
```

> ⭐ **타입스크립트**<br>
> 자바스크립트의 상위 확장 언어로, 클래스 기반 객체지향 언어가 지원하는 접근 제한자인 public, private, protected를 모두 지원하며, 의미 또한 모두 지원한다.

<br>

`public` 필드는 어디서든 참조할 수 있지만, `private` 필드는 클래스 내부에서만 참조할 수 있다.

|접근 가능성|public|private|
|--|--|--|
|클래스 내부|O|O|
|자식 클래스 내부|O|X|
|클래스 인스턴스를 통한 접근|O|X|

<br>

`private` 필드에 직접 접근할 수 있는 방법은 없다.

다만 접근자 프로퍼티를 통해 간접적으로 접근하는 방법은 유효하다.

```js
class Person {
    // private 필드 정의
    #name = '';

    constructor(name) {
        this.#name = name;
    }

    // name은 접근자 프로퍼티다.
    get name() {
        // private 필드를 참조하여 trim한 다음 반환한다.
        return this.#name.trim();
    }
}

const me = new Person(' lee ');
console.log(me.name);   // lee
```

이때 `private` 필드는 반드시 클래스 몸체에 정의해야 한다.

```js
class Person {
    constructor(name) {
        // private 필드는 클래스 몸체에서 정의해야 한다.
        this.#name = name;
        // SyntaxError
    }
}
```

### static 필드 정의 제안
클래스에는 `static` 키워드를 사용하여 정적 메서드를 정의할 수 있다.

하지만 `static` 키워드를 사용하여 정적 필드를 정의할 수는 없었다.

<br>

하지만 `static public` 필드, `static private` 필드, `static private` 메서드를 정의할 수 있는 새로운 표준 사항인 `Static class features`가 제안되어 있다.

```js
class MyMath {
    // static public 필드 정의
    static PI = 22 / 7;

    // static private 필드 정의
    static #num = 10;

    // static 메서드
    static increment() {
        return ++MyMath.#num;
    }
}

console.log(MyMath.PI);
console.log(MyMath.increment());
```

## 상속에 의한 클래스 확장
### 클래스 상속과 생성자 함수 상속
상속에 의한 클래스 확장은 지금까지 살펴본 프로토타입 기반 상속과는 다른 개념이다.

#### 📌 프로토타입 기반 상속 vs 클래스 기반 상속
- **프로토타입 기반 상속**: 프로토타입 체인을 통해 다른 객체의 자산을 상속 받는 개념
- **상속에 의한 클래스 확장**: 기존 클래스를 상속받아 새로운 클래스를 확장하여 정의하는 것

<br>

클래스와 생성자 함수는 인스턴스를 생성할 수 있는 함수라는 점에서 매우 유사하다.

하지만 클래스는 상속을 통해 기존 클래스를 확장할 수 있는 문법이 기본적으로 제공되지만, 생성자 함수는 그렇지 않다.

<br>

예를 들어, 동물을 추상화한 `Animal` 클래스와 새와 사자를 추상화한 `Bird`와 `Lion` 클래스를 각각 정의한다고 생각해보자.

새와 사자는 동물에 속하기 때문에 동물의 속성을 갖지만, 각각의 고유한 속성 또한 갖는다.

이때 `Animal` 클래스는 동물의 속성을 표현하고 `Bird`, `Lion` 클래스는 상속을 통해 `Animal` 클래스의 속성을 그대로 사용하면서 자신만의 고유한 속성만을 추가하여 확장할 수 있다.

### extends 키워드
상속을 통해 클래스를 확장하려면 `extends` 키워드를 사용하여 상속받을 클래스를 정의한다.

```js
// 슈퍼(베이스/부모) 클래스
class Base {}

// 서브(파생/자식) 클래스
class Derived extends Base {}
```

상속을 통해 확장된 클래스를 서브 클래스라 부르고, 서브 클래스에게 상속된 클래스를 수퍼 클래스라 부른다.

### 동적 상속
`extends` 키워드는 클래스 뿐만 아니라 생성자 함수를 상속받아 클래스를 확장할 수도 있다.

단, `extends` 키워드 앞에는 반드시 클래스가 와야 한다.

```js
// 생성자 함수
function Base(a) {
    this.a = a;
}

// 셍성자 함수를 상속받는 서브 클래스
class Derived extends Base {}

const derived = new Derived(1);
console.log(derived);   // Derived {a: 1}
```

`extends` 키워드 다음에는 클래스뿐이 아니라 [[Construct]] 내부 메서드를 갖는 함수 객체로 평가될 수 있는 모든 표현식을 사용할 수 있다.

이를 통해 동적으로 상속받을 대상을 결정할 수 있다.

```js
function Base1() {}

class Base2 {}

let condition = true;

// 조건에 따라 동적으로 상속 대상을 결정하는 서브클래스
class Derived extends (condition ? Base1 : Base2) {}

const derived = new Derived();
console.log(derived);   // Derived {}

console.log(derived instaceof Base1);   // true
console.log(derived instaceof Base2);   // false
```

### 서브클래스의 constructor
클래스에서 constructor를 생략하면 클래스에 다음과 같이 비어 있는 constructor가 암묵적으로 정의된다.

```js
constructor() {}
```

서브 클래스에서 constructor를 생략하면 클래스에 다음과 같은 constructor가 암묵적으로 정의된다.

```js
constructor(...args) { super(...args); }
```

`super()`는 수퍼클래스의 constructor를 호출하여 인스턴스를 생성한다.

<br>

수퍼클래스와 서브클래스 모두 constructor를 생략하면 어떻게 될까?

```js
// 수퍼클래스
class Base {}

// 서브클래스
class Derived extends Base {}
```

위 예제의 클래스에는 다음과 같이 암묵적으로 constructor가 정의된다.

```js
// 수퍼클래스
class Base {
    constructor() {}
}

// 서브클래스
class Derived extends Base {
    constructor(...args) { super(...args); }
}

const derived = new Derived();
console.log(derived);   // Derived {}
```

위 예제와 같이 수퍼클래스와 서브클래스 모두 constructor를 생략하면 빈 객체가 생성된다.

프로퍼티를 소유하는 인스턴스를 생성하려면 constructor 내부에서 인스턴스에 프로퍼티를 추가해야 한다.

### super 키워드
> 💡 **super 키워드**<br>
> 함수처럼 호출할 수도 있고, this와 같이 식별자처럼 참조할 수 있는 특수한 키워드다.

`super` 키워드는 다음과 같이 작동한다.

- super를 호출하면 수퍼클래스의 constructor를 호출한다.
- super를 참조하면 수퍼클래스의 메서드를 호출할 수 있다.
