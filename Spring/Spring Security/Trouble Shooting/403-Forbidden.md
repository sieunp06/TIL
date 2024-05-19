# Spring Security “403 Forbidden” 해결
## 🐛ISSUE
`Postman`으로 일반 회원가입 POST 테스트를 진행하다 `403 Forbidden` 오류가 발생하였다.

> 📌 403 Forbidden<br>
> 지정한 리소스에 대한 엑세스가 금지되었다.

## ❗CAUSE & ✨SOLUTIONS
`403 Forbidden`이 발생한 이유는 csrf 때문이라고 한다.

때문에 다음과 같이 Spring Security Config에서 csrf를 disable 해줬다.

```java
@Bean
SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
    http.httpBasic(AbstractHttpConfigurer::disable)
            .csrf(AbstractHttpConfigurer::disable)
            .formLogin(AbstractHttpConfigurer::disable)
            .authorizeHttpRequests((authorize) -> authorize
                    .requestMatchers("user/**").permitAll())
    ;
    return http.build();
}
```

위 securityFilterChain에서 다음 부분이 csrf를 disable 한 부분이다.

```java
.csrf(AbstractHttpConfigurer::disable)
```