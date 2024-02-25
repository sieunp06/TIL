# java.time Package
`java.time` 패키지는 `java.util.Calendar`의 단점을 보완한 패키지로, 다음 하위 패키지를 포함하고 있다.

- java.time.chrono: ISO-8601에 정의된 표준 달력 이외의 달력 시스템을 사용할 때 필요한 클래스들
- java.time.format: 날짜와 시간에 대한 데이터를 파싱하고 형식화하는 데 사용되는 클래스들
- java.time.temporal: 날짜와 시간에 대한 데이터를 연산하는 데 사용되는 보조 클래스들
- java.time.zone: 타임 존(time-zone)과 관련된 클래스들

## java.time Package에 포함된 클래스들
`java.time` 패키지는 다음 5개의 클래스가 포함되어 있다.

- LocalDate
- LocalTime
- LocalDateTime
- ZonedDateTime
- Instant

### TemporalField 인터페이스
`TemporalField` 인터페이스는 날짜와 시간에 관련된 필드를 정의해 놓은 인터페이스다.

|열거체 상수|설명|
|--|--|
|ERA|시대|
|YEAR|연도|
|MONTH_OF_YEAR|월|
|DAY_OF_MONTH|일|
|DAY_OF_WEEK|요일 (월요일:1, 화요일:2, ..., 일요일:7)|
|AMPM_OF_DAY|오전/오후|
|HOUR_OF_DAY|시(0~23)|
|CLOCK_HOUR_OF_DAY|	시(1~24)|
|HOUR_OF_AMPM|시(0~11)|
|CLOCK_HOUR_OF_AMPM|시(1~12)|
|MINUTE_OF_HOUR|분|
|SECOND_OF_MINUTE|초|
|DAY_OF_YEAR|해당 연도의 몇 번째 날 (1~365, 윤년이면 366)|
|EPOCH_DAY|	EPOCH(1970년 1월 1일)을 기준으로 몇 번째 날|

```java
LocalTime present = LocalTime.now();
String ampm;

if (present.get(ChronoField.AMPM_OF_DAY) == 0) {
    ampm = "오전";
} else {
    ampm = "오후";
}

System.out.println(ampm);       // 오전
System.out.println(present.get(ChronoField.HOUR_OF_AMPM));  // 11
```

## LocalDate와 LocalTime
### 📌 날짜와 시간 객체 생성하기: now()와 of()
`now()`는 현재 날짜와 시간을 저장하는 객체를 생성하여 반환한다.

```java
LocalDate today = LocalDate.now();          // 2024-02-25
LocalTime currentTime = LocalTime.now();    // 09:21:50.634
```

`of()`는 전달된 인수를 가지고 특정 날짜와 시간을 표현하는 객체를 생성하여 반환한다.

```java
static LocalTime of(int hour, int minute, int second, int nanoOfSecond)
```

이때 `LocalTime` 객체를 `of`로 생성할 때 위와 같이 인수를 전달해야 한다.

```java
LocalDate birthDay = LocalDate.of(2001, 06, 06);            // 2001-06-06
LocalTime birthTime = LocalTime.of(02, 02, 00, 100000000);  // 02:02:00.100
```

### 📌 날짜와 시간 객체에 접근하기
`LocalDate`와 `LocalTime` 클래스는 특정 필드의 값을 가져오기 위해 getter 메서드를 제공한다.

#### 👉 `LocalDate`의 대표적인 getter 메서드

|메서드|설명|
|--|--|
|int get(TemporalField field)<br>long getLong(TemporalField field)|해당 날짜 객체의 명시된 필드의 값을 int형이나 long 형으로 반환한다.|
|int getYear()|해당 날짜 객체의 연도(YEAR) 필드를 반환한다.|
|Month getMonth()|해당 날짜 객체의 월(MONTH_OF_YEAR) 필드의 값을 Month 열거체를 이용하여 반환한다.|
|int getMonthValue()|해당 날짜 객체의 월(MONTH_OF_YEAR) 필드의 값을 반환한다. (1~12)|
|int getDayOfMonth()|해당 날짜 객체의 일(DAY_OF_MONTH) 필드의 값을 반환한다. (1~31)|
|int getDayOfYear()|해당 날짜 객체의 일(DAY_OF_YEAR) 필드의 값을 반환한다. (1~365, 윤년이면 366)|
|DayOfWeek getDayOfWeek()|해당 날짜 객체의 요일(DAY_OF_WEEK) 필드의 값을 DayOfWeek 열거체를 이용하여 반환한다.|

#### 👉 `LocalTime`의 대표적인 getter 메서드

