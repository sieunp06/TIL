# Object 객체
### 목차
1. [객체의 생성](#객체의-생성)
    - [📌 리터럴 표기를 이용한 객체의 생성](#📌-리터럴-표기를-이용한-객체의-생성)
    - [📌 생성자를 이용한 객체의 생성](#📌-생성자를-이용한-객체의-생성)
2. [객체 관련 메서드들](#객체-관련-메서드들)

---
## 객체의 생성
#### 📌 리터럴 표기를 이용한 객체의 생성
```js
var kitty = {
    name: "나비",
    family: "코리안 숏 헤어",
    age: 1,
    weight: 0.1
};

console.log(kitty.name);
console.log(kitty.family);
console.log(kitty.age);
console.log(kitty.weight);
```
```
나비
코리안 숏 헤어
1
0.1
```

#### 📌 생성자를 이용한 객체의 생성
```js
function Dog(color, name, age) {
    this.color = color;
    this.name = name;
    this.age = age;
    this.family = "시베리안 허스키";
    this.breed = function() {
        return this.color + " " + this.family;
    };
} 
```
```js
var dog = new Dog("흰색", "마루", 1);
```

> 👉 [Javascript - 프로토타입]()

## 객체 관련 메서드들
### Object.assign()
`Object.assign()`는 하나 이상의 원본 객체들로부터 모든 열거가능한 속성들을 대상 객체로 복사한다.

```js
var user = {
    name : "mike",
    age: 30
}

var cloneUser = user;   // 객체주소 참조값이 복사됨.

var cloneUser2 = Object.assign({}, user);
var cloneUser3 = Object.assign({gender: 'male'}, user); // gender 프로퍼티를 추가해서 객체 값을 얻는다. // {gender: 'male', name = "mike", age: 30}
var cloneUser4 = Object.assign({name: 'Tom'}, user);    // 존재하는 프로퍼티면 값을 덮어 씌운다.

console.log(cloneUser2);
console.log(cloneUser3);
console.log(cloneUser4);
```
```
{ name: 'mike', age: 30 }
{ gender: 'male', name: 'mike', age: 30 }
{ name: 'mike', age: 30 }
```

또한 `Object.assign()`은 각각의 객체 변수를 합칠 수 있다.

```js
const user = {name: "mike"};
const age_info = {age: 30};
const gender_info = {gender: 'male'};

var cloneUser5 = Object.assign(user, age_info, gender_info);

console.log(cloneUser5);
```
```
{ name: "mike", age: 30, gender: 'male' }
```

### Object.keys()
`Object.keys()`는 객체의 키 값만 담은 배열을 반환한다.

```js
var user = {
    name: 'mike', 
    age: 30
};

console.log(Object.keys(user));
```
```
[ 'name', 'age' ]
```

### Object.values()
`Object.values()`는 객체의 value 값만 담은 배열을 반환한다.

```js
var user = {
    name: 'mike', 
    age: 30
};

console.log(Object.values(user));
```
```
[ 'mike', 30 ]
```

### Object.entries()
`Object.entries()`는 `[key, value]` 쌍을 담은 배열을 반환한다.

보통 이차원 배열로 변환한다.

```js
var user = {
    name: 'mike', 
    age: 30
};

console.log(Object.entries(user));
```
```
[ [ 'name', 'mike' ], [ 'age', 30 ] ]
```

### Object.fromEntries()
`Object.fromEntries()`는 `[key, value]` 형태의 배열을 객체로 반환한다.

```js
var user = [["name", "mike"], ["age", 30]];

console.log(Object.fromEntries(user));
```
```
{ name: 'mike', age: 30 }
```

### Object.is()
`Object.is()`는 두 값이 같은지 비교한다.



---
## References
- [https://tcpschool.com/javascript/js_object_create](https://tcpschool.com/javascript/js_object_create)
- [https://inpa.tistory.com/entry/JS-%F0%9F%93%9A-Object-%EB%A9%94%EC%86%8C%EB%93%9C-%E2%9C%8F%EF%B8%8F-%EC%A0%95%EB%A6%AC](https://inpa.tistory.com/entry/JS-%F0%9F%93%9A-Object-%EB%A9%94%EC%86%8C%EB%93%9C-%E2%9C%8F%EF%B8%8F-%EC%A0%95%EB%A6%AC)