### 목차
- [BCrypt](#bcrypt)
- [BCryptPasswordEncoder](#bcryptpasswordencoder)
  - [BCryptPasswordEncoder의 동작원리](#bcryptpasswordencoder의-동작원리)
  - [BCryptPasswordEncoder의 일치 여부 확인](#bcryptpasswordencoder의-일치-여부-확인)

---
## BCrypt
`BCrypt`는 블로피시 암호에 기반을 둔 해시 함수로, 내부에서 랜덤한 Salt 값을 생성하여 해싱한다.

> 📌 [솔팅(Salting)](https://github.com/sieunp06/TIL/blob/main/Security/encryption-methods.md#-%EC%86%94%ED%8C%85salting)

## BCryptPasswordEncoder
`BCryptPasswordEncoder`는 `PasswordEncoder`의 구현 클래스 중 하나로 `BCrypt` 해시 알고리즘을 통해 비밀번호를 암호화한다.

<br>

다음과 같이 빈으로 등록해서 사용할 수 있다.
```java
@Bean
PasswordEncoder passwordEncoder() {
    return new BCryptPasswordEncoder();
}
```

<br>

`BCryptPasswordEncoder`는 브루트포스 공격이나 레인보우 테이블 공격에 대한 저항력을 높이기 위해 의도적으로 느리게 설정되어 있다.

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

이때, 해당 속도를 강도(Strength)를 통해 조절할 수 있으며 10을 default로 하고 4 ~ 31 사이로 설정할 수 있다.


```java
BCryptPasswordEncoder encoder = new BCryptPasswordEncoder(16);
```

### BCryptPasswordEncoder의 동작원리
아래 `encode()` 메서드는 비밀번호를 해싱해주는 역할을 한다.
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

해당 메서드의 코드를 살펴보면 `getSalt()` 메서드를 통해 salt 값을 생성하고 `hashpw()` 메서드에서 최종적으로 해싱된 비밀번호를 갖게 된다.

<br>

`getSalt()` 메서드에서는 `gensalt()` 메서드를 통해 salt 값을 생성하여 반환한다.
```java
private String getSalt() {
    if (this.random != null) {
        return BCrypt.gensalt(this.version.getVersion(), this.strength, this.random);
    }
    return BCrypt.gensalt(this.version.getVersion(), this.strength);
}
```

다음은 `gensalt()` 메서드이다.

```java
public static String gensalt(String prefix, int log_rounds, SecureRandom random) throws IllegalArgumentException {
    StringBuilder rs = new StringBuilder();
    byte rnd[] = new byte[BCRYPT_SALT_LEN];

    if (!prefix.startsWith("$2")
            || (prefix.charAt(2) != 'a' && prefix.charAt(2) != 'y' && prefix.charAt(2) != 'b')) {
        throw new IllegalArgumentException("Invalid prefix");
    }
    if (log_rounds < 4 || log_rounds > 31) {
        throw new IllegalArgumentException("Invalid log_rounds");
    }

    random.nextBytes(rnd);

    rs.append("$2");
    rs.append(prefix.charAt(2));
    rs.append("$");
    if (log_rounds < 10) {
        rs.append("0");
    }
    rs.append(log_rounds);
    rs.append("$");
    encode_base64(rnd, rnd.length, rs);
    return rs.toString();
}
```

위 메서드로 생성된 salt 값은 다음과 같은 형태를 가지고 있다.

```
salt = BCryptVersion + "$" + log_rounds + "$" + real_salt 
```

해당 salt를 이용해 최종적으로 해싱되는 값은 다음과 같다.

```
salt 값 + (원문 + salt 값을 해싱한 값)
```

### BCryptPasswordEncoder의 일치 여부 확인
다음은 `BCryptPasswordEncoder`의 일치 여부를 확인하는 `matches()` 메서드이다.
```java
@Override
public boolean matches(CharSequence rawPassword, String encodedPassword) {
    if (rawPassword == null) {
        throw new IllegalArgumentException("rawPassword cannot be null");
    }
    if (encodedPassword == null || encodedPassword.length() == 0) {
        this.logger.warn("Empty encoded password");
        return false;
    }
    if (!this.BCRYPT_PATTERN.matcher(encodedPassword).matches()) {
        this.logger.warn("Encoded password does not look like BCrypt");
        return false;
    }
    return BCrypt.checkpw(rawPassword.toString(), encodedPassword);
}
```

`matches()` 메서드에서는 `checkpw()` 메서드를 거치게 된다.

```java
public static boolean checkpw(String plaintext, String hashed) {
    byte[] passwordb = plaintext.getBytes(StandardCharsets.UTF_8);
    return equalsNoEarlyReturn(hashed, hashpwforcheck(passwordb, hashed));
}
```

이때, `checkpw()` 메서드를 보면 `hashpwforcheck()` 메서드를 사용하게 되는데, 해당 메서드는 앞에서 사용했던 `hashpw()` 메서드를 사용한다.

```java
private static String hashpwforcheck(byte[] passwordb, String salt) {
    return hashpw(passwordb, salt, true);
}
```

plainText와 저장된 해시값을 넣고, 해시값에서 salt를 추출하여 plainText와 salt 값으로 다시 해싱한 후 해당 plainText를 비교한다.

> 🤔 salt 값이 노출되어도 되는 이유?<br>
> 솔팅 기법은 사실상 레인보우 테이블 공격을 막기 위해 고안된 방식이며, 해당 값이 노출되어도 레인보우 테이블이 만들어지기는 어렵다.

---
## references
- [https://wildeveloperetrain.tistory.com/175](https://wildeveloperetrain.tistory.com/175)
- [https://jhkimmm.tistory.com/24](https://jhkimmm.tistory.com/24)