|메서드|설명|
|--|--|
|int get(TemporalField field)<br>long getLong(TemporalField field)|해당 시간 객체의 명시된 필드의 값을 int형이나 long형으로 반환한다.|
|int getHour()|해당 시간 객체의 시(HOUR_OF_DAY) 필드의 값을 반환한다.|
|int getMinute()|해당 시간 객체의 분(MINUTE_OF_HOUR) 필드의 값을 반환한다.|
|int getSecond()|해당 시간 객체의 초(SECOND_OF_MINUTE) 필드의 값을 반환한다.|
|int getNano()|해당 시간 객체의 나노초(NANO_OF_SECOND) 필드의 값을 반환한다.|

### 📌 날짜와 시간 객체의 필드값 변경
`LocalDate`와 `LocalTime` 클래스는 날짜와 시간 객체에 접근하여 특정 필드의 값을 변경하기 위해 `with()`을 사용해야 한다.

#### 👉 `LocalDate`의 대표적인 with 메서드

|메서드|설명|
|--|--|
|LocalDate with(TemporalField field, long new Value)|해당 날짜 객체에서 특정 필드를 전달된 새로운 값으로 설정한 새로운 날짜 객체를 반환한다.|
|LocalDate withYear(int year)|해당 날짜 객체에서 연도(YEAR) 필드를 전달된 새로운 값으로 설정한 새로운 날짜 객체를 반환한다.|
|LocalDate withMonth(int month)|해당 날짜 객체에서 월(MONTH_OF_YEAR) 필드를 전달된 새로운 값으로 설정한 새로운 날짜 객체를 반환한다.|
|LocalDate withDayOfMonth(int dayOfMonth)|해당 날짜 객체에서 일(DAY_OF_MONTH) 필드를 전달된 새로운 값으로 설정한 새로운 날짜 객체를 반환한다.|
|LocalDate withDayOfYear(int dayOfYear)|해당 날짜 객체에서 DAY_OF_YEAR 필드를 전달된 새로운 값으로 설정한 새로운 날짜 객체를 반환한다.|

```java
LocalDate today = LocalDate.now();
System.out.println(today.getYear());        // 2024

LocalDate otherDay = today.withYear(2001);  
System.out.println(otherDay);               // 2001
```

#### 👉 `LocalTime`의 대표적인 with 메서드

|메서드|설명|
|--|--|
|LocalTime with(TemporalField field, long newValue)|해당 시간 객체에서 특정 필드를 전달된 새로운 값으로 설정한 새로운 시간 객체를 반환한다.|
|LocalTime withHour(int hour)|해당 시간 객체에서 특정 필드를 전달된 새로운 값으로 설정한 새로운 시간 객체를 반환한다.|
|LocalTime withMinute(int minute)|해당 시간 객체에서 시(HOUR_OF_DAY) 필드를 전달된 새로운 값으로 설정한 새로운 시간 객체를 반환한다.|
|LocalTime withSecond(int second)|해당 시간 객체에서 초(SECOND_OF_MINUTE) 필드를 전달된 새로운 값으로 설정한 새로운 시간 객체를 반환한다.|
|LocalTime withNano(int nanoOfSecond)|	해당 시간 객체에서 나노초(NANO_OF_SECOND) 필드를 전달된 새로운 값으로 설정한 새로운 시간 객체를 반환한다.|

```java
LocalTime present = LocalTime.now();
System.out.println(present.getHour());          // 17

LocalTime otherTime = present.withHour(8);  
System.out.println(otherTime);                  // 8
```

### 📌 날짜와 시간 객체의 연산
`LocalDate`와 `LocalTime`에서 날짜와 시간 객체의 연산은 `plus`와 `minus`를 통해 할 수 있다.

```java
LocalTime present = LocalTime.now();
System.out.println(present.get(ChronoField.HOUR_OF_DAY));       // 9

LocalTime otherTime = present.plus(2, ChronoUnit.HOURS);
System.out.println(otherTime.getHour());                        // 11

LocalTime anotherTime = present.minus(6, ChronoUnit.HOURS);
System.out.println(anotherTime.getHour());                      // 5
```

### 📌 날짜와 시간 객체의 비교
날짜와 시간 객체를 비교하는 메서드는 다음과 같다.

- isEquals()
- isBefore()
- isAfter()

이때 `isEquals()`는 `LocalDate` 클래스에서만 제공된다.

--- 
## references
- [https://tcpschool.com/java/java_time_javaTime](https://tcpschool.com/java/java_time_javaTime)
- [https://tcpschool.com/java/java_time_localDateTime](https://tcpschool.com/java/java_time_localDateTime)
- [https://happyeuni.tistory.com/m/80](https://happyeuni.tistory.com/m/80)