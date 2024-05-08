### 목차

---
# PasswordEncoder
`PasswordEncoder`는 Spring Security에서 지원하는 비밀번호 단방향 암호화 인터페이스이다.

<br>

클라이언트가 비밀번호를 입력했을 때, 해싱 알고리즘을 통해 데이터베이스에서 비밀번호를 가져와 값이 일치하는지 비교한다.

```java
public interface PasswordEncoder {
	// 비밀번호 단방향 암호화
	Stirng encode(CharSequence rawPassword);
	
	// 암호화되지 않은 비밀번호와 암호화된 비밀번호가 일치하는지 비교
	boolean matches(CharSequence rawPassword, String encodedPassword);
	
	// 암호화된 비밀번호를 다시 암호화하고자 할 경우 true를 return하도록 설정
	default boolean upgradeEncoding(String encodedPassword) {
		return false;
	}
}
```

## PasswordEncoder의 구현 클래스
Spring Security는 PasswordEncoder의 구현체로 다음을 제공한다.
- DelegatingPasswordEncoder
- BCryptPasswordEncoder
- Pbkdf2PasswordEncoder
- SCryptPasswordEncoder
- Argon2PasswordEncoder

위 구현 클래스 중 `BCryptPasswordEncoder`에 대해 자세히 알아보도록 한다.

## BCryptPasswordEncoder
`BCryptPasswordEncoder`는 `bcrypt` 해시 알고리즘을 통해 비밀번호를 암호화한다.

<br>

`Bruteforce attack`이나 `Rainbow table attack`과 같은 Password checking에 대한 저항력을 높이기 위해 의도적으로 느리게 설정되어 있다.

```java
public BCryptPasswordEncoder(BCryptVersion version, int strength, SecureRandom random) {
		if (strength != -1 && (strength < BCrypt.MIN_LOG_ROUNDS || strength > BCrypt.MAX_LOG_ROUNDS)) {
			throw new IllegalArgumentException("Bad strength");
		}
		this.version = version;
		this.strength = (strength == -1) ? 10 : strength;
		this.random = random;
	}
```
`BCryptPasswordEncoder`의 속도는 강도(strength)를 통해 조절할 수 있으며, 10을 default로 하고 4 ~ 31 사이로 설정할 수 있다.

```java
BCryptPasswordEncoder encoder = new BCryptPasswordEncoder(16);
```

### 동작 원리
다음은 비밀번호를 해싱하는 `encode` 메서드이다.
```java
@Override
public String encode(CharSequence rawPassword) {
    if (rawPassword == null) {
        throw new IllegalArgumentException("rawPassword cannot be null");
    }
    String salt = getSalt();
    return BCrypt.hashpw(rawPassword.toString(), salt);
}
```



```java
private String getSalt() {
    if (this.random != null) {
        return BCrypt.gensalt(this.version.getVersion(), this.strength, this.random);
    }
    return BCrypt.gensalt(this.version.getVersion(), this.strength);
}
```

