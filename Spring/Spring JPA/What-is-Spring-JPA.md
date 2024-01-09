# Spring JPA
#### 목차
1. [🐸SQL을 직접 다룰 때 발생하는 문제점](#🐸sql을-직접-다룰-때-발생하는-문제점과-jpa)
    - [👎문제점](#👎문제점)
        - [1️⃣ 반복, 반복 그리고 반복](#1️⃣-반복-반복-그리고-반복)
        - [2️⃣ SQL에 의존적인 개발](#2️⃣-sql에-의존적인-개발)
    - [👍JPA가 문제를 해결하는 방법](#👍jpa가-문제를-해결하는-방법)
        - [📌저장 기능](#📌저장-기능)
        - [📌조회 기능](#📌조회-기능)
        - [📌수정 기능](#📌수정-기능)
        - [📌연관된 객체 조회](#📌연관된-객체-조회)
2. [🐸패러다임 불일치](#🐸패러다임-불일치)
3. [🐸ORM과 JPA](#🐸orm과-jpa)
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
```java
Member member = jpa.find(Member.class, memberId);
Team team = member.getTeam();   // 연관된 객체 조회
```
`JPA`는 연관된 객체를 사용하는 시점에 적잘한 `SELECT SQL`을 실행한다.

## 🐸패러다임 불일치
### 객체와 관계형 데이터베이스의 패러다임 불일치 문제
객체와 관계형 데이터베이스는 지향하는 목적이 서로 다르기 때문에 둘의 기능과 표현 방법이 다르다. <br>
👉 `객체와 관계형 데이터베이스의 패러다임 불일치 문제`

애플리케이션은 객체지향 언어인 자바로 개발하고 데이터는 관계형 데이터베이스에 저장해야 한다면, 이때 발생하는 패러다임 불일치 문제를 개발자가 중간에서 해결해야 한다.

### 패러다임 불일치로 발생하는 문제들과 해결
#### 📌상속
객체는 `상속`이 가능하지만, 테이블은 `상속`이 불가능하다.

👉 데이터베이스 모델링에서는 슈퍼타입 서브타입 관계를 사용하여 객체 상속과 유사한 형태로 만들 수 있다.

```java
abstract class Item {
    Long id;
    String name;
    int price
}

class Album extends Item {
    String artist;
}

class Movie extends Item {
    String director;
    String actor;
}

class Book extends Item {
    String author;
    String isbn;
}
```
`Album` 객체를 저장하려면 이 객체를 분해해 두 SQL을 만들어야 한다.
```sql
INSERT INTO ITEM ...
INSERT INTO ALBUM ...
```
JDBC API를 사용하여 위 코드를 완성하려면 부모 객체에서 부모 데이터만 꺼내서 `Item`용 `INSERT SQL`을 작성하고, 자식 객체에서 자식 데이터만 꺼내서 `Album`용 `INSERT SQL`을 작성하야 한다.

👉 패러다임 불일치 문제를 해결하기 위해 불필요한 비용이 소모된다.

`JPA`는 상속과 관련된 패러다임 불일치 문제를 개발자 대신 해결한다.

```java
jpa.persist(album);
```
위 코드는 `JPA`를 사용하여 `Item`을 상속한 `Album` 객체를 저장한다.

`JPA`는 알아서 다음 `INSERT SQL`을 실행하여 객체를 `ITEM`과 `ALBUM` 두 테이블에 저장한다.

```sql
INSERT INTO ITEM ...
INSERT INTO ALBUM ...
```

개발자가 직접 SQL 코드를 작성하지 않아도 된다.

#### 📌연관관계
객체는 참조를 사용하여 다른 객체와 연관관계를 가지고 참조에 접근해서 다른 객체를 조회한다. 

반면 테이블은 외래키를 사용하여 다른 테이블과 연관관계를 가지고 조인을 활용하여 연관된 테이블을 조회한다.

```java
class Member {
    Team team;
    ... 
    Team getTeam() {
        return team;
    }
}
```
```java
class Team {

}
```

`Member` 객체는 `Member.team` 필드에 `Team` 객체의 참조를 보관하여 `Team` 객체와 연관관계를 맺는다.

```sql
SELECT M.*, T.*
    FROM MEMBER M
    JOIN TEAM T ON M.TEAM_ID = T.TEAM_ID
```

테이블 같은 경우, `MEMBER.TEAM_ID` 외래 키 컬럼을 사용하여 `TEAM` 테이블과 연관관계를 맺는다.

외래키를 사용하여 `MEMBER` 테이블과 `TEAM` 테이블을 조인하여 `MEMBER` 테이블과 연관된 `TEAM` 테이블을 조회할 수 있다.

이때, 테이블은 외래 키 하나로 `MEMBER JOIN TEAM`, `TEAM JOIN MEMBER` 모두 가능하지만, 객체 같은 경우 참조가 있는 방향으로만 조회할 수 있다.

#### 📌 객체를 테이블에 맞추어 모델링
```java
class Member {
    String id;  // MEMBER_ID 컬럼 사용
    Long teamId;    // TEAM_ID FK컬럼 사용
    String userName;    // USERNAME 컬럼 사용
}

class Team {
    Long id;    // TEAM_ID PK 사용
    String name;    // NAME 컬럼 사용
}
```
위 예제는 `MEMBER` 테이블의 컬럼을 그대로 가져와 `Member` 클래스를 구성하였다.

객체를 테이블에 맞게 모델링 했을 때, 객체를 테이블에 저장하고 조회할 때는 편리하지만 객체는 참조를 통해 연관관계를 맺기 때문에 연관된 참조를 보관해야 참조를 통해 연관된 객체를 찾을 수 있다.

이러한 방식을 따르면 좋은 객체지향 모델링은 기대하기 어렵고, 객체지향의 특징을 잃어버리게 된다.

#### 📌객체지향 모델링
```java
class Member {
    String id;  // MEMBER_ID 컬럼 사용
    Team team;  // 참조로 연관관계를 맺음.
    String username;    // USERNAME 컬럼 사용

    Team getTeam() {
        return team;
    }
}
```
```java
class Team {
    Long id;    // TEAM_ID PK 사용
    String name;    // NAME 컬럼 사용
}
```

객체를 테이블에 맞추어 모델링을 했을 때와 달리 참조를 통해 연관관계를 맺었기 때문에 회원관 연관된 팀을 조회할 수 있다.

하지만 객체지향 모델링을 사용하면 객체를 테이블에 저장하거나 조회하기 어렵다.

객체 모델은 참조만 필요하고 외래 키가 필요 없는 반면, 테이블은 참조가 필요 없고 외래 키만 필요하기 때문이다. 

👉 개발자가 중간에서 변환 역할을 수행해야 한다.

#### 📌JPA와 연관관계
`JPA`는 연관관계와 관련된 패러다임 불일치 문제를 해결한다.

```java
member.setTeam(team);   // 회원과 팀의 연관관계 설정
jpa.persist(member);    // 회원과 연관관계 함께 저장
```
JPA는 `team`의 참조를 외래 키로 변환하여 적절한 `INSERT SQL`을 데이터베이스에 전달한다.

### 객체 그래프 탐색
연관관계와 관련해 극복하기 어려운 패러다임의 불일치 문제이다.

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