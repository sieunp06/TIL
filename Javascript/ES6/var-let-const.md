# var과 let 그리고 const
### 목차
1. [ES6 - let과 const의 추가](#es6---let과-const의-추가)
2. [재할당과 재선언](#재할당과-재선언)
3. [함수 스코프와 블록 스코프](#함수-스코프와-블록-스코프)
4. [Hoisting 호이스팅](#hoisting-호이스팅)
    - [호이스팅이란?](#호이스팅이란)
    - [호이스팅 과정에서 undefined를 할당하지 않는 let과 const](#호이스팅-과정에서-undefined를-할당하지-않는-let과-const)

---
## ES6 - let과 const의 추가
`ES6`에서는 `let`과 `const`가 추가되었다.

`ES6` 발표 전에는 `var`만 존재했었다.

## 재할당과 재선언
### 📌 const
`const`는 constant 즉, 상수를 뜻한다.

`const`를 사용한 변수는 값을 재할당할 수 없으며, 반드시 값을 할당해야 한다.

```js
const name = 'Sieun';
name = 'Not Sieun';     // TypeError: Assignment to constant variable
```

만약 재할당을 시도하면 `TypeError`가 발생한다.

```js
const name;
console.log(name);      // SyntaxError: Missing initializer in const declaration
```

값을 할당하지 않고 선언하게 되면 `SyntaxError`가 발생한다.

### 📌 let
`let`은 변수를 선언할 때 사용한다.

```js
let meal = 'Bulgogi';
meal = 'kimbab';
```

하지만 `let`을 사용하여 변수를 선언할 때 값을 할당하지 않을 경우, 해당 값은 `undefined`가 된다.

```js
let a;
console.log(a);
```
```
undefined
```

### 📌 var
`var`은 `let`과 동일하게 재할당이 가능한 변수이다.

이때 `let`과 다른 점은 변수를 재선언할 수 있다는 점이다.

```js
var meal = 'Bulgogi';
meal = 'kimbab';
var meal = `cake`;
```

`let`으로 변수를 재선언하면 다음과 같이 `SyntaxError`가 발생한다.

```js
let meal = 'Bulgogi';
meal = 'kimbab';
let meal = `cake`;      // SyntaxError: Identifier 'meal' has already been declared
```

## Fuction-scope와 Block-scope
### 📌 Function-scope
`var`은 `Function-scope`에 해당된다.

```js
var level = 10;
console.log(level);     // 10

function levelUp() {
    var level = 30;
    console.log(level);     // 30
}

console.log(level);     // 10
```
즉, 함수 안에서 선언한 `level`은 지역 변수이기 때문에, 마지막에 호출한 `level`은 맨 처음 선언했던 전역 변수 `level`의 값인 `10`을 반환한다.

```js
var level = 10;
console.log(level);     // 10

if (true) {
    var level = 30;
    console.log(level);     // 30
}

console.log(level);     // 30
```
위 예제에서 마지막 `level`의 값이 `30`인 이유는 `if문`이 함수가 아니기 때문이다.

때문에 if문 안의 `level` 재선언으로 `level`의 값이 바뀐 것이다.

### 📌 Block-scope
`const`와 `let`이 `Block-scope`에 해당된다.

```js
let level = 10;
console.log(level);     // 10
 
function levelUp () {
    let level = 30;
    console.log(level);     // 30
}
 
console.log(level);     // 10
```
```js
let level = 10;
console.log(level);     // 10
 
if (true) {
    let level = 30;
    console.log(level);     // 30
}
 
console.log(level);     // 10
```

`Block-scope`는 반복문, 조건문 모두 블록이기 때문에 블록 안에서 재선언된 변수는 지역 변수가 된다.

## Hoisting 호이스팅
### 호이스팅이란?
`호이스팅`이란 스코프(함수) 안에 존재하는 모든 선언들을 해당 스코프(함수)의 최상단으로 끝어올리는 것처럼 보이는 것을 의미한다.

```js
console.log(x);
var x;
```
```
undefined
```

원래 작성한 코드는 작성한 순서대로 윗줄에서부터 차례대로 실행된다.

하지만, 위 예제를 보면 변수 `x`가 `console.log(x)` 전에 선언되지 않았지만 오류가 발생되지 않고 `undefined`가 출력된다.

이렇게 중간에 변수를 선언했지만, 맨 위에서 선언한 것처럼 끌어올려지는 것을 `호이스팅`이라고 한다.

### 호이스팅 과정에서 undefined를 할당하지 않는 let과 const
`ES6` 이후부터는 호이스팅 과정에서 `undefined`를 할당하지 않는 `let`과 `const`가 등장했다.

```js
let x;
let y = 2;

if (true) {
    let z;
    z = 2;
}

console.log(z);     // not defined Error
```

`let`과 `const`는 `var`과 마찬가지로 호이스팅이 되긴 하지만, 호이스팅의 과정에서 `undefined`를 할당하지 않기 때문에 위와 같은 예제에서는 오류가 발생한다.

---
## References
- [https://nykim.work/72](https://nykim.work/72)
- [https://lordofkangs.tistory.com/262](https://lordofkangs.tistory.com/262)
- [https://yoo11052.tistory.com/151](https://yoo11052.tistory.com/151)
- [https://ingg.dev/hoisting/](https://ingg.dev/hoisting/)
