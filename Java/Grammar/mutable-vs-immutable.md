# Mutable 객체와 immutable 객체

## Mutable 객체
`Mutable` 객체는 가변 객체로, 데이터 변경이 가능한 객체를 말한다.

## Immutable 객체
`Immutable` 객체는 불변 객체로, 데이터 변경이 불가능한 객체를 말한다.

<br>

`Immutable` 객체의 종류 중 대표적인 것들로는 다음과 같다.
- String
- Boolean
- Integer
- Float
- Long
- Double

### Immutable 객체는 수정이 가능하다?
`Immutable` 객체는 불변 객체이기 때문에 수정이 불가능해야 한다.

```java
Integer number = 1;
number = 3;
```

하지만 위 예시를 보면 같이 값의 수정이 가능한 것처럼 보인다.

<br>

수정이 불가능하다는 것은 사실 정확하게는 `힙 영역에 저장된 값을 수정할 수 없는 것`이다.

즉, 힙 영역에 저장된 값이 수정된 것이 아니라 힙 영역에 새로운 객체가 새로 생성되고 해당 객체에 대한 참조값을 변경한 것이다.

<br>

```java
Integer number = 1;
number = 3;             // 힙 영역에 새 객체 생성
```

`1`이 할당되었던 객체는 `Garbage`로 남아있다 `Garbage Collection`에 의해 사라진다.

> 📌 [Garbage Collection 가비지 컬랙션]()

### Immutable 객체를 만드는 방법
#### 👉 final
`final` 객체를 사용하면 참조값을 변경할 수 없기 때문에 불변성을 확보할 수 있다.

```java
final String text = "example text";
text = "modified text";     // compile error
```

위처럼 `final` 키워드를 붙이고 변수의 값을 변경하려고 하면 컴파일 에러가 발생한다.

<br>

하지만 아래 예시를 보면 `final`을 사용하여 `List`를 선언했지만, 아무 문제 없이 객체가 `add` 되는 것을 볼 수 있다.

```java
final List<String> list = new ArrayList<>();
list.add("text");
```

이를 방지하기 위해 불변 클래스를 만들어야 한다.

#### 👉 불변 클래스


> 📌 [Effective Java 3/E - item 17. 변경 가능성을 최소화하라](https://github.com/sieunp06/TIL/blob/main/Book/Effective%20Java/4-Classes%20and%20Interfaces/Item17-Minimize-mutability.md)

---
## references
- [https://choiblack.tistory.com/47](https://choiblack.tistory.com/47)
- [https://mangkyu.tistory.com/131](https://mangkyu.tistory.com/131)