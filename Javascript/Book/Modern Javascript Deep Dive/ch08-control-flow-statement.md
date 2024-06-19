### 목차
- [블록문](#블록문)
- [조건문](#조건문)
  - [if ... else 문](#if--else-문)
  - [switch 문](#switch-문)
- [반복문](#반복문)
  - [for 문](#for-문)
  - [while 문](#while-문)
  - [do ... while 문](#do--while-문)
- [break 문](#break-문)
- [continue 문](#continue-문)


---
> 자바스크립트 Deep Dive

<br>

# 08장 제어문
## 블록문
> 💡 **블록문**<br>
> 0개 이상의 문을 중괄호로 묶은 것<br>
> 코드 블록 또는 블록이라고 부르기도 한다.

자바스크립트는 블록문을 하나의 실행 단위로 취급한다.

블록문은 단독으로 사용할 수도 있으나 일반적으로 제어문이나 함수를 정의할 때 사용하는 것이 일반적이다.

<br>

#### 📌 블록문의 끝에는 세미콜론을 붙이지 않는다.
문의 끝에는 세미콜론을 붙이는 것이 일반적이지만, 블록문은 언제나 문의 종료를 의미하는 제체 종결성을 갖기 때문에 블록문의 끝에는 세미콜론을 붙이지 않는다.

```js
// 블록문
{
    var foo = 10;
}
```
```js
// 제어문
var x = 1;
if (x < 10) {
    x++;
}
```
```js
// 함수 선언문
function sum(a, b) {
    return a + b;
}
```

## 조건문
> 💡 **조건문**<br>
> 주어진 조건식의 평과 결과에 따라 코드 블록의 실행을 결정한다.

자바스크립트에서는 다음 두 가지 조건문을 제공한다.
- **if ... else 문**
- **switch 문**

<br>

### if ... else 문
`if ... else` 문은 주어진 조건식의 평가 결과, 즉 논리적 참 거짓에 따라 실행할 코드 블럭을 결정한다.

```js
if (조건식) {
    // 조건식이 참이면 이 코드 블럭이 실행된다.
} else {
    // 조건식이 거짓이면 이 코드 블럭이 실행된다.
}
```

이떄 if 문의 조건식은 불리언 값으로 평가되어야 한다.

만약 불리언 값이 아니라면 자바스크립트 엔진에 의해 암묵적 타입 변환에 의해 강제로 불리언 값으로 변환되어 실행할 코드 블럭을 결정한다.

> ⭐ 자세한 값은 [9.2 암묵적 타입 변환]()을 살펴보면 된다.

<br>

만약 조건에 따라 실행될 코드 블럭을 늘리고 싶으면 `else if` 문을 사용한다.

```js
if (조건식1) {
    // 조건식1이 참이면 이 코드 블럭이 실행된다.
} else if (조건식2) {
    // 조건식2가 참이면 이 코드 블럭이 실행된다.
} else {
    // 조건식1과 조건식2가 모두 거짓이면 이 코드 블럭이 실행된다.
}
```

이떄 `else if` 문과 `else` 문은 선택이다.

즉, 사용할 수도 있고 사용하지 않을 수도 있다.

또한 `if` 문과 `else` 문은 2번 이상 사용할 수 없지만 `else if` 문은 여러 번 사용할 수 있다.

### switch 문
`switch` 문은 주어진 표현식을 평가하여 그 값과 일치하는 표현식을 갖는 case 문으로 실행 흐름을 옮긴다.

```js
switch (표현식) {
    case 표현식1:
        switch 문의 표현식과 표현식1이 일치하면 실행될 문;
        break;
    case 표현식2:
        switch 문의 표현식과 표현식2가 일치하면 실행될 문;
        break;
    defualt:
        switch 문의 표현식과 일치하는 case 문이 없을 때 실행될 문;
}
```

이때 `switch` 문의 표현식과 일치하는 `case` 문이 없다면 실행 순서는 `default` 문으로 이동한다.

<br>

#### 📌 폴스루(fall through)
> 💡 **폴스루(fall through)**<br>
> switch 문의 표현식의 평가 결과와 일치하는 case 문으로 실행 흐름이 이동하여 문을 실행한 것은 맞지만 문을 실행한 후 switch 문을 탈출하지 않고 switch 문이 끝날 때까지 이후의 모든 case 문과 default 문을 실행하는 것.

```js
// 월을 영어로 변환한다.
var month = 11;
var monthName;

switch (month) {
    case 1: monthName = 'January';
    case 2: monthName = 'February';
    case 3: monthName = 'March';
    case 4: monthName = 'April';
    case 5: monthName = 'May';
    case 6: monthName = 'June';
    case 7: monthName = 'July';
    case 8: monthName = 'August';
    case 9: monthName = 'September';
    case 10: monthName = 'October';
    case 11: monthName = 'November';
    case 12: monthName = 'December';
    defualt: monthName = 'Invalid month';
}

console.log(monthName); // 'Invalid month'
```

위 코드는 폴스루의 예시로, 각 case 문에 해당하는 문의 마지막에 break를 사용하지 않았기 때문에 발생한다.

break 문이 없다면 case 문의 표현식과 일치하지 않더라도 실행 흐름이 다음 case 문으로 연이어 이동한다.

<br>

때문에 올바른 `switch` 문은 각 case 문에 break를 붙이는 것이다.

이때 default 문에는 break를 생략하는 것이 일반적이다.

default는 switch 문의 맨 마지막에 위치하기 때문에 default 문의 실행이 종료되면 switch 문을 빠져나가기 때문이다.

## 반복문
> 💡 **반복문**<br>
> 조건식의 평가 결과가 참인 경우 코드 블록을 실행한다. 그 후 조건식을 다시 평가하여 여전히 참인 경우 코드 블록을 다시 실행한다. 이는 조건식이 거짓일 때까지 반복된다.

자바스크립트는 세 가지 반복문을 제공한다.
- **for 문**
- **while 문**
- **do ... while 문**

<br>

> ⭐ **반복문을 대체할 수 있는 다양한 기능**<br>
> - forEach 메서드
> - for ... in 문
> - for ... of 문(ES6)

### for 문
for 문은 조건식이 거짓으로 평가될 때까지 코드 블록을 반복 실행한다.

```js
for (변수 선언문 또는 할당문; 조건식; 증감식) {
    조건식이 참인 경우 반복 실행될 문;
}
```
```js
for (var i = 0; i < 2; i++) {
    console.log(i);
}
```

<br>

#### 📌 for 문을 통한 무한 루프
for 문을 사용할 때 변수 선언문, 조건식, 증감식은 모두 옵션이기 때문에 반드시 사용할 필요는 없다.

```js
for (;;) { ... }
```

위처럼 변수 선언문, 조건식, 증감식을 생략하면 무한 루프가 된다.

### while 문
while 문은 주어진 조건식의 평가 결과가 참이면 코드 블럭을 계속해서 반복 실행한다.

```js
var count = 0;

while (count < 3) {
    console.log(count);
    count++;
}
```

#### 📌 while 문을 통한 무한 루프
조건식 평가 결과가 언제나 참이면 무한 루프가 된다.

```js
while (true) { ... }
```

무한 루프를 탈출하기 위해서는 `break;`를 사용하면 된다.

### do ... while 문
do ... while 문은 코드 블럭을 먼저 실행하고 조건식을 평가한다.

따라서 코드 블럭은 무조건 한 번 이상 실행된다.

```js
var count = 0;

do {
    console.log(count); // 0 1 2
    count++;
} while (count < 3);
```

## break 문
break 문은 코드 블럭을 탈출한다.

이떄 말하는 코드 블럭은 레이블 문, 반복문(for, for ... in, for ... of, while, do ... while) 또는 switch 문의 코드 블럭을 탈출한다.

이외의 코드 블럭에 break 문을 사용하면 SyntaxError가 발생한다.

## continue 문
continue 문은 반복문의 코드 블럭 실행을 현 지점에서 중단하고 반복문의 증감식으로 실행 흐름을 이동시킨다.

break와 달리 반복문을 탈출하지는 않는다.


