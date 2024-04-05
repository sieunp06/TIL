## equals()와 hashCode()
`equals()`와 `hashCode()` 모두 두 객체가 같은지를 겸사하기 위한 메서드이다.

#### 📌 동일성 비교
동일성 비교는 `==`와 같다.

즉, 객체의 인스턴스 주소 값을 비교한다.

#### 📌 동등성 비교
동등성 비교는 `equals()`를 사용하여 객체 내부의 값을 비교한다.

## equals()
`equals()`는 두 객체의 내용이 같은지 확인한다.

```java
public boolean equals(Object obj) {
    return (this == obj);
}
```

비교할 대상이 객체 자체일 경우, 객체의 주소를 사용하여 비교한다.

즉, `==`을 사용하여 비교하는 것과 같기 때문에 동일 비교를 하게 된다.

```java
class Person {
    String name;

    public Person(String name) {
        this.name = name;
    }
}

public class Example {
    public static void main(String[] args) {
        Person person1 = new Person("이름1");
        Person person2 = new Person("이름2");

        System.out.println(person1 == person2);         // false
        System.out.println(person1.equals(person2));    // false
    }
}
```

### equals 오버라이딩
만약 객체 자료형을 비교할 때, 객체의 주소 값이 아닌 객체의 필드 값을 기준으로 동등 비교를 하고 싶다면 `equals` 메서드를 오버라이딩하여 이를 재정의 하면 된다. 

```java
class Person {
    String name;

    public Person(String name) {
        this.name = name;
    }

    public boolean equals(Object o) {
        if (this == o) return true;
        if (!(o instanceof Person) return false);
        Person person = (Person) o;
        return Object.equals(this.name, person.name);
    }
}

public class Example {
    public static void main(String[] args) {
        Person person1 = new Person("이름");
        Person person2 = new Person("이름");

        System.out.println(person1.equals(person2));    // true
    }
}
```

아래는 위 예시에서 재정의한 `equals` 메서드이다.

```java
public boolean equals(Object o) {
    if (this == o) return true; // 현 객체와 o가 같을 같을 경우 true
    if (!(o instanceof Person) return false);   // o가 Person 타입이 아닌 경우 false
    Person person = (Person) o;
    return Object.equals(this.name, person.name);   // 현 객체와 name 필드 값이 같을 경우 true, 다를 경우 false
}
```

위 예시에서 두개의 `Person` 객체 `person1`과 `person2`는 name 필드의 값이 `이름`으로 같기 때문에 재정의한 `equals` 메서드를 통해 true를 반환한다.

## hashCode()
`hashCode()`는 두 객체가 같은 객체인지 확인한다.

즉, `hashCode()`는 객체의 주소 값을 사용한 해싱 기법을 통해 해시 코드를 만든 후 반환한다.

```java
class Parson {
    String name;

    public Person(String name) {
        this.name = name;
    }
}

public class Main {
    public static void main(String[] args) {
        Person p1 = new Person("홍길동");
        Person p2 = new Person("홍길동");

        // 객체 인스턴스마다 각기 다른 주해시코드(주소))를 가지고 있다.
        System.out.println(p1.hashCode()); // 622488023
        System.out.println(p2.hashCode()); // 1933863327
    }
}
```

이때 서로 다른 두 객체는 같은 해시 코드를 갖지 못한다.

## equals()와 hashCode()를 같이 재정의해야 하는 이유
`equals()`와 `hashCode()`를 같이 재정의하지 않으면 hash 값을 사용하는 `Collection Framework`를 사용할 때 문제가 발생한다.

- HashSet
- HashMap
- HashTable

다음은 `equals()`만 오버라이딩 했을 경우이다.
```java
class Person {
    private String name;

    public Person(String name) {
        this.name = name;
    }

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if ((o == null || getClass() != o.getClass()) return false;
        Person person = (Person) o;
        return Objects.equals(name, person.name);
    }
}
```
```java
public class ClassTest {
    public static void main(String[] args) throws Exception {
        Person p1 = new Person("홍길동");
        Person p2 = new Person("홍길동");

        // 두 객체의 해시 코드
        System.out.println(p1.hashCode()); // 460141958
        System.out.println(p2.hashCode()); // 1163157884

        // 해시코드가 달라도, equals를 재정의 했기 때문에 동등함
        System.out.println(p1.equals(p2)); // true

        // 리스트를 생성하고 두 객체 데이터를 추가한다.
        List<Person> people = new ArrayList<>();
        people.add(p1);
        people.add(p2);
        
        // 그리고 리스트의 길이를 출력한다.
        System.out.println(people.size()); // 2
    }
}
```
`p1`와 `p2`의 해시 코드가 다르지만, 같은 내용의 `name` 필드를 가지고 있기 때문에 같은 객체로 판단된다.

```java
public static void main(String[] args) {
    Set<Car> cars = new HashSet<>();
    cars.add(new Car("foo"));
    cars.add(new Car("foo"));

    System.out.println(cars.size());    // 2
}
```

하지만 위와 같이 `HashSet`을 사용하는 경우는 다르다.

두 객체의 해시 코드가 다르기 때문에 다른 객체로 판단되어 중복 값을 허용하지 않는 `HashSet`의 길이가 2가 되었다.

`p1`와 `p2`가 논리적으로 같다고 판단하였지만, 해시 코드가 다르기 때문에 중복된 데이터가 컬렉션에 추가된 것이다.

### equals()와 hashCode()의 동작 순서
hash 값을 사용하는 `Collection Framework`는 객체가 논리적으로 같은지를 판별할 때 다음과 같은 과정을 거친다.

<br>

> `hashCode` 비교 → `equals` 비교

---
## references
- [https://steady-coding.tistory.com/604](https://steady-coding.tistory.com/604)
- [https://jisooo.tistory.com/entry/java-hashcode%EC%99%80-equals-%EB%A9%94%EC%84%9C%EB%93%9C%EB%8A%94-%EC%96%B8%EC%A0%9C-%EC%82%AC%EC%9A%A9%ED%95%98%EA%B3%A0-%EC%99%9C-%EC%82%AC%EC%9A%A9%ED%95%A0%EA%B9%8C](https://jisooo.tistory.com/entry/java-hashcode%EC%99%80-equals-%EB%A9%94%EC%84%9C%EB%93%9C%EB%8A%94-%EC%96%B8%EC%A0%9C-%EC%82%AC%EC%9A%A9%ED%95%98%EA%B3%A0-%EC%99%9C-%EC%82%AC%EC%9A%A9%ED%95%A0%EA%B9%8C)
- [https://inpa.tistory.com/entry/JAVA-%E2%98%95-equals-hashCode-%EB%A9%94%EC%84%9C%EB%93%9C-%EA%B0%9C%EB%85%90-%ED%99%9C%EC%9A%A9-%ED%8C%8C%ED%97%A4%EC%B9%98%EA%B8%B0](https://inpa.tistory.com/entry/JAVA-%E2%98%95-equals-hashCode-%EB%A9%94%EC%84%9C%EB%93%9C-%EA%B0%9C%EB%85%90-%ED%99%9C%EC%9A%A9-%ED%8C%8C%ED%97%A4%EC%B9%98%EA%B8%B0)