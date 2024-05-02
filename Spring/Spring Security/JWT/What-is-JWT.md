### 목차

---
# JWT(JSON Web Token)이란?
`JWT`는 JSON Web Token의 약자로, JSON 객체로 정보를 주고 받을 때 안전하게 전송하기 위한 방식이다.

주로 로그인 인증 구현 등에 사용된다.

<br>

다음 두 가지 방법으로 JWT를 구현할 수 있다.

1. jwt-java 라이브러리를 사용하여 JWT를 생성하고 파싱하는 방법
2. spring security 라이브러리를 사용하는 방법

## JWT의 구조
JWT는 `.`을 구분자로 하여 아래 세 가지 문자열로 구성된다.
- Header 헤더
- Payload 내용
- Signature 서명

EX) aaaaaa.bbbbbb.cccccc

### 📌 Header 헤더
`Header`는 다음 두 가지 정보를 가지고 있다.
- `typ`: 토큰 타입을 지정한다.
- `alg`: 해싱 알고리즘을 지정한다.

> 💡 보통 해싱 알고리즘은 `HMAC SHA256`이나 `RSA`를 사용하며, 토큰을 검증하는 Signature 부분에서 사용된다.

### 📌 Payload 내용
`Payload`에는 토큰을 담을 정보가 들어있다.

위 토큰에 담긴 정보의 한 조각을 `claim`이라고 하며, name과 value 한쌍으로 이루어져 있다.

하나의 토큰에 여러 claim을 담을 수 있다.

#### ✨ Claim의 종류
`Claim`은 다음 세 가지 분류로 나누어진다.
- registered claim
- public claim
- private claim

<br>

1. registered claim<br>
    서비스에서 필요한 정보가 아닌, 토큰에 대한 정보들을 담기 위해 이름이 미리 정해진 claim이다.

    해당 registered claim들의 사용은 모두 선택적이다.
    - `iss`: 토큰 발급자(issuer)
    - `sub`: 토큰 제목(subject)
    - `aud`: 토큰 대상자(audience)
    - `exp`: 토큰의 만료시간(expiration), 항상 현재 시간보다 이후로 설정해야 한다.
    - `nbf`: Not Before을 의미하며, 토큰의 활성 날짜와 유사한 개념이다. 해당 날짜가 지나기 전까지는 토큰이 처리되지 않는다.
    - `iat`: 토큰이 발급된 시간(issued at), 해당 claim을 통해 토큰의 age를 알 수 있다.
    - `jti`: JWT의 고유 식별자로, 중복적인 처리를 방지하기 위해 사용된다.
2. public claim<br>
    `public claim`은 충돌이 방지된 이름을 가지고 있어야 한다.

    때문에 claim 이름을 URI 형식으로 짓는다.
3. private claim<br>
   클라이언트와 서버 양 측 간 협의하에 사용하는 claim 이름으로, public claim과 달리 이름이 중복되어 충돌이 될 수 있다.

### 📌 Signature 서명
`Signature`은 Header의 인코딩 값과 Payload의 인코딩 값을 합쳐 주어진 비밀키로 해시를 하여 생성한다.

## JWT 로그인 흐름
![picture1](./images/What%20is%20JWT/What-is-JWT-1.png)
1. 사용자가 username과 password로 로그인을 한다.
2. 서버는 해당 계정 정보를 읽어 사용자가 있는지 확인 후 JWT을 발급한다.
    
    이때, 토큰에 사용자의 고유한 ID값을 부여한 후 기타 정보와 함께 Payload에 넣고 토큰의 유효 기간을 설정한다.
    
3. 서버는 secret_key를 이용해 Access Token을 발급하고, 해당 토큰을 브라우저에 전달한다.
4. 사용자는 Access Token을 받아 저장하고, 인증이 필요한 요청마다 Authorization 헤더에 토큰을 실어 보낸다.
5. 서버는 secret_key를 이용해 해당 토큰의 Verify Signature를 복호화한 후, 조작 여부와 유효기간을 확인한다.
6. 검증이 완료되면 Payload를 디코딩하여 사용자 데이터를 전달한다.

## JWT의 단점 및 고려사항
### ✨ Self-contained

JWT는 토큰 자체에 정보를 담고 있기 때문에 양날의 검이 될 수 있다.

### ✨ 토큰의 길이

JWT의 Paylaod에 세 가지 claim(registered, public, private)을 담기 때문에 정보가 많아질수록 토큰의 길이가 늘어나 네트워크에 부하를 줄 수 있다.

### ✨ Payload 인코딩

Payload는 암호화된 것이 아니라 `BASE64`로 인코딩된 것이기 때문에, 토큰을 탈취하여 디코딩하면 데이터를 모두 볼 수 있다.

때문에, Payload를 JWE로 암호화하거나 Payload에 중요 정보를 담지 않아야 한다.

### ✨ Stateless

JWT는 상태를 저장하지 않기 때문에 한번 만들어지면 제어가 불가능하다.

즉, 토큰을 임의로 삭제하는 것이 불가능하기 때문에 토큰 만료 시간을 꼭 포함해야 한다.

### ✨ Tore Token

토큰은 클라이언트 측에서 관리해야 하기 때문에 토큰을 저장해야 한다.

---
## references
- [https://velopert.com/2389](https://velopert.com/2389)
- [https://chb2005.tistory.com/178](https://chb2005.tistory.com/178)
- [https://mangkyu.tistory.com/56](https://mangkyu.tistory.com/56)
