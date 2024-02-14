# Prototype
### 목차
1. [상속(inheritance)](#상속inheritance)
    - [프로토타입(Prototype)](#프로토타입prototype)
    - [프로토타입 체인(Prototype Chain)](#프로토타입-체인prototype-chain)
        - [👉 Object.prototype](#👉-objectprototype)
2. [프로토타입 생성](#프로토타입-생성)


---
## 상속(inheritance)
`Javascript`는 프로토타입 기반의 객체 지향 언어이다.

현재 존재하고 있는 객체를 프로토타입으로 사용하여, 해당 객체를 복제하여 재사용하는 것을 `상속`이라고 한다.

### 프로토타입(Prototype)
Javascript의 `모든 객체는 프로토타입을 가지고 있다.`

- 모든 객체는 그들의 프로토타입으로부터 프로퍼티와 메서드를 상속받는다.

모든 객체는 최소한 하나 이상의 다른 객체로부터 상속을 받으며, 이때 상속되는 정보를 제공하는 객체를 프로토타입이라고 한다.

### 프로토타입 체인(Prototype chain)
객체 이니셜라이저를 사용해 생성된 같은 타입의 객체들은 모두 같은 프로토타입을 가진다.

```js
var obj = new Object();     // Object.prototype
var arr = new Array();      // Array.prototype
var date = new Date();      // Date.prototype
```

이렇게 `new` 연산자를 사용해 생성된 객체는 생성자의 프로토타입을 자신의 프로토타입으로 상속 받는다.

#### 👉 Object.prototype
`Object.prototype` 객체는 어떠한 프로토타입도 가지지 않으며, 아무 프로퍼티도 상속 받지 않는 가장 상위에 존재하는 프로토타입이다.

```js
var date = new Date();    
```

위 `Date.prototype` 객체는 `Date.prototype` 뿐만 아니라 `Object.prototype`도 프로토타입으로 가진다.

이런 상속의 연결고리를 `프로토타입 체인`이라고 한다.

## 프로토타입 생성
```js
function Dog(color, name, age) {
    this.color = color;
    this.name = name;
    this.age = age;
}

var myDog = new Dog("흰색", "마루", 1);     // Dog.prototype을 가짐
```

## 객체에 프로퍼티 및 메서드 추가
```js
function Dog(color, name, age) {
    this.color = color;
    this.name = name;
    this.age = age;
}

var myDog = new Dog("흰색", "마루", 1);

myDog.family = "시베리안 허스키";
myDog.breed = function() {
    return this.color + " " + this.family;
};
```

---
## References
- [https://tcpschool.com/javascript/js_object_prototype](https://tcpschool.com/javascript/js_object_prototype)