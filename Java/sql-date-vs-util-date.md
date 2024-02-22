## java.sql.Date와 java.util.Date
> ✨ 시, 분, 초의 유무

### java.util.Date
`"Mon Dec 30 12:30:30 GMT-0700 2008"`
```java
Date()
Date(long date)
Date(int year, int month, int date)
Date(int year, int month, int date, int hrs, int min)
Date(int year, int month, int date, int hrs, int min, int sec)
```

`java.util.Date`는 `Java 8` 이후로 권고되지 않으며, 대신 `java.time` 패키지 사용이 권고 된다.

> 📌 [java.time Package]()

- 1970년 1월 1일 00:00:00 GMT(Epoch Time 또는 POSIX Time) 이후의 특정 시점을 millisecond 단위로 나타낸다.

### java.sql.Date
`"2008-12-30 12:30:20"`
```java
Date(int year, int month, int day)
Date(long date)
```

`java.sql.Date`는 `java.util.Date`를 상속한다.

- 데이터베이스를 다룰 때 사용된다.

---
## references
- [https://velog.io/@zeroh0/java-Date](https://velog.io/@zeroh0/java-Date)