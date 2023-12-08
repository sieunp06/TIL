# Item 1 생성자 대신 정적 팩터리 메서드를 고려하라
## ❓정적 팩터리 매서드
정적 팩터리 메서드(static factory method)는 `객체를 생성하는 역할을 하는 static 메서드`이다.

다음은 java의 박싱 클래스인 `Boolean`이다.
```java
public static Boolean valueOf(boolean b) {
    return b ? Boolean.TRUE : Boolean.FALSE;
}
```
위 메서드는 정적 팩터리 메서드의 예시 중 하나로 기본 타입인 boolean 값을 받아 Boolean 객체 참조로 반환한다.

> 📌 여기서 말하는 정적 팩터리 메서드(static factory method)는 팩토리 메서드 디자인 패턴과는 다르다.

## 👍장점
### 1. 이름을 가질 수 있다.
####  객체의 특징을 더 명확히 설명할 수 있다.
정적 팩터리 메서드는 생성자와 다르게 이름을 가질 수 있다.

이러한 정적 팩터리 메서드를 사용하여 반환될 객체의 특성을 더 명확히 설명할 수 있다.

```java
BigInteger(int, int, Random)
```
```java
BigInteger.probablePrime
```
위 두 가지 예시 중 `값이 소수인 BigInteger`를 반환한다는 의미를 잘 표현한 것은 정적 팩터리 메서드를 사용한 두 번째 예시일 것이다.

#### 시그니처(메서드 명 + 파라미터 타입)가 같은 생성자를 구분할 수 있다.
예시를 위해 `Drink` 클래스가 있다 가정한다. 
`Drink` 클래스는 음료가 캔에 들어있는지 병에 들어있는지를 구분한다.
```java
public class Drink {
    private String name;
    private boolean isCan;
    private boolean isBottle;
}
```
만약 이 음료가 캔에 들어있을 때 `isCan`이 `true`이고, 병에 들어 있을 때 `isBottle`이 `true`가 될 수 있도록 만들려면 어떻게 해야할까?

🎀 생성자
```java
public class Drink {
    private String name;
    private boolean isCan;
    private boolean isBottle;
    
    public Drink(String name, boolean isCan) {
        this.name = name;
        this.isCan = isCan;
    }
    
    // Error: 'Drink(String, boolean)' is already defined in 'Drink'
    public Drink(String name, boolean isBottle) {
        this.name = name;
        this.isBottle = isBottle;
    }
}
```
하나의 시그니처로는 하나의 생성자만 만들 수 있기 때문에 에러가 발생하게 된다.

🎀 정적 팩터리 메서드
```java
public class Drink {
    private String name;
    private boolean isCan;
    private boolean isBottle;

    public static Drink drinkInCan(String name, boolean isCan) {
        Drink drink = new Drink();
        drink.name = name;
        drink.isCan = true;
        return drink;
    }

    public static Drink drinkInBottle(String name, boolean isBottle) {
        Drink drink = new Drink();
        drink.name = name;
        drink.isBottle = true;
        return drink;
    }
}
```
정적 팩터리 메서드는 생성자와 달리 이름만 다르다면 시그니처가 같아도 여러 개를 만들 수 있다.

때문에 위와 같은 예시는 에러가 발생하지 않고, 생성자가 여러 개 존재하는 것처럼 사용할 수 있다.

### 2. 호출될 때마다 인스턴스를 새로 생성하지는 않아도 된다.
java의 박싱 클래스인 `Boolean`의 `valueOf`는 객체를 생성하지 않고 미리 만들어 놓은 인스턴스를 반환한다.

이 방법은 `Boolean` 객체를 `TRUE`, `FALSE` 두 가지 인스턴스만을 갖는 `불변 객체`로 사용할 수 있다.

> 📌 [불변 객체 - Item 17 변경 가능성을 최소화하라]()

