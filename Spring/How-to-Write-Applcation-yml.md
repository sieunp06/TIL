### 목차
- [application.yml  기본 설정](#applicationyml--기본-설정)
- [spring](#spring)
  - [프로필 분리하기](#프로필-분리하기)
  - [datasource](#datasource)
  - [sql](#sql)
  - [JPA](#jpa)
- [logging](#logging)
- [JWT(JSON Web Token)](#jwtjson-web-token)
- [references](#references)

---
## application.yml  기본 설정
`spring boot` 프로젝트의 다음 경로에 `application.yml`을 만든다.
```
root/src/main/resources
```
## spring
### 프로필 분리하기
하나의 `yml` 파일에서 프로필을 분리할 때는 `---`를 통해 나눠 작성하면 된다.
```yml
# default
spring:
    profiles:
        active: local   # 기본 환경을 local로 설정한다.

---
# local
spring:
    config:
        activate:
            on-profile: local

---
# prod
spring:
    config:
        activate:
            on-profile: prod
```
`default` 설정에서 공통되는 설정을 작성하고, 나머지 `local`이나 `prod` 각각 달라지는 설정을 작성하면 된다.

### datasource
```yml
spring:
    # ...
    datasource:
        url: ${DATABASE_URL}
        username: ${DATABASE_USERNAME}
        password: ${DATABASE_PASSWORD}
        driver-class-name: ${DATABASE_DRIVER}
```
- `url`: 데이터베이스의 JDBC URL
- `username`: 데이터베이스의 로그인 사용자 이름
- `password`: 데이터베이스의 로그인 비밀번호
- `driver-class-name`: JDBC 드라이버의 완전한 이름을 작성한다. 기본적으로 URL을 기반으로 자동 감지하게 된다.

### sql
```yml
spring:
    sql:
        init:
            mode: <embedded/always>
```
- `sql.init.mode`: 기본값은 `embedded`이며, `always`로 설정하면 모든 데이터베이스를 초기화 대상으로 변경한다.

### JPA
```yml
spring:
    # ...
    jpa:
        hibernate:
            ddl-auto: <create/create-drop/update/validate/none>
        properties:
            hibernate:
                dialect: <database_dialect>
                show_sql: <true/false>
                format_sql: <true/false>
                use_sql_comments: <true/false>
                jdbc:
                    batch_size: <batch_size_value>
                    fetch_size: <fetch_size_value>
                    named_query_check_regions: <named_query_check_regions_value>
                cache:
                    use_second_level_cache: <true/false>
                    use_query_cache: <true/false>
        defer-datasource-initialization: <true/false>
```
- `hibernate`
  - `ddl-auto`: DDL 모드를 설정한다.
    |종류|역할|
    |--|--|
    |create|기존 테이블을 삭제하고 다시 생성한다.|
    |create-drop|기존 테이블을 삭제하고 다시 생성한다. 종료 시점에 테이블을 삭제한다.|
    |update|변경된 스키마를 적용한다.|
    |validate|Entity와 테이블이 정상 매핑되었는지 확인한다.|
    |none|사용하지 않음|
    - 개발 초기 단계에는 local 서버에서 `create`와 `update`를 사용한다.
    - 여러 개발자가 함께 사용하는 테스트 서버에서는 `update`와 `validate`를 사용한다. 이떄, `create`를 쓰면 다른 개발자가 테스트한 데이터가 모두 날아가므로 주의한다.
    - 스테이징과 운영 서버는 `validate`와 `none`을 사용한다.
- `properties`
  - `hibernate`: JPA 공급자에 설정할 추가 기본 속성이다.
    - `dialect`: Hibernate가 사용할 데이터베이스 방언(Dialect)을 설정한다.
    - `show_sql`: Hibernate가 실행한 SQL 쿼리를 로그에 출력할지 여부를 설정한다.
    - `format_sql`: Hibernate가 실행한 SQL 쿼리를 포맷팅하여 로그에 출력할지 여부를 설정한다.
    - `use_sql_comments`: Hibernate가 실행한 SQL 쿼리에 주석을 추가할지 여부를 설정한다.
    - `jdbc`
      - `batch_size`: JDBC 배치 사이즈를 결정한다.
      - `fetch_size`: JDBC 페치 사이즈를 결정한다.
      - `named_query_check_regions`: named 쿼리 검증 여부를 설정한다.
    - `cache`
      - `use_second_level_cache`: Hibernate 두 번째 레벨 캐시 사용 여부를 설정한다.
      - `use_query_cache`: Hibernate 쿼리 캐시 사용 가능 여부를 설정한다.
- `defer-datasource-initialization`: Hibernate 초기화 이후 data.sql이 실행되도록 할지를 설정한다.

<br>

## logging
```yml
logging:
    level:
        org.hibernate.SQL: debug
```

## JWT(JSON Web Token)
```yml
jwt:
    header: Authorization
    secret: <secret key>
    token-validity-in-seconds: <seconds>
```
- `header`: 
- `secret`: secret key를 base64로 인코딩하여 작성한다.
- `token-validity-in-seconds`: 

---
## references
- [https://velog.io/@minbo2002/JPA-application.yml-%EC%84%A4%EC%A0%95](https://velog.io/@minbo2002/JPA-application.yml-%EC%84%A4%EC%A0%95)
- [https://developyoun.github.io/spring/Spring-JPA%EC%9D%98-application.yml-%EC%84%A4%EC%A0%95%ED%95%98%EB%8A%94-%ED%94%84%EB%A1%9C%ED%8D%BC%ED%8B%B0-%ED%95%AD%EB%AA%A9/](https://developyoun.github.io/spring/Spring-JPA%EC%9D%98-application.yml-%EC%84%A4%EC%A0%95%ED%95%98%EB%8A%94-%ED%94%84%EB%A1%9C%ED%8D%BC%ED%8B%B0-%ED%95%AD%EB%AA%A9/)
- [https://kkkdh.tistory.com/entry/Spring-datasql%EC%9D%84-%EC%9D%B4%EC%9A%A9%ED%95%9C-%ED%85%8C%EC%8A%A4%ED%8A%B8-%EB%8D%B0%EC%9D%B4%ED%84%B0-%EC%B6%94%EA%B0%80%ED%95%98%EA%B8%B0-%EC%8A%A4%ED%94%84%EB%A7%81-%EB%B6%80%ED%8A%B8-278-%EB%B2%84%EC%A0%84](https://kkkdh.tistory.com/entry/Spring-datasql%EC%9D%84-%EC%9D%B4%EC%9A%A9%ED%95%9C-%ED%85%8C%EC%8A%A4%ED%8A%B8-%EB%8D%B0%EC%9D%B4%ED%84%B0-%EC%B6%94%EA%B0%80%ED%95%98%EA%B8%B0-%EC%8A%A4%ED%94%84%EB%A7%81-%EB%B6%80%ED%8A%B8-278-%EB%B2%84%EC%A0%84)