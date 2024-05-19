# Spring JPA “not-null property references a null or transient value” 해결
## 🐛ISSUE
![p1](./images/not-null%20property%20references%20a%20null%20or%20transient%20value/not-null-property-references-a-null-or-trasient-value-1.png)

`User` 엔티티의 `createAt`에서 다음과 같은 오류가 발생하였다.

```
not-null property references a null or transient value
```

## ❗CAUSE

위 오류가 발생하였을 때 `User` 엔티티는 다음과 같았다.

```java
@CreatedDate
@Column(name = "user_created_at", nullable = false)
private LocalDate createdAt;         // 가입일
```

`createAt`은 not-null로 설정되어 있는데, `User` 객체를 만들 때 해당 값이 null이거나 transient 값이 라는 것이었다.

<br>

잘 살펴보니 `User` 클래스의 생성자에서 `createAt`을 잊었던 것이다!

```java
@Builder
private User(String userEmail, String userPassword, String userNickName) {
    this.userEmail = userEmail;
    this.userPassword = userPassword;
    this.userNickName = userNickName;
}
```

## ✨SOLUTIONS
다음과 같이 `LocalDate.now()`를 통해 현재 시간을 createAt 값으로 저장할 수 있도록 변경하였다.
```java
@Builder
private User(String userEmail, String userPassword, String userNickName) {
    this.userEmail = userEmail;
    this.userPassword = userPassword;
    this.userNickName = userNickName;
    this.createdAt = LocalDate.now();
}
```