```java
public final class Boolean {
    public static final Boolean TRUE = new Boolean(true);
    public static final Boolean FALSE = new Boolean(false);

    // ...

    public static Boolean valueOf(boolean b) {
        return (b ? TRUE : FALSE);
    }

    // ...
}
```
이 방법은 요청이 계속 반복되는 상황에서 성능을 꽤 끌어올릴 수 있다.

이러한 클래스를 `인스턴스 통제(instance-controlled)` 클래스라고 한다. 

반복되는 요청에 같은 객체를 반환하는 식으로 정적 팩터리 메서드 방식의 클래스가 언제 어느 인스턴스가 살아 있게 할지를 철저히 통제하기 때문이다.

#### ❓ 인스턴스를 통제하는 이유는 무엇일까?
인스턴스틑 통제하면 다음과 같은 이점이 있다.
- 클래스를 `싱글턴(singleton)`으로 만들 수 있다.
- 클래스를 `인스턴스화 불가(noninstantiable)`로 만들 수 있다.
- `불변 값 클래스`에서 동치인 인스턴스가 단 하나 뿐임을 보장할 수 있다.

> 📌 [싱글턴(singleton) - Item 3 private 생성자나 열거 타입으로 싱글턴임을 보증하라]()

> 📌 [인스턴스화 불가(noninstantiable) - Item 4 인스턴스화를 막으려거든 private 생성자를 사용하라]()

> 📌 [불변 값 클래스 - Item 17 변경 가능성을 최소화하라]()

### 3. 반환 타입의 하위 타입 객체를 반환할 수 있는 능력이 있다.
`java 8` 이후 인터페이스가 정적 팩터리 메서드를 가질 수 있게 되었다. 

```java
// Drink.java
public interface Drink {
    static Cola getCola() {
        return new Cola();
    }
    
    static Soda getSoda() {
        return new Soda();
    }
}

// Cola.java
class Cola implements Drink {}

// Soda.java
class Soda implements Drink {}

// Main.java
public class Main {
    public static void main(String[] args) {
        Cola cola = Drink.getCola();
        Soda soda = Drink.getSoda();
    }
}
```

### 4. 입력 매개변수에 따라 매번 다른 클래스의 객체를 반환할 수 있다.
#### 클라이언트는 어떤 객체가 반환되는지 모른다.
반환 타입의 하위 타입이기만 하면 어떤 클래스의 객체를 반환하든 상관 없으며, 다음 릴리즈에서는 또 다른 클래스의 객체를 반환해도 된다.

`EnumSet`은 public 생성자 없이 정적 팩터리 메서드만 제공한다.
```java
// EnumSet.java
public static <E extends Enum<E>> EnumSet<E> noneOf(Class<E> elementType) {
    Enum<?>[] universe = getUniverse(elementType);
    if (universe == null)
        throw new ClassCastException(elementType + " not an enum");

    if (universe.length <= 64)
        return new RegularEnumSet<>(elementType, universe);
    else
        return new JumboEnumSet<>(elementType, universe);
}
```

`EnumSet`의 `noneOf` 메서드를 보면 원소의 수에 따라 두 가지 하위 클래스 중 하나의 인스턴스를 반환한다.

이때 클라이언트는 `EnumSet`의 `noneOf` 메서드는 알고 있지만, `noneOf`로 반환되는 객체가 `RegularEnumSet`인지 `JumboEnumSet`인지 모르며 알 필요가 없다.

때문에, `JumboEnumSet`을 사용할 필요가 없다면 언제든 삭제해도 상관이 없으며 성능을 개선하기 위해 다른 클래스를 다음 릴리즈에 추가할 수 있다.

> 📌 [EnumSet 클래스 - Item 36 비트 필드 대신 EnumSet을 사용하라]()

#### 매개변수에 따라 다른 클래스를 반환하도록 선택할 수 있다.
```java
// Drink.java
public interface Drink {
    void printName();

    static Drink of(String name) {
        if (name.equals("Cola")) {
            return new Cola();
        }
        return new Soda();
    }
}

// Cola.java
public class Cola implements Drink {
    @Override
    public void printName() {
        System.out.println("Cola");
    }
}

// Soda.java
public class Soda implements Drink {
    @Override
    public void printName() {
        System.out.println("Soda");
    }
}
```
`Drink` 인터페이스 안에 존재하는 정적 팩터리 메서드 `of`에서는 매개변수인 음료의 이름에 따라 하위 구현 클래스인 `Cola`와 `Soda`를 반환한다.

