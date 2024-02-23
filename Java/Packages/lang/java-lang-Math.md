## java.lang.Math 클래스
### 목차
- [random()](#random)
    - [특정 범위의 난수를 생성하는 방법](#특정-범위의-난수를-생성하는-방법)
- [abs()](#abs)
- [floor(), ceil(), round()](#floor-ceil-round)
- [max()와 min()](#max와-min)
- [pow()와 sqrt()](#pow와-sqrt)

---
## Math.E와 Math.PI
- `Math.E`: 오일러의 수, 약 2.718
- `Math.PI`: 파이 값, 3.14159

## random()
`random()`은 0.0 이상 1.0 미만의 범위의 double형 값을 하나 생성하여 반환한다.

```java
int randomNumber = (int)(Math.random() * 100);     // 0 ~ 99
System.out.println(randomNumber);
```

`java.lang.Math` 클래스는 `java.util.Random` 클래스를 사용한 의사 난수 생성기를 사용하여 임의의 수를 생성한다.

때문에 아래와 같이 `java.util.Random` 클래스의 `nextInt()` 메서드를 사용하여 난수를 생성할 수 있다.

```java
Random ran = new Random();
System.out.println(ran.nextInt(100));   // 0 ~ 99
```

### 특정 범위의 난수를 생성하는 방법
아래 예시들과 같이 특정 범위의 난수를 생성할 수 있다.

```java
(int)(Math.random() * 6);       // 0 ~ 5
((int)(Math.random() * 6) + 1); // 1 ~ 6
((int)(Math.random() * 6) + 3); // 3 ~ 8
```

## abs()
`abs()`는 인수로 받은 값의 절댓값을 반환한다.

즉, 음수가 전달되면 해당 수의 절댓값을 반환하며, 양수가 전달됐을 경우 그 값을 그대로 반환한다.

```java
Math.abs(-1);       // 1
Math.abs(3);        // 3
Math.abs(-3.14);    // 3.14
```

## floor(), ceil(), round()
### floor()
`floor()`은 인수로 전달받은 값과 같거나 작은 수 중에서 가장 큰 정수를 반환한다.

```java
Math.floor(10.0)     // 10.0
Math.floor(10.9)     // 10.0
```

### ceil()
`ceil()`은 인수로 전달받은 값과 같거나 큰 수 중에서 가장 작은 정수를 반환한다.

```java
Math.ceil(10.0)      // 10.0
Math.ceil(10.1)      // 11.0
Math.ceil(10.000001) // 11.0
```

### round()
`round()`는 전달받은 실수를 소수점 첫째 자리에서 반올림한 정수를 반환한다.

```java
Math.round(10.0)     // 10
Math.round(10.4)     // 10
Math.round(10.5)     // 11
```

## max()와 min()
`max()`와 `min()`은 각각 전달된 두 인수 중 큰 값과 작은 값을 반환한다.

```java
Math.max(2, 5);     // 5
```
```java
Math.min(2, 5);     // 2
```

## pow()와 sqrt()
### pow()
`pow()`는 전달된 두 double형 값으로 제곱 연산을 수행하여 반환한다.

```java
Math.pow(a, b);
```

위 코드는 `a`의 `b`승을 반환한다.

```java
(int)Math.pow(5, 2);    // 25
```

### sqrt()
`sqrt()`는 전달된 double형 값의 제곱근 값을 반환한다.

```java
Math.sqrt(25);      // 5
```

---
## references
- [https://tcpschool.com/java/java_api_math](https://tcpschool.com/java/java_api_math)