# Entity 매핑 어노테이션
#### 목차
1. [객체와 테이블을 매핑하는 어노테이션](#객체와-테이블을-매핑하는-어노테이션)
    - [@Entity](#entity)
        - [📌@Entity의 속성들](#📌entity의-속성들)
        - [📌@Entity 적용시 주의할 점들](#📌entity-적용시-주의할-점들)
    - [@Table](#table)
        - [📌@Table의 속성들](#📌table의-속성들)
2. [기본키를 매핑하는 어노테이션](#기본키를-매핑하는-어노테이션)
    - [JPA가 제공하는 데이터베이스 기본 키 생성 전략](#jpa가-제공하는-데이터베이스-기본-키-생성-전략)
        - [직접 할당 전략](#👉-직접-할당-전략)
        - [IDENTITY 전략](#👉-identity-전략)
        - [SEQUENCE 전략](#👉-sequence-전략)
        - [TABLE 전략](#👉-table-전략)
        - [AUTO 전략](#👉-auto-전략)
3. [필드와 컬럼을 매핑하는 어노테이션](#필드와-컬럼을-매핑하는-어노테이션)
    - [@Column](#column)
        - [📌@Column의 속성들](#📌column의-속성들)
        - [📌@Column의 생략](#📌column의-생략)
    - [@Enumerated](#enumerated)
        - [📌@Enumerated 속성](#📌enumerated-속성)
        - [📌@Enumerated 사용 예](#📌enumerated-사용-예)
    - [@Temporal](#temporal)
        - [📌@Temporal의 속성](#📌temporal의-속성)
        - [📌@Temporal의 사용 예](#📌temporal의-사용-예)
    - [@Lob](#lob)
        - [📌@Lob의 속성](#📌lob의-속성)
        - [📌@Lob의 사용 예](#📌lob의-사용-예)
    - [@Transient](#transient)
    - [@Access](#access)
        - [📌필드 접근](#📌필드-접근)
        - [📌프로퍼티 접근](#📌프로퍼티-접근)

----

## 객체와 테이블을 매핑하는 어노테이션
### @Entity
`@Entity`가 붙은 어노테이션은 JPA가 관리한다고 알리는 것으로, JPA를 사용해서 객체와 테이블을 매핑하려는 클래스는 `@Entity`를 꼭 붙여야 한다.

`@Entity`를 붙인 클래스는 `엔티티`라고 부른다.

#### 📌@Entity의 속성들

|속성|기능|기본값|
|--|--|--|
|name|JPA에서 사용할 엔티티 이름을 지정한다.|클래스의 이름|

#### 📌@Entity 적용시 주의할 점들
- 기본 생성자는 필수적으로 존재해야 한다.
    - 만약 존재하지 않는다면 기본 생성자를 자동으로 만든다.
        ```java
        public Member() {} // 기본 생성자
        ```
    - 생성자를 하나 이상 만들면 기본 생성자를 자동으로 만들지 않기 떄문에 직접 만들어야 한다.
        ```java
        public Member() {}  // 직접 만든 기본 생성자

        public Member(String name) {
            this.name = name;
        }
        ```
- final 클래스, enum, interface, inner 클래스에는 사용할 수 없다.
- 저장할 필드에 final을 사용하면 안된다.

### @Table
`@Table`은 엔티티와 매핑할 테이블을 지정한다.

만약 생략하면 엔티티의 이름을 테이블의 이름으로 사용한다.

#### 📌@Table의 속성들

|속성|기능|기본값|
|--|--|--|
|name|매핑할 테이블의 이름을 지정한다.|엔티티의 이름|
|catelog|catalog 기능이 있는 데이터베이스에서 catalog를 매핑한다.||
|schema|schema 기능이 있는 데이터베이스에서 schema를 매핑한다.||

## 기본키를 매핑하는 어노테이션
### JPA가 제공하는 데이터베이스 기본 키 생성 전략
- 직접 할당: 기본 키를 애플리케이션에서 직접 할당한다.
- 자동 생성: 대리 키 사용 방식
    - IDENTITY: 기본 키 생성 방식을 데이터베이스에 위임한다.
    - SEQUENCE: 데이터베이스 시퀀스를 사용해서 기본 키를 할당한다.
    - TABLE: 키 생성 테이블을 사용한다.

<br>

> <strong>❗자동생성 방식이 다양한 이유 </strong><br>
> - 데이터베이스 벤더마다 지원하는 방식이 다르기 때문이다.<br>
>   📌 [DBMS 종류별 특징]()

#### 👉 직접 할당 전략
기본 키 직접 할당 전략은 아래와 같이 `persist` 하기 전에 애플리케이션의 기본 키를 직접 할당하는 방법이다.
```java
Board board = new Board();
board.setId("id1")  // 기본 키 직접 할당
em.persist(board);
```

JPA에서 기본 키를 직접 할당하기 위해서는 `@Id`를 사용하여 매핑한다.

```java
@Id
@Column(name = "Id")
private String id;
```

`@Id`를 적용할 수 있는 자바 타입은 아래와 같다.
- 자바 기본형
- 자바 래퍼형
- String
- java.util.Date
- java.sql.Date
- java.math.BigDecimal
- java.math.BigInteger

#### 👉 IDENTITY 전략
`IDENTITY` 전략은 기본 키 생성을 데이터베이스에 위임하는 전략인데, 주로 PostgreSQL, SQL Server, DB2에서 사용한다.

`MySQL`의 `AUTO_INCREMENT`와 같이 데이터베이스에서 알아서 ID 컬럼에 순서대로 값을 넣어주는 것과 같다.

<br>

`IDENTITY` 전략을 사용하기 위해서는 `@Id` 어노테이션과 함께 `@GeneratedValue` 어노테이션에 `strategy` 속성 값을 `GenerationType.IDENTITY`로 지정하여 사용하여야 한다.

```java
@Entity
public class Board {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
}
```

기본 키 생성을 데이터베이스에 위임했기 때문에 `persist`로 엔티티를 저장하면서 데이터베이스가 기본 키를 순서대로 값을 생성하여 `persist` 후 `board`의 아이디 값을 출력했을 때 `1`이 조회되어 출력된다.

```java
private static void logic(EntityManager em) {
    Board board = new Board();
    em.persist(board);
    System.out.println("board.id = " + board.getId());  // "board.id = 1"
}
```

#### 👉 SEQUENCE 전략
`SEQUENCE` 전략은 유일한 값을 순서대로 생성하는 전략으로, 시퀀스를 지원하는 오라클, PostgreSQL, DB2, H2 데이터베이스에서 사용된다.

```java
@Entity
@SequenceGenerator(
    name = "BOARD_SEQ_GENERATOR,
    sequenceName = "BOARD_SEQ", // 매핑할 데이터베이스 시퀀스 이름
    initialValue = 1, allocationSize = 1)
public class Board {
    @Id
    @GeneratedValue(strategy = GenerationType.SEQUENCE,
                    generator = "BOARD_SEQ_GENERATOR")
    private Long id;
}
```
`@SequenceGenerator`를 사용하여 시퀀스 생성기를 등록한다.

|속성|기능|기본값|
|--|--|--|
|name|시퀀스 생성기 이름을 지정한다.|필수|
|sequenceName|데이터베이스에 지정되어 있는 시퀀스 이름을 지정한다.|hibernate_sequence|
|initialValue|DDL 생성시에만 사용되며, 시퀀스 DDL을 생성할 때 처음 시작하는 수를 지정한다.|1|
|allocationSize|시퀀스 한 번 호출에 증가하는 수(성능 최적화에 사용됨)를 지정한다.|50|
|catalog.schema|데이터베이스 catalog.schema의 이름을 지정한다.||

후에 `@GeneratedValue` 어노테이션의 `strategy` 속성을 `GenerationType.SEQUENCE`로 지정하고, `Generator` 속성에 생성한 시퀀스 생성기의 이름을 등록한다.

```java
private static void logic(EntityManager em) {
    Board board = new Board();
    em.persist(board);
    System.out.println("board.id = " + board.getId()))  // board.id = 1
}
```

`IDENTITY` 전략과 같아보이지만 내부 동작 방식은 다르다.

`persist`를 호출할 때 데이터베이스 시퀀스를 사용하여 식별자를 조회한 후, 조회한 식별자를 엔티티에 할당한 후에 엔티티를 영속성 컨텍스트에 저장한다.

이후 트랜잭션을 커밋하여 플러시가 일어나면 엔티티를 데이터베이스에 저장한다.

#### 👉 TABLE 전략
`TABLE` 전략은 키 생성 전용 테이블을 만들고 이 테이블에 이름과 값으로 사용할 컬럼을 만들어 데이터베이스 시퀀스를 흉내내는 전략이다. 

테이블을 사용하여 시퀀스를 흉내내는 전략이기 때문에 모든 데이터베이스에서 사용할 수 있다.

```SQL
CREATE TABLE MY_SEQUENCES (
    SEQUENCE_NAME VARCHAR(255) NOT NULL,
    NEXT_VAL BIGINT,
    PRIMARY KEY (SEQUENCE_NAME)
)
```
`TABLE` 전략을 사용하려면 위처럼 키를 생성할 테이블을 생성해야 한다.

```java
@Entity
@TableGenerator(
    name = "BOARD_SEQ_GENERATOR",
    table = "MY_SEQUENCES",
    pkColumnValue = "BOARD_SEQ", allocationSize = 1)
public class Board {
    @Id
    @GeneratedValue(strategy = GenerationType.TABLE)
    private Long id;
}
```

`@TableGenerator`를 사용하여 테이블 키 생성기를 등록한다.

|속성|기능|기본값|
|--|--|--|
|name|식별자 생성기의 이름을 지정한다.|필수|
|table|키 생성 테이블 이름을 지정한다.|hibernate_sequences|
|pkColumnName|시퀀스의 컬럼 이름을 지정한다.|sequence_name|
|valueColumName|시퀀스 값의 컬럼 이름을 지정한다.|next_val|
|pkColumnValue|키로 사용할 값의 이름을 지정한다.|엔티티 이름|
|initialValue|초기 값, 마지막으로 생성된 값이 기준이다.|0|
|allocationValue|시퀀스 한 번 호출에 증가하는 수(성능 최적화에 사용됨)를 지정한다.|50|
|catalog.schema|데이터베이스 catalog.schema 이름||
|uniqueConstrains(DDL)|유니크 제약 조건을 지정한다.||

위 코드에서 `name` 속성을 통해 `BOARD_SEQ_GENERATOR`라는 테이블 키 생성기를 등록하고, `table` 속성으로 위에서 생성한 `MY_SEQUENCES` 테이블 키 생성용 테이블을 매핑한다.

그 후 `@GeneratedValue` 어노테이션의 `strategy` 속성을 `GenerationType.TABLE`로 설정한다.

```java
private static void logic(EntityManager em) {
    Board board = new Board();
    em.persist(board);
    System.out.println("board.id = " + board.getId());  // board.id = 1
}
```

|sequence_name|next_val|
|--|--|
|BOARD_SEQ|2|
|MEMBER_SEQ|10|
|PRODUCT_SEQ|50|

위 코드에서 `@TableGenerator` 어노테이션의 `pkColumnValue` 속성 값으로 지정한 `BOARD_SEQ`이 추가된 것을 알 수 있다. 

키 생성기를 사용할 때마다 `next_val` 값이 증가하게 된다.

#### 👉 AUTO 전략
`@GeneratedValue.strategy`의 기본값은 `AUTO`이다.

기본 값이기 떄문에 생략해도 무관하므로 아래 두 코드는 같은 의미를 가진다.

```java
@Entity
public class Board {
    @Id
    @GeneratedValue(strategy = GenerationValue.AUTO)
    private Long id;
}
```
```java
@Entity
public class Board {
    @Id @GeneratedValue
    private Long id;
```

`AUTO` 전략은 데이터베이스에 따라 알아서 전략을 선택한다.

즉, 오라클을 선택하면 `SEQUENCE`를, MySQL을 선택하면 `IDENTITY`를 선택한다.

## 필드와 컬럼을 매핑하는 어노테이션
### @Column
`@Column`은 객체 필드를 테이블 컬럼에 매핑한다.

#### 📌@Column의 속성들

|속성|기능|기본값|
|--|--|--|
|name|필드와 매핑할 테이블의 컬럼 이름|객체의 필드 이름|
|insertable<br>(거의 사용X)|엔티티 저장 시 필드도 같이 저장한다.<br>false: 해당 필드를 데이터베이스에 저장하지 않는다.(읽기 전용일 때 사용)|true|
|updatable<br>(거의 사용X)|엔티티 수정 시 해당 필드도 같이 수정한다.<br>false: 데이터베이스에 수정하지 않는다.(읽기 전용일 때 사용)|true|
|table<br>(거의 사용X)|하나의 엔티티를 두 개 이상의 테이블에 매핑할 때 사용한다.<br>지정한 필드를 매핑할 수 있다.|현재 클래스가 매핑된 테이블|
|nullable(DDL)|null 값의 허용 여부를 설정한다.<br>false: DDL 생성 시에 NOT NULL 제약 조건이 붙는다.|true|
|unique(DDL)|@Table의 uniqueConstraints와 같지만 한 컬럼에 간단히 유니크 제약 조건을 걸 때 사용한다.||
|columnDefinition|데이터베이스 컬럼 정보를 직접 설정할 수 있다.|필드의 자바 타입과 데이터베이스 정보를 사용하여 적절한 컬럼타입을 생성한다.
|length(DDL)|문자 길이 제약 조건을 지정한다.<br>String 타입에서만 사용한다.|255|
|precision, scale(DDL)|BigDecimal/BigInteger 타입에서 사용하며 precision은 소수점을 포함한 전체 자릿수를, scale은 소수의 자릿수를 자징한다.|precision = 19, <br>scale = 2|

<br>

- nullable(DDL)
    ```java
    @Column(nullable = false)
    private String data;
    ```
    ```SQL
    data VARCHAR(255) not null
    ```
- unique(DDL)
    ```java
    @Column(unique = true)
    private String username;
    ```
    ```SQL
    ALTER TABLE Tablename
        ADD CONSTRAINT UK_Xxx UNIQUE (username)
    ```
- columnDefinition(DDL)
    ```java
    @Column(columnDefinition = "varchar(100) default 'EMPTY'")
    private String data;
    ```
    ```SQL
    data VARCHAR(100)  default 'EMPTY'
    ```
- length(DDL)
    ```java
    @Column(length = 400)
    private String data;
    ```
    ```SQL
    data VARCHAR(400)
    ```
- precision, scale(DDL)
    ```java
    @Column(precision = 10, scale = 2)
    private String data;
    ```
    ```SQL
    cal numeric(10, 2)  // H2, PostgreSQL
    cal number(10, 2)   // 오라클
    cal decimal(10, 2)  // MySQL
    ```

### @Enumerated
`@Enumerated`는 자바 `enum` 타입을 매핑할 때 사용한다.

#### 📌@Enumerated의 속성

|속성|기능|기본값|
|--|--|--|
|value|👉EnumType.ORDINAL: enum 순서를 데이터베이스에 저장<br>👉EnumType.STRING: enum 이름을 데이터베이스에 저장|EnumType.ORDINAL|

- EnumType.ORDINAL
    - enum에 정의된 순서대로 저장된다. <br>
    Ex) ADMIN - 0, USER - 1
    - 장점: 데이터베이스에 저장되는 데이터의 크기가 작다.
    - 단점: 이미 저장된 enum의 순서를 변경할 수 없다.
- EnumType.STRING
    - enum의 이름 그대로 저장한다. <br>
    Ex) ADMIN - 'ADMIN', USER- 'USER'
    - 장점: 저장된 enum의 순서가 바뀌거나 enum이 추가되어도 안전하다.
    - 단점: 데이터베이스에 저장되는 데이터의 크기가 ORDINAL에 비해 크다.

<br>

> <strong>❗ ORDINAL은 주의해서 사용해야 한다.<br></strong>
> ADMIN(0번), USER(1번) 사이에 enum이 추가되어 ADMIN(0번), NEW(1번), USER(2번)으로 설정되었다면, USER는 2로 저장되지만 기존 데이터베이스에서는 여전히 1로 남아 있다.<br>
> 해당 문제가 발생하지 않는 `EnumType.STRING`을 사용하는 것을 권장한다.

#### 📌@Enumerated 사용 예
```java
enum RoleType {
    ADMIN, USER
}
```
아래처럼 `@Enumerated`를 사용하여 위 Enum 클래스를 매핑한다.
```java
@Enumerated(EnumType.STRING)
private RoleType roleType;
```

이렇게 매핑한 Enum 클래스를 다음과 같이 사용할 수 있다.

```java
member.setRoleType(RoleType.ADMIN); // DB에 문자 ADMIN으로 저장된다.
```

### @Temporal
`@Temporal`은 날짜 타입(java.util.Date, java.util.Calendar)을 매핑할 때 사용한다.

#### 📌@Temporal의 속성

|속성|기능|기본값|
|--|--|--|
|value|👉TemporalType.DATE: 날짜, 데이터베이스 date 타입과 매핑(EX: 2024-01-22)<br>👉TemporalType.TIME: 시간, 데이터베이스 time 타입과 매핑(EX: 11:11:11)<br>👉TemporalType.TIMESTAMP: 날짜와 시간, 데이터베이스 timestamp 타입과 매핑(EX: 2024-01-22 11:11:11)|필수|

#### 📌@Temporal의 사용 예
자바의 `Date`는 년원일 시분초 모두 제공하지만, 데이터베이스에서는 date(날짜), time(시간), timestamp(날짜와 시간) 세 가지 타입이 존재한다.

```java
@Temporal(TemporalType.DATE)
private Date date;  // 날짜
```
```java
@Temporal(TemporalType.TIME)
private Date time;  // 시간
```
```java
@Temporal(TemporalType.TIMESTAMP)
private Date timestamp; // 날짜와 시간
```

만약 `@Temporal`을 생략하면 자바의 `Date`와 가장 유사한 `timestamp`로 정의된다.

### @Lob
데이터베이스의 BLOB, CLOB 타입과 매핑한다.

#### 📌@Lob의 속성
`@Lob`에는 속성이 없지만, 매핑하는 필드의 타입에 따라 BLOB과 CLOB 타입으로 매핑된다.
- CLOB: String, char[], java.sql.CLOB
- BLOB: byte[], java.sql.BLOB

#### 📌@Lob의 사용 예
```java
@Lob
private String lobString;
```
```java
@Lob
private byte[] lobByte;
```
- 오라클
    ```SQL
    lobString clob,
    lobByte blob,
    ```
- MySQL
    ```SQL
    lobString longtext,
    lobByte longByte,
    ```
- PostgreSQL
    ```SQL
    lobString text,
    lobByte old,
    ```

### @Transient
`@Transient`는 필드를 매핑하지 않기 때문에 데이터베이스에 저장하지 않고 조회하지도 않는다.

객체에 임시로 어떤 값을 보관하고 싶을 때 사용한다.

```java
@Transient
private Integer temp;
```

### @Access
`@Access`는 JPA가 엔티티에 접근하는 방식을 지정한다.
- 필드 접근: `AccessType.FIELD`
    - 필드에 직접 접근하며, 필드 접근 권한이 private 라고 접근 가능하다.
- 프로퍼티 접근: `AccessType.PROPERTY`
    - 접근자를 사용한다.

#### 📌필드 접근
```java
@Entity
@Access(AccessType.FIELD)
public class Member {
    @Id
    private String id;

    private String data1;
    private String data2;
}
```
`@Id`에 필드가 있기 떄문에 `@Access(AccessType.FIELD)`로 설정한 것과 같다.

따라서 `@Access`는 생략해도 무관하다.

#### 📌프로퍼티 접근
```java
@Entity
@Access(AccessType.PROPERTY)
public class Member {
    private String id;

    private String data1;
    private String data2

    @Id
    public String getId() {
        return id;
    }

    @Column
    public String getData1() {
        return data1;
    }

    public String getData2() {
        return data2;
    }
}
```

-----
## 💎 References
- [자바 ORM 표준 JPA 프로그래밍](https://product.kyobobook.co.kr/detail/S000000935744)