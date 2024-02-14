# const VS Object.freeze() VS Object.seal()
### 목차
1. [❓const로 선언한 객체는 수정이 가능하다❓](#❓const로-선언한-객체는-수정이-가능하다❓)
    - [✨ 수정이 된다?](#✨-수정이-된다)
    - [✨ 딱 하나 안되는 것: 변수에 대한 재할당](#✨-딱-하나-안되는-것-변수에-대한-재할당)
2. [Object.seal()과 Object.freeze()](#objectseal과-objectfreeze)
    - [Object.seal()과 Object.freeze()의 차이점](#objectseal과-objectfreeze의-차이점)
        - [👉 Object.seal()](#👉-objectseal)
        - [👉 Object.freeze()](#👉-objectfreeze)

---
## ❓const로 선언한 객체는 수정이 가능하다❓
`const`는 보통 상수라고 알고 있다.

하지만 `const`로 선언한 객체 형태의 내부는 재할당 및 수정이 가능하다.

### ✨ 수정이 된다?
```js
const obj = {};
obj.key = "value";

console.log(obj);
```
```
{ key: 'value' }
```

빈 객체에 값을 할당하니 할당이 된다.

```js
const obj = {key: "value"};
obj.key = "new value";

console.log(obj);
```
```
{ key: 'new value' }
```
수정이 된다?

### ✨ 딱 하나 안되는 것: 변수에 대한 재할당
```js
const obj = {key: "value"};
obj = {key: "new value"};   // TypeError: Assignment to constant variable.
```

> 📌 [Javascript/ES6 - var과 let 그리고 const](https://github.com/sieunp06/TIL/blob/main/Javascript/ES6/var-let-const.md)

## Object.seal()과 Object.freeze()
`Object.seal()`과 `Object.freeze()` 모두 객체를 고정시킨다.

즉, 위 메서드들을 사용한 객체는 다음과 같아진다.

- 새로운 속성을 추가할 수 없다.
- 현재 존재하는 모든 속성을 설정 불가능 상태로 만든다.

### Object.seal()과 Object.freeze()의 차이점
`Object.seal()`은 쓰기 가능한 속성의 값은 변경할 수 있지만, `Object.freeze()`는 속성의 값 또한 변경할 수 없다는 것이다.

#### 👉 Object.seal()
```js
let obj = {name: '홍길동', age: 23, houseLocation: ['서울', '경기도']};
Object.seal(obj);

obj.pants = 'blue';

console.log(obj);

delete obj.name;

console.log(obj);
```
```
{ name: '홍길동', age: 23, houseLocation: [ '서울', '경기도' ] }
{ name: '홍길동', age: 23, houseLocation: [ '서울', '경기도' ] }
```

위 예제에서는 `Object.seal()`을 통해 객체를 고정시킨 후, `pants` 프로퍼티를 추가하고 객체를 삭제해도 객체의 프로퍼티들이 그대로 출력된다.

```js
let obj = {name: '홍길동', age: 23, houseLocation: ['서울', '경기도']};
Object.seal(obj);

obj.name = 'sieun';

console.log(obj);
```
```
{ name: 'sieun', age: 23, houseLocation: [ '서울', '경기도' ] }
```
하지만 원래 존재하던 속성 값의 수정은 가능하다.

#### 👉 Object.freeze()
`Object.freeze()`는 `Object.seal()`과 거의 동일하지만, `Object.freeze()`는 값의 수정 또한 불가능하다.

```js
let obj = {name: '홍길동', age: 23, houseLocation: ['서울', '경기도']};
Object.freeze(obj);

obj.name = 'sieun';

console.log(obj);
```
```
{ name: '홍길동', age: 23, houseLocation: [ '서울', '경기도' ] }
```
`Object.seal()`을 사용했을 때와 달리 원래 존재하던 속성의 값도 변경되지 않는다.

---
## References
- [https://moneytech.kr/115#---%--%EB%B-%--%--%EA%B-%-D%EC%B-%B-%EC%--%--%--%EB%--%--%EB%-B%A-%--%EA%B-%--%--%EB%--%A-%EC%--%B-%EB%B-%B-%EA%B-%B-](https://moneytech.kr/115#---%--%EB%B-%--%--%EA%B-%-D%EC%B-%B-%EC%--%--%--%EB%--%--%EB%-B%A-%--%EA%B-%--%--%EB%--%A-%EC%--%B-%EB%B-%B-%EA%B-%B-)
- [https://velog.io/@muman_kim/Object.freeze%EC%99%80-Object.seal%EC%9D%80-%EB%AC%B4%EC%97%87%EC%9D%B8%EA%B0%80](https://velog.io/@muman_kim/Object.freeze%EC%99%80-Object.seal%EC%9D%80-%EB%AC%B4%EC%97%87%EC%9D%B8%EA%B0%80)