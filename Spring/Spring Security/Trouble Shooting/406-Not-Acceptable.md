# Spring Security “406 Not Acceptable” 해결
## 🐛ISSUE
`Postman`으로 일반 회원가입 POST 테스트를 진행하다 `406 Not Acceptable` 오류가 발생하였다.

> 📌 406 Not Acceptable<br>
> 콘텐츠 협상에 일치하는 것이 없음

## ❗CAUSE & ✨SOLUTIONS
해당 오류는 `signup` 메서드가 리턴하는 `UserResponseDto`에 Getter가 없어서 발생하는 오류라고 한다.

```java
public UserResponseDto signup(UserSignupDto userSignupDto) {
    String userEmail = userSignupDto.getUserEmail();
    String encodedPassword = passwordEncoder.encode(userSignupDto.getUserPassword());
    String userNickName = userSignupDto.getUserNickName();

    validateDuplicateUser(userEmail);

    User savedUser = userRepository.save(User.builder()
            .userEmail(userEmail)
            .userPassword(encodedPassword)
            .userNickName(userNickName)
            .build());

    return UserResponseDto.from(savedUser);
}
```

위와 같이 `signup` 메서드는 `UserResponseDto`를 반환하는데, 해당 클래스를 들어가보면 @Getter가 존재하지 않았다.

```java
@Getter
public class UserResponseDto {
    private String userEmail;
    private String userNickName;

    @Builder
    private UserResponseDto(String userEmail, String userNickName) {
        this.userEmail = userEmail;
        this.userNickName = userNickName;
    }

    public static UserResponseDto from(User user) {
        return UserResponseDto.builder()
                .userEmail(user.getUserEmail())
                .userNickName(user.getUserNickName())
                .build();
    }
}
```
그래서 붙여줬다..!