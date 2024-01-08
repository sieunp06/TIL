# Spring JPA
#### 목차
1. [🐸SQL을 직접 다룰 때 발생하는 문제점](#🐸sql을-직접-다룰-때-발생하는-문제점과-jpa)
    - [👎문제점](#👎문제점)
        - [1️⃣ 반복, 반복 그리고 반복](#1️⃣-반복-반복-그리고-반복)
        - [2️⃣ SQL에 의존적인 개발](#2️⃣-sql에-의존적인-개발)
    - [👍JPA가 문제를 해결하는 방법](#👍jpa가-문제를-해결하는-방법)
        - [📌저장 기능](#📌저장-기능)
2. [🐸ORM과 JPA](#🐸orm과-jpa)
    - [ORM이란?](#orm이란)
        - [ORM 이해하기](#orm-이해하기)
    - [JPA란?](#jpa란)

## 🐸SQL을 직접 다룰 때 발생하는 문제점과 JPA
### 👎문제점
#### 1️⃣ 반복, 반복 그리고 반복
SQL을 직접 다루어 객체를 데이터베이스에 CRUD 할 때, 너무 많은 SQL과 JDBC API를 작성해야 한다.

테이블마다 많은 SQL과 JDBC API를 작성해야 하는데, 만약 테이블이 100개라면 이를 100번 반복하여 작성해야 한다.

#### 2️⃣ SQL에 의존적인 개발
SQL에 모든 것을 의존하는 상황에서는 개발자들이 엔티티를 신뢰하고 사용할 수 없다.

대신 DAO를 열어 어떤 SQL이 실행되고 있는지 확인해야 한다.

이는 진정한 의미의 계층 분할이 아니며, 물리적으로 SQL과 JDBC API를 DAO에 숨겼다고 해도 논리적으로는 매우 강한 의존관계를 가지고 있다.

이 때문에 필드 하나를 추가해도 DAO의 CRUD 코드와 SQL 대부분을 수정해야 한다.

- 진정한 의미의 계층 분할이 어렵다.
- 엔티티를 신뢰할 수 없다.
- SQL에 의존적인 개발을 피하기 어렵다.

### 👍JPA가 문제를 해결하는 방법
`JPA`를 사용하여 객체를 데이터베이스에 저장하고 관리할 때, 개발자가 SQL를 직접 작성하는 것이 아닌 `JPA API`를 사용한다.

이후 `JPA`가 적절한 `SQL`을 생성하여 데이터베이스에 전달한다.

#### 📌저장 기능
`persist()`는 객체를 데이터베이스에 저장한다.
```java
jpa.persist(member);    // 저장
```
`JPA`는 객체 매핑 정보에 따라 적절한 `INSERT SQL`을 생성하여 데이터베이스에 전달한다.

#### 📌조회 기능
`find()`는 객체 하나를 데이터베이스에서 조회한다.
```java
String memberId = "helloId";
Member member = jpa.find(Member.class, memberId);   // 조회
```
`JPA`는 객체 매핑 정보에 따라 적절한 `SELECT SQL`을 생성하여 데이터베이스에 전달하고 이 결과로 객체를 생성해서 반환한다.

#### 📌수정 기능
```java
Member member = jpa.find(Member.class, memberId);
member.setName("이름변경");     // 수정
```
`JPA`는 별도의 수정 기능을 제공하지 않는다.

하지만 객체 조회 후 변경하면 트랜잭션을 커밋할 때, 알아서 `UPDATE SQL`을 전달한다.

#### 📌연관된 객체 조회

## 🐸ORM과 JPA
### ORM이란?
> ORM(Object Relational Mapping)

`ORM`은 자바 문법으로 데이터베이스를 다룰 수 있도록 한다.

즉, ORM을 통해 개발자가 SQL을 직접 작성하지 않고 데이터베이스의 처리가 가능하게 된다.

#### ORM 이해하기
`ORM`은 데이터베이스의 테이블을 자바 클래스로 만들어 관리할 수 있다.


|id|subject|content|
|--|--|--|
|1|안녕하세요|가입했습니다!|
|2|질문 있습니다|ORM이 뭔가용|
|...|...|...|

위 `question` 테이블에 데이터를 저장하려면 다음 SQL 쿼리문을 작성해야 한다.

```sql
INSERT INTO question (id, subject, content) VALUES (1, '안녕하세요', '가입했습니다!');
INSERT INTO question (id, subject, content) VALUES (2, '질문 있습니다', 'ORM이 뭔가용');
```

이 SQL 쿼리문을 ORM을 사용하여 다음과 같이 나타낼 수 있다.

```java
Question q1 = new Question();
q1.setId(1);
q1.setSubject("안녕하세요");
q1.setContent("가입했습니다!");
this.questionRepository.save(q1);

Question q2 = new Question();
q2.setId(2);
q2.setSubject("질문 있습니다");
q2.setContent("ORM이 뭔가용");
this.questionRepository.save(q2);
```

### JPA란?
> JPA(Java Persistance API)

스프링 부트

-----
## 💎 References
- [자바 ORM 표준 JPA 프로그래밍](https://product.kyobobook.co.kr/detail/S000000935744)
- [점프 투 스프링부트](https://wikidocs.net/161164)