### 목차
- [@Getter / @Setter](#getter--setter)
- [@AllArgsConstructor](#allargsconstructor)
- [@NoArgsConstructor](#noargsconstructor)
- [@RequiredArgsConstructor](#requiredargsconstructor)
    - [@NonNull](#nonnull)
- [@EqualsAndHashCode](#equalsandhashcode)
- [@ToString](#tostring)
- [@Data](#data)
- [@Builder](#builder)

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

`@NonNull` 어노테이션을 사용한 `name` 필드와 `final`로 선언한 `phoneNumber` 필드가 생성자에 추가되었다.

## @EqualsAndHashCode
`@EqualsAndHashCode`는 해당 클래스에 대한 `equals` 메서드와 `hashCode` 메서드를 자동으로 생성한다.

```java
@EqualsAndHashCode
public class Member {
    private String name;
    private String email;
    private String phoneNumber;
}
```

### @NonNull
`@NonNull`은 Null 값이 될 수 없다는 것을 명시하며, `NullPointerException`에 대한 대비책이 될 수 있다.

## @ToString
`@ToString`은 클래스의 필드를 기반으로 `ToString` 메서드를 자동 생성한다.

```java
@ToString
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

    @Override
    public String toString() {
        return "Member(name=" + this.id + ", email=" + this.name + ", phoneNumber=" + this.phoneNumber + ")";
    }
}
```

아래와 같이 `exclude`를 설정할 수 있다.

`@ToString`의 `exclude` 속성과 `@ToString.exclude` 어노테이션을 통해 `name` 필드와 `phoneNumber` 필드가 `ToString`에서 제외하였다.

```java
@ToString(exclude = "name")
public class Member {
    private String name;
    private String email;

    @ToString.Exclude
    private String phoneNumber;
}
```

또한 아래처럼 `@ToString(callSuper = true)`를 통해 상위 클래스에 대해서도 `ToString`을 적용시킬 수 있다.

```java
@ToString(callSuper = true)
public class Member {
    private String name;
    private String email;
    private String phoneNumber;
}
```

## @Data
`@Data`는 아래의 5개 어노테이션을 자동 적용시킨다.

- @Getter
- @Setter
- @RequiredArgsConstructor
- @EqualsAndHashCode
- @ToString

하지만 객체의 안정성을 위해 `@Data` 어노테이션 사용을 지양하는 것이 좋다.

## @Builder
`@Builder`는 해당 클래스의 객체 생성에 `Builder` 패턴을 적용시킨다.

```java
@Builder
public class Member {
    private String name;
    private String email;
    private String phoneNumber;
}
```

위처럼 `@Builder` 어노테이션을 사용하면 빌더 패턴을 사용한 객체를 생성할 수 있다.

```java
Member member = Member.builder()
                .name("회원 이름")
                .email("회원 이메일")
                .phoneNumber("회원 전화번호")
                .build();
```

만약 특정 필드만 `@Builder`를 적용하려면 다음과 같이 생성자를 작성하고 해당 생성자에 `@Builder`를 적용하면 된다.

```java
public class Member {
    private String name;
    private String email;
    private String phoneNumber;

    @Builder
    public Member(String name, String email) {
        this.name = name;
        this.email = email;
    }
}
```

```java
Member member = Member.builder()
                .name("회원 이름")
                .email("회원 이메일")
                .build();
```

---
## references
- [https://mangkyu.tistory.com/78](https://mangkyu.tistory.com/78)