### 목차
- [COUNT](#count)
  - [Null 값을 포함한 모든 값 세기](#null-값을-포함한-모든-값-세기)
  - [Null 값을 제외한 값 세기](#null-값을-제외한-값-세기)
  - [Null 값과 중복된 값을 제외한 값 세기](#null-값과-중복된-값을-제외한-값-세기)
- [AVG](#avg)
  - [Null을 무시할 때](#null을-무시할-때)
  - [Null을 0으로 할 때](#null을-0으로-할-때)
- [MIN과 MAX](#min과-max)
- [References](#references)


---
## COUNT
`COUNT`는 행의 개수를 센다.

### Null 값을 포함한 모든 값 세기
```SQL
SELECT COUNT(*) FROM sample;
```

위와 같이 `*`를 사용하면 `Null` 값도 함께 센다.

<br>

### Null 값을 제외한 값 세기
```SQL
SELECT COUNT(컬럼명) FROM sample;
```

<br>

### Null 값과 중복된 값을 제외한 값 세기
```SQL
SELECT COUNT(DISTINCT 컬럼명) FROM sample;
```

## AVG
`AVG`는 평균을 구한다.

<br>

`AVG`는 `Null`을 무시할지, 0으로 볼지에 따라 달라지게 된다.

### Null을 무시할 때
```SQL
SELECT AVG(컬럼명) FROM sample;
```

### Null을 0으로 할 때
```SQL
SELECT SUM(컬럼명)/COUNT(*) FROM sample;
```

## MIN과 MAX
`MIN`은 최소값을, `MAX`는 최대값을 보여준다.

```SQL
SELECT MIN('컬럼명') FROM sample;
SELECT MAX('컬럼명') FROM sample;
```

---
## References
- [https://velog.io/@ifyouseeksoomi/Mysql-%EC%A7%91%EA%B3%84%ED%95%A8%EC%88%98](https://velog.io/@ifyouseeksoomi/Mysql-%EC%A7%91%EA%B3%84%ED%95%A8%EC%88%98)