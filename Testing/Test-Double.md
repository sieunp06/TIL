## Test Double이란?
`Test Double`이라는 용어는 `xUnit Test Patterns`의 저자인 제라드 메스자로스가 만든 용어이다.

> 테스트를 진행하기 어려운 경우 이를 대신해 테스트를 진행할 수 있도록 만들어주는 객체

### Test Double이 필요한 이유
데이터베이스로부터 조회한 값을 연산하는 로직을 구현했다고 할 때, 해당 로직을 테스트하기 위해서는 데이터베이스의 영향을 받게 된다.

이는 다른 말로 데이터베이스의 상태에 따라 다른 결과가 발생할 수 있다는 뜻이다.

## Test Double의 종류
### Dummy
`Dummy`는 가장 기본적인 Test Double이다.

- 객체가 필요하지만, 기능은 필요하지 않은 경우에 사용한다.
- Dummy 객체의 메서드가 호출되었을 때 정상 동작은 보장하지 않는다.

```java
public interface PrintWarning {
    void print();
}
```
```java
public class PrintWarningDummy implements PrintWarning {
    @Override
    public void print() {
        // 구현 안함.
    }
}
```

위 예시에서는 `PrintWarning` 인터페이스의 구현체를 구현하지 않았다.

`print`는 로그 출력을 위한 구현체기 때문에 테스트 환경에서는 필요가 없기 때문이다.

이렇게 테스트 환경에서 영향을 미치지 않는 객체를 `Dummy`라고 한다.

### Fake
`Fake`는 실제 객체와 동일한 역할을 하도록 만들어 사용하는 객체이다.

```java
public interface UserRepository {
    void save(User user);
    User findById(Long id);
}
```
```java
public class FakeUserRepository implements UserRepository {
    private Collection<User> users = new ArrayList<>();

    @Override
    public void save(User user) {
        if (findById(user.getId()) == null) {
            users.add(user);
        }
    }

    @Override
    public User findById(Long id) {
        for (User user : users) {
            if (user.getId() == id) {
                return user;
            }
        }
        return null;
    }
}
```
테스트 해야할 객체가 데이터베이스와 연관되어 있다고 할 때, 위 예시와 같이 `FakeUserRepository`를 만들어 테스트 객체에 주입할 수 있다.

### Stub
`Stub`은 `Dummy` 객체가 실제로 동작하는 것처럼 보이게 만들어 놓은 객체이다.

즉, 테스트를 위해 의도된 결과만 반환되도록 한다.

```java
public class StubUserRepository implements UserRepository {
    @Override
    public User findById(Long id) {
        return new User(id, "Test User");
    }
}
```

위와 같이, `findById()` 메서드를 사용했을 때 항상 Test User를 반환하도록 한다.

하지만, `findById()` 메서드가 변경되면 `StubUserRepository`의 `findById()`도 변경해야 한다.

### Spy
`Spy`는 `Stub`의 역할을 가지면서 호출된 내용에 대한 약간의 정보를 기록한다.

```java
public class MailingService {
    private long sendMailCount = 0;
    private Collection<Mail> mails = new ArrayList<>();

    public void sendMail(Mail mail) {
        sendMailCount++;
        mails.add(mail);
    }

    public long getSendMailCount() {
        return sendMailCount;
    }
}
```

위 예시에서 `sendMail()` 메서드를 실행하면 `sendMailCount` 변수를 증가시키고 `getSendMailCount()` 메서드를 통해 해당 변수에 저장된 값을 반환한다.

`Mockito` 프레임워크의 `verify()` 메서드와 같은 역할을 한다.

### Mock
`Mock`은 호출에 대한 기대를 명세하고 내용에 따라 동작하도록 프로그래밍된 객체이다.

```java
@ExtendWith(MockitoExtension.class)
public class UserServiceTest {
    @Mock
    private UserRepository userRepository;

    @Test
    void test() {
        when(userRepository.findById(anyLong())).thenReturn(new User(1, "Test User"));

        User actual = userService.findById(1);
        assertThat(actual.getId()).isEqualTo(1);
        assertThat(actual.getName()).isEqualTo("Test User");
    }
}
```

위 예시를 보면 어떤 결과 반환할지 결정할 수 있다.

---
## references
- [https://tecoble.techcourse.co.kr/post/2020-09-19-what-is-test-double/](https://tecoble.techcourse.co.kr/post/2020-09-19-what-is-test-double/)