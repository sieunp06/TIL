### 목차
- [JUnit5 Assert Methods](#junit5-assert-methods)
    - [assertEquals / assertNotEquals](#assertequals--assertnotequals)


---
## JUnit5 Assert Methods
> https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/Assertions.html

### assertEquals / assertNotEquals
`assertEquals`는 `expected`와 `actual`가 같은 값을 가졌는지 확인한다.

`expected`와 `actual`이 같은 값을 가졌으면 true를 반환하고, 같지 않으면 false를 반환한다.

```java
public static void assertEquals(Object expected, Object actual) {
    AssertEquals.assertEquals(expected, actual);
}
```
```java
public static void assertEquals(Object expected, Object actual, String message) {
    AssertEquals.assertEquals(expected, actual, message);
}
```

다음과 같이 `expected`와 `actual`을 비교한다.
```java
static void assertEquals(Object expected, Object actual, String message) {
    if (!objectsAreEqual(expected, actual)) {
        failNotEqual(expected, actual, message);
    }
}
```
`objectsAreEqual`을 통해 `expected`와 `actual`을 비교하고, 두 값이 다를 때 `failNotEqual`을 실행한다.
```java
static boolean objectsAreEqual(Object obj1, Object obj2) {
    if (obj1 == null) {
        return (obj2 == null);
    }
    return obj1.equals(obj2);
}
```
두 값이 다를 때 실행되는 `failNotEqual`은 `AssertionFailedError`를 발생시켜 테스트를 통과하지 못하도록 한다.
```java
static void failNotEqual(Object expected, Object actual, String message) {
    fail(format(expected, actual, message), expected, actual);
}

static void fail(String message, Object expected, Object actual) {
    throw new AssertionFailedError(message, expected, actual);
}
```

#### 📌 Example
다음은 정상적으로 테스트가 통과되는 예이다.
```java
@Test
public void assertEqauls_text() {
    String textText = "text";
    assertEquals(testText, "text");
}
```

만약 다음과 같이 두 값이 다르다면 `AssertionFailedError`가 발생하게 된다.

```java
@Test
public void assertEquals_test() {
    String testText = "text";
    assertEquals(testText, "not text");
}
```

![picture1](images/JUnit5-assert-Methods/JUnit5-assert-Method-1.png)

`expected`와 `actual`이 같지 않을 때 출력할 메시지를 마지막 인자인 `message`를 통해 지정할 수 있다.

```java
@Test
public void assertEquals_test() {
    String testText = "text";
    assertEquals(testText, "not text", "같지 않습니다.");
}
```

![picture2](images/JUnit5-assert-Methods/JUnit5-assert-Method-2.png)

### assertSame / assertNotSame
`assertSame`은 두 객체가 동일한 객체인지 확인한다.

```java
public static void assertSame(Object expected, Object actual) {
    AssertEquals.assertSame(expected, actual);
}
```
```java
public static void assertSame(Object expected, Object actual, String message) {
    AssertEquals.assertSame(expected, actual, message);
}
```

다음과 같이 `expected`와 `actual`을 비교한다.

`assertEquals`와 동일하게 두 객체를 비교하는 것에서 동일하지만, `assertEquals`는 두 값이 동일한지를 비교하고 `assertSame`은 두 객체가 동일한 객체인지 주소 값을 비교한다.

```java
static void assertSame(Object expected, Object actual, String message) {
    if (expected != actual) {
        failNotSame(expected, actual, message);
    }
}
```

`assertSame`이 주소 값을 통해 두 객체를 비교하는 메서드이기 때문에 `clone`을 통해 복사된 객체를 비교하면 테스트가 실패하게 된다.
```java
@Test
public void assertEquals_test() {
    ArrayList<Integer> list1 = new ArrayList<>();
    ArrayList<Integer> list2 = (ArrayList<Integer>) list1.clone();

    assertEquals(list1, list2);
}

@Test
public void assertSame_test() {
    ArrayList<Integer> list1 = new ArrayList<>();
    ArrayList<Integer> list2 = (ArrayList<Integer>) list1.clone();

    assertSame(list1, list2);
}
```

아래 사진에서 ArrayList를 clone하여 비교했을 때, 값을 비교하는 `assertEquals`는 테스트가 통과되는 반면 `assertSame`은 테스트가 통과되지 못했다.

![picture3](images/JUnit5-assert-Methods/JUnit5-assert-Method-3.png)

두 객체가 같지 않으면 `AssertionFailedError`를 발생시켜 테스트를 통과하지 못하게 한다.
```java
private static void failNotSame(Object expected, Object actual, String message) {
    fail(format(expected, actual, message), expected, actual);
}
```
```java
static void fail(String message, Object expected, Object actual) {
    throw new AssertionFailedError(message, expected, actual);
}
```

#### 📌 Example
다음은 정상적으로 테스트가 통과되는 예이다.

```java
@Test
public void assertEquals_test() {
    String testText = "text";
    assertSame(testText, "text");
}
```

### assertTrue / assertFalse
`assertTrue`는 값이 `true`인지 확인한다.

```java
public static void assertTrue(boolean condition) {
    AssertTrue.assertTrue(condition);
}
```

다음과 같이 전달 받은 `condition`이 true인지 false인지 확인한다.

```java
static void assertTrue(boolean condition) {
    assertTrue(condition, (String) null);
}

static void assertTrue(boolean condition, String message) {
    if (!condition) {
        fail(buildPrefix(message) + EXPECTED_TRUE, true, false);
    }
}
```

만약 `condition`이 false라면 `fail`을 실행시켜 `AssertionFailedError`를 발생시킨다.

```java
static void fail(String message, Object expected, Object actual) {
    throw new AssertionFailedError(message, expected, actual);
}
```

#### 📌 Example
다음은 정상적으로 테스트가 통과되는 예이다.

```java
@Test
public void assertTrue_test() {
    boolean test = true;

    assertTrue(test);
}
```

### assertNull / assertNotNull
`assertNull`는 값이 `null`인지 확인한다.

```java
public static void assertNull(Object actual) {
    AssertNull.assertNull(actual);
}
```

`actual`이 null인지 확인하고, null이 아닐 때 `failNotNull`을 실행한다.

```java
static void assertNull(Object actual) {
    assertNull(actual, (String) null);
}
```

```java
static void assertNull(Object actual, String message) {
    if (actual != null) {
        failNotNull(actual, message);
    }
}
```

`failNotNUll`에서는 `AssertFailedError`를 발생시킨다.

```java
private static void failNotNull(Object actual, String message) {
    String stringRepresentation = actual.toString();
    if (stringRepresentation == null || stringRepresentation.equals("null")) {
        fail(format(null, actual, message), null, actual);
    }
    else {
        fail(buildPrefix(message) + "expected: <null> but was: <" + actual + ">", null, actual);
    }
}

static void fail(String message, Object expected, Object actual) {
    throw new AssertionFailedError(message, expected, actual);
}
```

#### 📌 Example
다음은 정상적으로 테스트가 통과되는 예이다.
```java
@Test
public void assertNull_test() {
    String test = null;

    assertNull(test);
}
```

### assertAll
`assertAll`은 `assertAll` 안에 존재하는 모든 assertion을 확인한다.

```java
public static void assertAll(Executable... executables) throws MultipleFailuresError {
    AssertAll.assertAll(executables);
}
```

`notEmpty`를 통해 실행하려는 assertion들의 array가 비어있는지 확인한 후, 각각의 assertion이 비어있는지도 확인한다.

```java
static void assertAll(String heading, Executable... executables) {
    Preconditions.notEmpty(executables, "executables array must not be null or empty");
    Preconditions.containsNoNullElements(executables, "individual executables must not be null");
    assertAll(heading, Arrays.stream(executables));
}
```

그 이후 `assertAll`을 실행한다.

```java
static void assertAll(String heading, Stream<Executable> executables) {
    Preconditions.notNull(executables, "executables stream must not be null");

    List<Throwable> failures = executables //
            .map(executable -> {
                Preconditions.notNull(executable, "individual executables must not be null");
                try {
                    executable.execute();
                    return null;
                }
                catch (Throwable t) {
                    UnrecoverableExceptions.rethrowIfUnrecoverable(t);
                    return t;
                }
            }) //
            .filter(Objects::nonNull) //
            .collect(Collectors.toList());

    if (!failures.isEmpty()) {
        MultipleFailuresError multipleFailuresError = new MultipleFailuresError(heading, failures);
        failures.forEach(multipleFailuresError::addSuppressed);
        throw multipleFailuresError;
    }
}
```

#### 📌 Example
다음은 정상적으로 테스트가 통과되는 예이다.

```java
@Test
public void assertAll_test() {
    String text = "sieun";

    assertAll(
        () -> assertEquals(text, "sieun"),
        () -> assertSame(text, "sieun")
    );
}
```

### assertThrows
`assertThrows`는 예상하는 exception이 발생하는지 확인한다.

```java
@Test
public void assertThrow_test() {
    IllegalArgumentException illegalArgumentException =
        assertThrows(IllegalArgumentException.class, () -> new Study(10, -1));
    String message = illegalArgumentException.getMessage();
    assertEquals("최소 참석인원은 0 보다 커야 합니다.", message);
}
```

---
## references
- [https://incheol-jung.gitbook.io/docs/study/undefined-3/chap-05.-junit-5](https://incheol-jung.gitbook.io/docs/study/undefined-3/chap-05.-junit-5)
- [https://velog.io/@roycewon/JUnit-Assert-Methods1](https://velog.io/@roycewon/JUnit-Assert-Methods1)
- [http://blog.iotinfra.net/?p=1851](http://blog.iotinfra.net/?p=1851)
- [https://gracelove91.tistory.com/108](https://gracelove91.tistory.com/108)