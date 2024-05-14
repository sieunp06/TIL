### 목차
- [ResponseEntity](#responseentity)
  - [ResponseEntity의 사용](#responseentity의-사용)
    - [📌 생성자 패턴](#-생성자-패턴)
    - [📌 빌더 패턴](#-빌더-패턴)
    - [생성자 패턴과 빌더 패턴 중 어떤 패턴을 사용하면 좋을까?](#생성자-패턴과-빌더-패턴-중-어떤-패턴을-사용하면-좋을까)
  - [ResponseEntity의 Body 타입](#responseentity의-body-타입)

---
## ResponseEntity
`ResponseEntity`는 `httpentity`를 상속받는 클래스로, `HttpStatus`, `HttpHeader`, `HttpBody`를 포함한다.

적절한 상태 코드와 응답 헤더 및 응답 본문을 생성해서 클라이언트에 전달할 수 있다.

```java
@GetMapping("/")
public ResponseEntity<User> getUser() {
    User user = userService.getUser();
    return ResponseEntity.ok(user);
}
```

### ResponseEntity의 사용
#### 📌 생성자 패턴
```java
return new ReponseEntity(body, headers, HttpStatus.valueOf(200));
```

#### 📌 빌더 패턴
`ResponseEntity.ok()`를 사용한다.
```java
return ResponseEntity.ok()
    .headers(headers)
    .body(body);
```

#### 생성자 패턴과 빌더 패턴 중 어떤 패턴을 사용하면 좋을까?
> 생성자 패턴보다는 빌더 패턴을 사용하길 권장한다.

위 두 패턴을 살펴보면, 생성자 패턴은 상태코드를 숫자로 입력하도록 되어 있다.

때문에, 생성자 패턴을 사용하면 해당 상태코드를 잘못 작성할 여지가 있다.

```java
return ResponseEntity.ok()
    .headers(headers)
    .body(body);
```

위와 같이 빌더 패턴을 사용하면 상태코드를 숫자로 입력하지 않아도 된다.


### ResponseEntity의 Body 타입
```java
public ResponseEntity getUser {
    ...
}
```
다음과 같이 Body 타입을 명시하지 않고 모든 Object 타입을 입력 받는다.

즉, 다음 코드와 같다.
```java
public ResponseEntity<Object> getUser {
    ...
}
```

---
## references
- [https://tecoble.techcourse.co.kr/post/2021-05-10-response-entity/](https://tecoble.techcourse.co.kr/post/2021-05-10-response-entity/)
- [https://stir.tistory.com/343](https://stir.tistory.com/343)