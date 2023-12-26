# [Python] 문자열 관련 함수들
#### 목차
- [문자 개수 세기](#문자-개수-세기)
    - [count](#count)
- [문자 위치 찾기](#문자-위치-찾기)
    - [find](#find)
    - [index](#index)
- [문자열 삽입하기](#문자열-삽입)
    - [join](#join)
- [소문자-대문자 변경](#소문자-대문자-변경)
    - [upper](#upper)
    - [lower](#lower)
- [공백 지우기](#공백-지우기)
    - [lstrip](#lstrip)
    - [rstrip](#rstrip)
    - [strip](#strip)
- [문자열 바꾸기](#문자열-바꾸기)
    - [replace](#replace)
- [문자열 나누기](#문자열-나누기)
    - [split](#split)

## 문자 개수 세기
### count
문자열에서 매개변수로 입력된 문자의 개수를 반환한다.

> ❗이때 매개변수는 문자, 문자열 상관 없이 가능하다.

<br>

📌 매개변수가 문자일 때
```python
a = "hobby"
print(a.count('b'))
```
```
2
```

📌 매개변수가 문자열일 때
```python
b = "cocoa"
print(b.count("co"))
```
```
2
```

## 문자 위치 찾기
### find
문자열 중 입력받은 매개변수가 처음으로 나온 위치를 반환한다.

만약 문자가 문자열에 존재하지 않는다면 -1을 반환한다.

> ❗이때 매개변수는 문자, 문자열 상관 없이 가능하다.

<br>

📌 매개변수가 문자일 때
```python
a = "Python is the best choice"
print(a.find('b'))
print(a.find('k'))
```
```
14
-1
```

📌 매개변수가 문자열일 때
```python
b = "cocoa"
print(b.find("oa"))
```
```
3
```

### index
`find`와 동일하게 문자열 중 입력받은 매개변수가 처음으로 나온 위치를 반환한다.

```python
a = "Life is too short"
print(a.index('t'))
```
```
8
```

하지만 매개변수가 문자열에 존재하지 않을 때 `-1`을 반환하는 `find`와 달리 `index`는 오류가 발생한다.

```python
a = "Life is too short"
print(a.index('k'))
```
```
ValueError: substring not found
```

## 문자열 삽입
### join
`join`은 매개변수로 입력받은 요소를 합친다.
```python
a = ['a', 'b', 'c', 'd']
print("".join(a))
```
```
abcd
```
또한 `join`을 사용하여 합칠 때 구분자를 사이사이에 넣을 수 있다.
```python
b = ",".join("abcd")
print(b)
```
```
a,b,c,d
```

## 소문자-대문자 변경
### upper
`upper`은 문자열 중 소문자를 대문자로 바꿔준다.
```python
a = "sMall"
print(a.upper())
```
```
SMALL
```

### lower
`lower`은 `upper`와 반대로 소문자를 대문자로 바꾼다.
```python
b = "BiG"
print(b.lower())
```
```
big
```

## 공백 지우기
### lstrip
`lstrip`은 문자열 가장 왼쪽에 존재하는 한 칸 이상의 연속된 공백을 지운다.
```python
a = "     hi    "
print(a.lstrip())
```
```
hi    
```

### rstrip
`lstrip`이 왼쪽의 공백을 지운다면 `rstrip`은 가장 오른쪽에 존재하는 한 칸 이상의 연속된 공백을 지운다.
```python
b = "  hi    "
print(b.rstrip())
```
```
  hi
```

### strip
`strip`은 문자열 양쪽에 존재하는 한 칸 이상의 공백을 지운다.
```python
c = "     hi       "
print(c.strip())
```
```
hi
```

## 문자열 바꾸기
### replace
`replace`는 매개변수로 바뀔 문자열과 바꿀 문자열을 받아 바꿜 문자열을 바꿀 문자열로 치환한다.

아래 예시는 `Life`를 `Your leg`로 바꾼다는 의미이다.
```python
a = "Life is too short"
print(a.replace("Life", "Your leg"))
```
```
Your leg is too short
```

## 문자열 나누기
### split
`split`은 입력받은 매개변수에 따라 문자열을 나눈다.

```python
a = "a:b:c:d"
print(a.split(":"))
```
```
['a', 'b', 'c', 'd']
```
 
> ❗매개변수에 아무 것도 입력하지 않으면 공백(space, tab, enter)를 기준으로 문자열을 나눈다.

```python
b = "Life is too short"
print(b.split())
```
```
['Life', 'is', 'too', 'short']
```

-----
## 💎 References
- [점프 투 파이썬](https://wikidocs.net/13#_30)