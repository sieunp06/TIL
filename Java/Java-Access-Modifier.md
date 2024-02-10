# Java Access Modifier
### 목차
- [접근 제어자(Access Modifier)란?](#접근-제어자access-modifier란)
    - [✨private](#✨private)
    - [✨default](#✨default)
    - [✨protected](#✨protected)
    - [✨public](#✨public)
---

## 접근 제어자(Access Modifier)란?
`접근 제어자`를 사용하여 변수나 메서드의 사용 권한을 설정할 수 있다.

- private
- default
- protected
- public

Java에서는 위 4가지 접근 제어자를 제공하고, 아래와 같이 접근 권한을 허용한다.

`private` < `default` < `protected` < `public`

즉, `private`가 가장 적은 접근 권한을 허용하고 `public`이 가장 많은 접근 권한을 허용한다.

### ✨private
`private`가 붙은 변수나 메서드는 해당 클래스 안에서만 접근이 가능하다.

```java
public class Sample {
    private String secret;

    private String getSecret() {
        return this.secret;
    }
}
```
위 예제 코드에서 변수 `secret`과 메서드 `getSecret()`은 `Sample` 클래스 안에서만 접근이 가능하다.

### ✨default
접근 제어자를 별도로 설정하지 않았을 때, `default` 접근 제어자를 기본으로 사용한다.

`default`를 사용한 변수와 메서드는 동일한 패키지 안에서 접근이 가능하다.


```java
// 📄 house/HouseKim.java

package house;

public class HouseKim {
    String lastname = "Kim";
}
```
```java
// 📄 house/HousePark.java

package house;

public class HousePark {
    String lastname = "Park";

    public static void main(String[] args) {
        HouseKim kim = new HouseKim();
        System.out.println(kim.lastname);       // Kim
    }
}
```
위 예제 코드들은 `house`라는 동일한 패키지에 존재하기 때문에 `HousePark` 클래스에서 `kim.lastname`에 접근이 가능하다.

### ✨protected
`protected`는 동일한 패키지의 클래스 또는 해당 클래스를 상속받은 클래스에서만 접근이 가능하다.

```java
// 📄 house/HousePark.java

package house;

public class HousePark {
    protected String lastname = "Park";
}
```
```java
// 📄 house/person/SieunPark.java

package hoouse.person;

public class SieunPark extends HousePark {
    public static void main(String[] args) {
        SieunPark sep = new SieunPark();

        System.out.println(sep.lastname);       // Park
    }
}
```
위 예제에서는 두 코드의 패키지가 다르지만 `SieunPark`이 `HousePark`을 상속하고 있기 때문에 `HousePark.lastname`에 접근 가능하다.

### ✨public
`public`은 `public`이 붙은 변수나 메서드는 어떤 클래스에서도 접근 가능하다.

```java
// 📄 house/HousePark.java

package house;

public class HousePark {
    protected String lastname = "Park";
    public String info = "this is public message.";
}
```
```java
import house.HousePark;

public class Sample {
    public static void main(String[] args) {
        HousePark housePark = new HousePark();
        System.out.println(housePark.info);     // this is public message.
    }
}
```

-----
## 💎 References
- [점프 투 자바 - 접근 제어자](https://wikidocs.net/232)