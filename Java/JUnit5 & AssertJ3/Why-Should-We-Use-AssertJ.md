## AssertJ를 사용해야 하는 이유
### 가독성
`AssertJ`를 사용하면 가독성이 훨씬 좋아진다.
```java
// JUnit
assertEquals(a, b);

// AssertJ
assertThat(a).isEquals(b);
```

`JUnit`을 사용한 코드를 보면 어떤 것이 실제 값이고 어떤 값이 예상 값인지 한 눈에 파악되지 않는다.

하지만 `AssertJ`를 사용한 코드는 한 눈에 `a`가 실제 값이고, `b`가 예상 값이라는 것을 알 수 있다.

또한 왼쪽에서 오른쪽으로 자연스럽게 읽힌다.

### 자세한 실패 메시지
`AssertJ`는 `JUnit`을 사용했을 때보다 더 자세한 실패 메시지를 얻을 수 있다.

```java
// JUnit
assertTrue(name.contains("o"));
```
```
expected: <true> but was: <false>
Expected :true
Actual   :false
```
`JUnit`을 사용한 테스트 코드의 결과는 예상 값이 true지만 실제로는 false가 나와 실패했다는 것만 알 수 있다.

<br>

반면 동일한 내용의 코드를 `AssertJ`로 작성하여 테스트를 실행하면 다음과 같은 결과가 나온다.

```java
// AssertJ
assertThat(name).contains("o");
```
```
java.lang.AssertionError:
Expecting actual:
  "sieun"
to contain:
  "o"
```

`AssertJ`를 사용한 테스트 코드의 결과는 실제 값은 `"sieun"`이고, 이 값에 `"o"`가 없기 때문에 `AssertionError`가 발생했다는 것을 알 수 있다.

<br>

`AssertJ`를 사용하면 `JUnit`만을 사용했을 때보다 더욱 자세한 실패 메시지로 실패 원인을 더욱 자세히 파악할 수 있다.

### 다양한 검증 메서드 제공
```java
// JUnit
assertTrue(winners.containsAll(List.of("애쉬", "스플릿")) && winners.size() == 2);
assertArrayEquals(winners.toArray(), new String[]{"애쉬", "스플릿"});
assertTrue(winners.containsAll(List.of("애쉬", "스플릿")));

// AssertJ
assertThat(winners).containsExactlyInAnyOrder("애쉬", "스플릿");
assertThat(winners).containsExactly("애쉬", "스플릿");
assertThat(winners).contains("애쉬", "스플릿");
```

위 예시를 보면 상대적으로 `JUnit`은 `AssertJ`보다 더욱 많은 과정을 거쳐 검증을 해야 함을 알 수 있다.

### 메서드 체이닝
`AssertJ`는 메서드 체이닝을 지원하여 여러 테스트를 한번에 할 수 있다.

```java
assertThat("[TITLE] Hello, My Name is ash")
        .isNotEmpty()
        .contains("[TITLE]")
        .containsOnlyOnce("ash")
        .doesNotStartWith("[ERROR]");
```

또한 위에서 언급한 것처럼 가독성이 매우 좋다.

--- 
## references
- [https://xxeol.tistory.com/12](https://xxeol.tistory.com/12)