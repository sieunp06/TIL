## @Data
`@Data`는 아래의 5개 어노테이션을 자동 적용시킨다.

- @Getter
- @Setter
- @RequiredArgsConstructor
- @EqualsAndHashCode
- @ToString

## @Data를 지양해야 하는 이유
### ✨ @Setter: Setter의 남용
`@Data`는 모든 필드에 Setter를 적용시키기 때문에, 변경이 없어야 하는 필드도 Setter 메서드가 생성된다.

이러한 Setter의 남용은 객체의 안정성을 보장하기 힘들다.

### ✨ @RequiredArgsConstructor: 같은 타입인 필드의 위치 변경
`Lombok`은 `@AllArgsConstructor`나 `@AllArgsConstructor`를 사용할 때, 생성자의 파라미터 순서를 필드 선언 순서에 따라 변형하게 된다.

이때 아래 예시와 같이 개발자의 의도와 다르게 같은 타입의 값이 올바르지 않은 필드에 들어가는 문제가 발생할 수 있다.
```java
public class RequiredArgConstructorTest {
    @AllArgsConstructor
    public static class HighSchoolStudent {
        private int age;
        private int grade;
    }

    public static void main(String[] args) {
        HighSchoolStudent student = new HighSchoolStudent(1, 17);
    }
}
```

### ✨ @EqualsAndHashCode: mutable 객체에서의 사용

> 📌 [@EqualsAndHashCode](https://github.com/sieunp06/TIL/blob/main/Java/Lombok/Lombok-Annotations.md#equalsandhashcode)

> 📌 [mutable vs immutable]()

`@EqualsAndHashCode`를 `mutable` 객체에서 사용할 경우, 다음과 같은 문제가 발생할 수 있다.

```java
@EqualsAndHashCode
class Member {
    private Long id;
    private String name;
    private int age;

    public MainClass(Long id, String name, int age) {
        this.id = id;
        this.name = name;
        this.age = age;
    }

    public void setAge(int age) {
        this.age = age;
    }
}
```

mutable 객체인 `Member`에 `@EqualsAndHashCode`를 사용했을 때, 아래와 같이 필드 값을 변경하자 동일한 객체임에도 찾지 못한다.

```java
public static void main(String[] args) {
    Member member = new Member(1L, "name1", 10);

    Set<Member> members = new HashSet<>();
    members.add(member);

    System.out.printf("변경전 = %s \n", members.contains(member));
    //변경전 = true
    mainClass.setAge(20);
    System.out.printf("변경후 = %s", members.contains(member));
    //변경후 = false
}
```

<br>

이를 해결하기 위해서는 `@EqualsAndHashCode(of = {필드명})`을 통해 동등성 비교에 필요한 필드를 지정해야 한다.

```java
@EqualsAndHashCode(of = {"id"})
class Member {
    ...
}
```
```java
public static void main(String[] args) {
    ...

    System.out.printf("변경전 = %s \n", members.contains(member));
    //변경전 = true
    mainClass.setAge(20);
    System.out.printf("변경후 = %s", members.contains(member));
    //변경후 = true
}
```


### ✨ @ToString: 양방향 연관관계에서의 ToString의 순환 참조
JPA에서 1:N 연관관계 양방향 참조시 `@ToString`을 사용하면 순환 참조 문제가 발생할 수 있다.

<br>

아래 예시에서는 `User`와 `Coupon`이 1:N 양방향 연관관계를 맺고 있다.

```java
@ToString
@Entity
public class User {
		@OneToMany
		@JoinColumn(name = "coupon_id")
		private List<Coupon> coupons = new ArrayList<>();
}

@ToString
@Entity
public class Coupon {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    @ManyToOne
    private User user;
    
    public Coupon(User user) {
            this.user = user;
    }
}
```

만약 `User`를 조회했을 때, `User`의 `ToString`이 호출되고 `User`와 연관관계를 맺고 있는 `Coupon`을 호출한다.

이때 `Coupon`과 연관관계를 맺고 있는 `User`를 또 다시 호출하면서 무한 참조가 발생하여 `StackOverFlowError`가 발생한다.

<br>

이때, `@ToString(exclude = "")`를 통해 특정 항목을 제외시킴으로써 위 문제를 해결할 수 있다.

---
## references
- [https://riimy.tistory.com/83](https://riimy.tistory.com/83)
- [https://roopredev.tistory.com/14](https://roopredev.tistory.com/14)
- [https://dev-code-notepad.tistory.com/153](https://dev-code-notepad.tistory.com/153)
- [https://assets.velcdn.com/@penrose_15/Data%EC%97%90-%EB%8C%80%ED%95%9C-%EA%B3%A0%EC%B0%B0](https://assets.velcdn.com/@penrose_15/Data%EC%97%90-%EB%8C%80%ED%95%9C-%EA%B3%A0%EC%B0%B0)