```java
// Main.java
public class Main {
    public static void main(String[] args) {
        Drink drink = Drink.of("Cola");
        drink.printName();  // Cola
    }
}
```

## 👎단점
### 1. 상속을 하려면 public이나 protected 생성자가 필요하니 정적 팩터리 메서드만 제공하면 하위 클래스를 만들 수 없다.
정적 팩터리 메서드를 사용하여 객체를 생성할 수 있게 만들었다고 가정한다면, 보통 클라이언트로 하여금 생성자가 아닌 정적 팩터리 메서드로 객체를 생성할 수 있도록 생성자는 `private`로 막아둔다.
```java
class Drink {
    // ...
    private Drink(String name) {...}

    public static Drink of(String name) {
        return new Drink(String name);
    }
}
```
이러한 방법을 사용하면 생성자를 `private`로 막아두면 상속할 수 없게 된다는 단점이 있다.

하지만, 상속보다는 `컴포지션`을 사용하도록 유도하고 `불변 타입`으로 만들려면 위 제약을 지켜야 하기 때문에 장점으로 받아들일 수도 있다.

> 📌 [컴포지션 - Item 18 상속보다는 컴포지션을 사용하라]()

> 📌 [불변 값 클래스 - Item 17 변경 가능성을 최소화하라]()

### 2. 정적 팩터리 메서드는 프로그래머가 찾기 어렵다.
정적 팩터리 메서드는 생성자와 달리 API 설명에 명확히 드러나지 않기 때문에 인스턴스를 생성해주는 메서드를 직접 찾아야 한다.

이런 단점을 해결하기 위해 API 문서를 꼼꼼히 작성하고, 메서드 이름도 널리 알려진 규약을 따라 짓는 방식으로 이런 문제를 해결해야 한다.

## ✍️정적 팩터리 메서드의 명명 방식
- `from`: 매개변수를 하나 받아서 해당 타입의 인스턴스를 반환하는 형변환 메서드
    ```java
    Date d = Date.from(instant);
    ```
- `of`: 여러 매개변수를 받아 적합한 타입의 인스턴스를 반환하는 집계 메서드
    ```java
    Set<Rank> faceCards = EnumSet.of(JACK, QUEEN, KING);
    ```
- `valueOf`: `from`과 `of`의 더 자세한 버전
    ```java
    BigInteger prime = BigInteger.valueOf(Integer.MAX_VALUE);
    ```
- `instance` or `getInstance`: (매개변수를 받는다면) 매개변수로 명시한 인스턴스를 반환하지만, 같은 인스턴스임을 보장하지 않는다.
    ```java
    StackWalker luke = StackWalker.getInstance(options);
    ```
- `create` or `newInstance`: `instance` 혹은 `getInstance`와 같지만, 매번 새로운 인스턴스를 생성해 반환함을 보장한다.
    ```java
    Object newArray = Array.newInstance(classObject, arrayLen);
    ```
- `getType`: `getInstance`와 같으나, 생성할 클래스가 아닌 다른 클래스에 팩터리 메서드를 정의할 때 쓴다. `Type`은 팩터리 메서드가 반환할 객체의 타입이다.
    ```java
    FileStore fs = Files.getFileStore(path);
    ```
- `newType`: `newInstance`와 같으나, 생성할 클래스가 아닌 다른 클래스에 팩터리 메서드를 정의할 때 쓴다. `Type`은 팩터리 메서드가 반환할 객체의 타입이다.
    ```java
    BufferedReader br = Files.newBufferedReader(path);
    ```
- `type`: `getType`과 `newType`의 간결한 버전
    ```java
    List<Complaint> litany = Collections.list(legacyLitany);
    ```
    