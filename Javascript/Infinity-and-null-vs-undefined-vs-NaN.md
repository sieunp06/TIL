### 목차
1. [✨ Infinity](#✨-infinity)
2. [✨ NaN](#✨-nan)
3. [✨ null과 undefined](#✨-null과-undefined)
    - [❗ null과 undefined는 다르다.](#❗-null과-undefined는-다르다)
    - [📌 null](#📌-null)
    - [📌 undefined](#📌-undefined)

---
## ✨ Infinity
`Infinity`는 무한대를 의미하며, 양의 무한대인 `Infinity` 그리고 음의 무한대인 `-Infinity`가 존재한다.

```js
typeof Infinity;    // Number
```
`Infinity` 값은 사용자가 임의로 수정할 수 없는 읽기 전용 값이며, Javascript의 어떤 수보다 큰 수로 취급된다.

<br>

- 숫자를 0으로 나누면 `Infinity`를 반환한다.
    ```js
    var x = 10 / 0  // Infinity
    ```
- `Infinity`에 어떠한 수를 산술 연산해도 `Infinity`를 반환한다.
    ```js
    var y = Infinity * 1000;    // Infinity
    ```
- `Infinity`의 역수는 0을 반환한다.
    ```js
    var z = 1 / Infinity;   // 0
    ```

## ✨ NaN
> 말도 안되는 값

`NaN` 즉, `Not a Number`은 숫자가 아니라는 의미로, 정의되지 않은 값이나 표현할 수 없는 값을 나타낼 때 사용한다.

```js
typeof NaN;     // number
```

`NaN`은 숫자가 아니라는 의미이지만 `number` 타입을 가지고 있다.

<br>

- 숫자로 변환할 수 없는 피연산자로 산술 연산을 시도하는 경우 `NaN`을 반환한다.
    ```js
    var x = 100 - "10";         // 90
    var y = 100 - "문자열";     // NaN
    ```
    `"문자열"`은 숫자로 변환할 수 없기 때문에 `NaN`을 반환한다.
- 0을 0으로 나누는 경우 `NaN`을 반환한다.
    ```js
    var z = 0 / 0;      // NaN
    ```

### 📌 isNaN()
`isNaN()`은 전달받은 숫자의 값이 숫자인지 아닌지를 판단한다.

```js
var x = isNaN(100 * "문자열");

console.log(x);     // true
```

해당 값이 `NaN`라면 `true`를 반환하며, `NaN`가 아니라면 `false`를 반환한다.

## ✨ null과 undefined
`null`과 `undefined`는 빈 값 또는 존재하지 않는 값이라는 의미를 가지고 있다.

### ❗ null과 undefined는 다르다.
아래 코드에서 `null`과 `undefined`를 비교한 결과는 다음과 같다.

```js
console.log(null == undefined);     // true
console.log(null === undefined);    // false
```

`==`를 사용하여 비교했을 때는 `true`가 반환되지만, `===`를 사용하여 자료형까지 비교했을 때는 `false`를 반환한다.

```js
console.log(typeof null);           // object
console.log(typeof undefined);      // undefined
```

다음과 같이 `null`과 `undefined`는 타입이 다르다.

 ### 📌 null
`null`은 object 타입으로 비어있는 변수 또는 값이 존재하지 않음을 의미한다.

```js
var str = null;     // null
```


### 📌 undefined
`undefined`는 변수가 정의되었으나 아무 값도 할당 받지 않은 상태를 의미한다.

object 타입인 `null`과 달리 `undefined`는 타입이 정해지지 않은 것이다.

<br>

- 변수를 초기화 하지 않은 경우 `undefined`을 반환한다.
    ```js
    var num;

    console.log(num);       // undefined
    ```
- 정의되지 않은 변수에 접근할 경우 `undefined`를 반환한다.
    ```js
    console.log(num2);      // undefined
    ```

---
## References
- [https://tcpschool.com/javascript/js_standard_number](https://tcpschool.com/javascript/js_standard_number)
- [https://tcpschool.com/javascript/js_datatype_basic](https://tcpschool.com/javascript/js_datatype_basic)
- [https://inpa.tistory.com/entry/%F0%9F%93%9A-null-undefined-NaN](https://inpa.tistory.com/entry/%F0%9F%93%9A-null-undefined-NaN)
- [https://jsdevlog.tistory.com/entry/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8javascript-null%EA%B3%BC-undefined%EC%9D%98-%EC%B0%A8%EC%9D%B4](https://jsdevlog.tistory.com/entry/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8javascript-null%EA%B3%BC-undefined%EC%9D%98-%EC%B0%A8%EC%9D%B4)