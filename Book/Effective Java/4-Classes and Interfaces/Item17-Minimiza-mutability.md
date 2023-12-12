# Item 17 변경 가능성을 최소화하라
## ❓불변 클래스
불변 클래스란 그 인스턴스 내부 값을 수정할 수 없는 클래스이며, 불변 인스턴스에 설정된 정보는 고정되어 객체가 파괴되는 순간까지 절대 달라지지 않는다.

이러한 불변 클래스는 가변 클래스보다 설계하고 구현하기 쉬우며, 오류가 생길 여지가 적고 안전하다.

자바 라이브러리 안에는 다음과 같이 다양한 불변 클래스가 있다.
- String
- 기본 타입의 박싱된 클래스들
- BigInteger, BigDecimal

## 불변 클래스를 만들기 위한 5가지 규칙
### 1. 객체의 상태를 변경하는 메서드(변경자)를 제공하지 않는다.
즉, 세터(setter) 메서드를 제공하지 않는다. 

세터 메서드를 제공하면 세터 메서드를 통해 상태가 변경될 수 있기 떄문에 불변 클래스로 만들 수 없다.

아래 `Drink` 클래스는 세터 메서드를 제공하는 예시이다.
```java
public class Drink {
    private String name;

    public Drink(String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }

    // setter 메서드
    public void setName(String name) { 
        this.name = name;
    }
}
```
```java
Drink cola = new Drink("cola");
System.out.println(cola.getName());     // cola;
cola.setName("soda");  
System.out.println(cola.getName());     // soda;
```
`setName` 메서드를 사용하면 `name` 필드의 상태가 변경되기 때문에 위 클래스를 불변으로 만들 수 없다.

### 2. 클래스를 확장할 수 없도록 한다.
클래스를 확장할 수 없도록 하면 하위 클래스에서 부주의하게 or 나쁜 의도로 객체의 상태를 변하게 만드는 사태를 막는다.

 ```java
 
 ```

#### 상속을 막는 대표적인 방법
- 클래스를 `final`로 선언한다.
- 

### 3. 모든 필드를 final로 선언한다.
모든 필드를 `final`로 선언하는 방법은 시스템이 강제하는 수단을 이용해 설계자의 의도를 명확히 드러낸다.

### 4. 모든 필드를 private으로 선언한다.
필드가 참조하는 가변 객체를 클라이언트에서 직접 접근해 수정하는 일을 막아준다.

기본 타입 필드나 불변 객체를 참조하는 필드를 `public final`로만 선언해도 불변 객체를 만들 수 있지만, 이런 방법을 사용하면 다음 릴리즈에서 내부 표현을 바꾸지 못하기 때문에 추천되지 않는 방법이다.

> 📌 [예외 메시지 - Item 15 예외의 상세 메시지에 실패 관련 정보를 담으라]()

> 📌 [예외 메시지 - Item 16 예외의 상세 메시지에 실패 관련 정보를 담으라]()

### 5. 자신 외에는 내부의 가변 컴포넌트에 접근할 수 없도록 한다.
클래스에서 가변 객체를 참조하는 필드가 하나라도 있다면 클라이언트에서 그 객체의 참조를 얻을 수 없도록 해야 한다.

이런 필드는 절대 클라이언트가 제공한 객체 참조를 가리키게 해서는 안된다.

또한, 접근자 메서드가 그 필드를 그대로 반환해서도 안된다.

생성자, 접근자, readObject 메서드에서 방어적 복사를 수행해야 한다.

> 📌 [예외 메시지 - Item 88 예외의 상세 메시지에 실패 관련 정보를 담으라]()

## 불변 복소수 클래스
아래 예시 코드는 복소수(실수부와 허수부로 구성된 수)를 표현한다.
```java
public final class Complex {
    private final double re;
    private final double im;

    public Complex(double re, double im) {
        this.re = re;
        this.im = im;
    }

    public double realPart() {
        return re;
    }

    public double imaginaryPart() {
        return im;
    }

    public Complex plus(Complex c) {
        return new Complex(re + c.re, im + c.im);
    }

    public Complex minus(Complex c) {
        return new Complex(re - c.re, im - c.im);
    }

    public Complex times(Complex c) {
        return new Complex(re * c.re - im * c.im, re * c.im + im * c.re);
    }

    public Complex dividedBy(Complex c) {
        double tmp = c.re * c.re + c.im * c.im;
        return new Complex((re * c.re + im * c.im) / tmp, (im * c.re - re * c.im) / tmp);
    }

    @Override
    public boolean equals(Object o) {
        if (o == this) {
            return true;
        }
        if (!(o instanceof Complex)) {
            return false;
        }
        Complex c = (Complex) o;

        return Double.compare(c.re, re) == 0 && Double.compare(c.im, im) == 0;
    }

    @Override
    public int hashCode() {
        return 31 * Double.hashCode(re) + Double.hashCode(im);
    }

    @Override
    public String toString() {
        return "(" + re + " + " + im + "i)";
    }
}
```

