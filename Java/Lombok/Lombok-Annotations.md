### 목차
- [@Getter / @Setter](#getter--setter)
- [@AllArgsConstructor](#allargsconstructor)
- [@NoArgsConstructor](#noargsconstructor)
- [@RequiredArgsConstructor](#requiredargsconstructor)
- [@EqualsAndHashCode](#equalsandhashcode)
- [@ToString](#tostring)
- [@Data](#data)
- [@Builder](#builder)
- [@Delegate](#delegate)

---
## @Getter / @Setter
`@Getter`와 `@Setter` 어노테이션을 사용하면 어노테이션을 사용한 대상에 한하여 각각 Getter와 Setter 메서드를 자동으로 만들어준다.

```java
@Getter
public class Member {
    private String name;

    @Setter
    private String email;
    private String phoneNumber;
}
```

위 예제에서는 `Member` 클래스에 `@Getter`를 사용하여 클래스에 존재하는 모든 필드에 대해 Getter 메서드를 자동으로 생성하고, `email` 필드에만 `@Setter`를 사용하여 email 필드에 한해서만 Setter 메서드가 자동으로 생성된다.

즉, 다음과 같은 상태이다.

```java
public class Member {
    private String name;
    private String email;
    private String phoneNumber;

    public String getName() {
        return name;
    }

    public String getEmail() {
        return email;
    }

    public void setEmail(String email) {
        this.email = email
    }

    public String get phoneNumber() {
        return phoneNumber;
    }
}
```

## @AllArgsConstructor
`@AllArgsConstructor`은 모든 필드를 사용하는 생성자를 자동으로 생성하는 어노테이션이다.

```java
@AllArgsContructor
public class Member {
    private String name;
    private String email;
    private String phoneNumber;
}
```

위 코드는 다음과 같이 모든 필드를 사용하는 생성자를 자동생성한다.

```java
public class Member {
    private String name;
    private String email;
    private String phoneNumber;

    public Member(String name, String email, String phoneNumber) {
        this.name = name;
        this.email = email;
        this.phoneNumber = phoneNumber;
    }
}
```

## @NoArgsConstructor
`@NoArgsConstructor`은 모든 필드를 사용하는 생성자를 자동생성하는 어노테이션인 `@AllArgsConstructor`과 달리 어떠한 필드를 사용하지 않는 기본 생성자를 자동생성한다.

```java
@NoArgsConstructor
public class Member {
    private String name;
    private String email;
    private String phoneNumber;
}
```

위 코드는 다음과 같다.

```java
public class Member {
    private String name;
    private String email;
    private String phoneNumber;

    public Member() {}
}
```

## @RequiredArgsConstructor
`@RequiredArgsConstructor`은 특정 필드만 사용하는 생성자를 자동생성 해주는 어노테이션이다.

생성자에 추가할 필드에 `@NonNull` 어노테이션을 사용하여 생성자의 인자로 추가할 수 있다.

또는 해당 필드를 `final`로 선언해도 생성자의 인자로 추가할 수 있다.

```java
@RequiredArgsConstructor
public class Member {

    @NonNull
    private String name;
    private String email;
    private final String phoneNumber;
}
```

위 코드에서는 `name` 필드에 `@NonNull` 어노테이션이 사용되었기 때문에 생성자에 `name` 필드가 추가된다.

```java
public class Member {
    private String name;
    private String email;
    private String phoneNumber;

    public Member(String name, String phoneNumber) {
        this.name = name;
        this.phoneNumber = phoneNumber;
    }
}
```

## @EqualsAndHashCode

## @ToString

## @Data

## @Builder

## @Delegate

---
## references
- [https://mangkyu.tistory.com/78](https://mangkyu.tistory.com/78)