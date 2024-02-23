## AssertJ 사용법
### 목차
1. [Simple AssertJ Example](#simple-assertj-example)
2. [Test Fail Message - as](#test-fail-message---as)
    - []

## Simple AssertJ Example
```java
@Test
void simeple_assertJ_example() {
    String str = "example test text";
    assertThat(str).isNotNull()
            .startWith("example")
            .contains("test")
            .endWiths("text");
}
```

## Test Fail Message - as
`AssertJ`에서는 `as`를 사용하여 테스트 실패 시 보여줄 메시지를 표현할 수 있다.

- `as`는 검증문 앞에 작성해야 한다.

```java
@Test
void test() throws Exception {
    String str = "name";
    assertThat(str).as("값을 확인해주세요. 현재 값: %s", str)
                .isEqualsTo("name2");
}
```

## Collection Filtering - filteredOn
`AssertJ`에서는 `filteredOn`을 사용하여 테스트할 데이터들을 필터링할 수 있다.

```java
@Getter
@AllArgsConstructor
public class Member {
    private String name;
    private int age;
    private MemberType type;
}

enum MemberType{
    ADMIN, USER
}
```

위 예시 코드에서 객체가 가진 필드에 따라 필터링하여 다음과 같이 테스트를 할 수 있다.

```java
private Member dexter = new Member("dexter", 12, ADMIN);
private Member james = new Member("james", 30, ADMIN);
private Member park = new Member("park", 23, USER);
private Member lee = new Member("lee", 33, USER);

private List<Member> members = newArrayList(dexter, james, park, lee);
```

우선 위와 같이 `dexter`, `james`, `park`, `lee` 4개의 `Member` 객체를 생성하고, 이를 `members`라는 ArrayList에 넣어 놓는다고 가정한다.

<br>

#### 📌 filteredOn - 기본 사용
아래 예시는 기본적인 `filteredOn` 사용 예시이다.

`members`에 있는 `Member` 객체 중 `type`이 `ADMIN`인 객체를 필터링 했을 때, `dexter`와 `james`만을 포함하고 있는지를 확인한다.
```java
@Test
void filteringTestOne() {
    assertThat(members)
                .filteredOn("type", ADMIN)
                .containsOnly(dexter, james);
}
```

#### 📌 filteredOn - 람다식 사용
`filteredOn`은 아래 예시와 같이 람다식을 사용할 수 있다.

```java
@Test
void filteredTestTwo() {
    assertThat(members)
                .filteredOn(m -> m.getType() == USER)
                .containsOnly(park, lee);
}
```

#### 📌 filteredOn - 프로퍼티 접근 보조 메서드 사용
`filteredOn`은 아래 세 가지 메서드를 사용하여 프로퍼티 접근을 보조하도록 할 수 있다.
- not
- in
- notIn

```java
@Test
void filteringTestThree() {
    assertThat(members)
                .filteredOn("type", in(ADMIN, USER))
                .containsOnly(dexter, james, park, lee);
}
```

```java
@Test
void filteringTestFour() {
    assertThat(members)
                .filteredOn("type", not(ADMIN))
                .containsOnly(park, lee);
}
```

#### 📌 filteredOn - 연쇄 사용
`filteredOn`은 다음과 같이 연쇄적으로 사용이 가능하다.

```java
@Test
void filteringTestFive() {
    assertThat(members)
                .filteredOn("type", ADMIN)
                .filteredOn(m -> m.getAge() > 20)
                .containsOnly(james);
}
```


## Collection Data Extracting - extracting
`extracting`은 객체의 필드를 추출하여 테스트를 할 수 있다.

```java
@Test
void no_extracting_test() throws Exception {
    List<String> names = new ArrayList<>();
    
    for (Member member : members) {
        names.add(member.getName());
    }

    assertThat(names).containsOnly("dexter", "james", "park", "lee");
}
```

위 테스트 코드에서는 `extracting`을 사용하지 않고 생성된 `Member` 객체의 이름을 테스트한다.

이 경우, 따로 `Member` 객체의 이름을 가지고 있는 `names`라는 ArrayList를 만들어 이를 `containsOnly`를 사용하여 테스트 해야 한다.

하지만 아래와 같이 `extracting`을 사용하면 더욱 간결하게 표현할 수 있다.

```java
@Test
void extracting_test() throws Exception {
    assertThat(members).extracting("name")
                .containsOnly("dexter", "james", "park", "lee");
}
```

이때 추출할 데이터가 하나라면 타입 지정이 가능하다.

```java
@Test
void extracting_test() throws Exception {
    assertThat(members)
                .extracting("name", String.class)
                .contains("dexter", "james", "park", "lee");
}
```

### tuple

또한 여러 데이터를 추출하여 테스트를 할 때, `AssertJ`에서 제공하는 `tuple`을 사용하여 테스트를 할 수 있다.

```java
@Test
void extracting_test() throws Exception {
    assertThat(members)
                .extracting("name", "age")
                .contains(
                    tuple("dexter", 12),
                    tuple("james", 30),
                    tuple("park", 23),
                    tuple("lee", 33)
                );
}
```

## Soft Assertions
보통 여러 테스트를 진행할 때, 앞의 assertThat이 실패하면 다음 테스트는 중지된다.

`AssertJ`에서는 앞의 assertThat이 실패해도 다음 테스트가 중지되지 않도록 하는 `Soft Assertions`를 지원한다.

```java
@Test
void no_softAssertion_test() {
    assertThat(dexter.getAge())
                .as("Dexter Age Test").isEqualsTo(11);  // fail
    assertThat(james.getAge())
                .as("James Age Test").isEqualsTo(31);   // 실행되지 않음
    assertThat(park.getAge())
                .as("Park Age Test").isEqualsTo(24);    // 실행되지 않음
    assertThat(lee.getAge())
                .as("Lee Age Test").isEqualsTo(32);     // 실행되지 않음
}
```

위 테스트는 첫 번째 `assertThat`에서 실패하여 다음 `assertThat`이 실행되지 않는다.

이때 `softAssertion` 인스턴스를 만들어 이를 해결할 수 있다.

```java
@Test
void softAssertion_test() {
    SoftAssertion softAssertion = new SoftAssertion();

    softAssertion.assertThat(dexter.getAge())   // fail
                .isEqualsTo(11);                
    softAssertion.assertThat(james.getAge())    // fail
                .isEqualsTo(31);
    softAssertion.assertThat(park.getAge())     // fail
                .isEqualsTo(24);
    softAssertion.assertThat(lee.getAge())      // fail
                .isEqualsTo(32);

    softAssertion.assertAll();
}
```

마지막에 `assertAll`를 사용하여 테스트를 진행한다.

`assertSoftly`를 사용하면 `assertAll`을 사용하지 않아도 된다.

## Exception Assertion
`AssertJ`에서는 다음 메서드를 통해 예외를 테스트할 수 있다.

- assertThatThrownBy
- isInstance
- hasMassage

#### 📌 Exception Test - 기본 예외 발생 테스트
`assertThatThrownBy`는 예외가 발생하는지를 확인한다.
```java
@Test
void exceptionTest() {
    assertThatThrownBy(() -> new Member(null, 12, ADMIN));
}
```

특정 예외가 발생하는지 확인하기 위해서는 `isInstance(Class)`를 사용한다.

```java
@Test
void exceptionTest() {
    assertThatThrownBy(() -> new Member(null, 12, ADMIN))
                .isInstance(NullPointException.class);
}
```

#### 📌 Exception Test - 자주 발생하는 예외에 대한 테스트
자주 발생하는 Exception의 경우, `assertThatThrownBy` 대신 다음 메서드를 사용하여 검증할 수 있다.

- assertThatNullPointException
- assertThatIllegalArgumentException
- assertThatIllegalStateException
- assertThatIOException

#### 📌 Exception Test - 에러 메시지 테스트
`hasMessage`를 사용하여 에러 메시지에 대한 검증도 가능하다.

```java
@Test
void exceptionTest() {
    assertThatThrownBy(() -> new Member(null, 12, ADMIN))
                .isInstance(NullPointException.class)
                .hasMessage("에러 메시지");
}
```

## Custom Comparison Assertions

## Field Comparison

---
## references
- [https://sun-22.tistory.com/86](https://sun-22.tistory.com/86)
- [https://velog.io/@roycewon/AssertJ-Methods](https://velog.io/@roycewon/AssertJ-Methods)