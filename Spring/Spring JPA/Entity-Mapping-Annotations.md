# Entity 매핑 어노테이션
#### 목차
1. [객체와 테이블을 매핑하는 어노테이션](#객체와-테이블을-매핑하는-어노테이션)
    - [@Entity](#entity)
    - [@Table](#table)
2. [기본키를 매핑하는 어노테이션](#기본키를-매핑하는-어노테이션)
    - [JPA가 제공하는 데이터베이스 기본 키 생성 전략](#jpa가-제공하는-데이터베이스-기본-키-생성-전략)
        - [직접 할당 전략](#👉-직접-할당-전략)
        - [IDENTITY 전략](#👉-identity-전략)
        - [SEQUENCE 전략](#👉-sequence-전략)
        - [TABLE 전략](#👉-table-전략)
        - [AUTO 전략](#👉-auto-전략)
3. [필드와 컬럼을 매핑하는 어노테이션](#필드와-컬럼을-매핑하는-어노테이션)
    - [@Column](#column)

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
