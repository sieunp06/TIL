# [Python] 문자열 포맷팅
#### 목차
- [%를 사용하는 방법](#를-사용하는-방법)
- [format 함수를 사용하는 방법](#format-함수를-사용하는-방법)
- [f문자열 포맷팅을 사용하는 방법](#f문자열-포맷팅을-사용하는-방법)

-----

## %를 사용하는 방법
`%` 이후에 포맷 코드를 붙여 문자열을 포맷할 수 있다.

#### 📌 숫자를 대입하는 방법
```python
print("I eat %d apples" % 3)
```
```
I eat 3 apples
```

#### 📌 문자열을 대입하는 방법
```python
print("I eat %s apples" % "three")
```
```
I eat three apples
```

#### 📌 2개 이상의 값 대입하는 방법
2개 이상의 값을 대입하려면 `%` 다음 괄호 안에 `,`로 대입할 값을 작성하면 된다.
```python
number = 10
day = "three"

print("I ate %d apples. So I was sick for %s days." % (number, day))
```
```
I ate 10 apples. So I was sick for three days.
```

### 포맷 코드
|코드|설명|
|---|---|
|%s|문자열(String)|
|%c|문자(Character)|
|%d|정수(Integer)|
|%f|부동소수(floating-point)|
|%o|8진수|
|%x|16진수|
|%%|Literal %(문자 % 자체)|

### 포맷코드와 숫자 함께 사용하기
#### 📌 정렬과 공백
아래 예시는 전체 길이가 10인 문자열에서 대입되는 값을 오른쪽에 정렬하고 나머지를 공백으로 채운다는 의미이다.
```python
print("%10s" % "hi")
```
```
        hi
```

반대로 아래 예시는 대입되는 값을 왼쪽에 정렬하고 나머지를 공백으로 채운다는 의미와 같다.
```python
print("%-10sthere" % "hi")
```
```
hi        there
```

#### 📌 소수점 표현
소수점을 표현하고 싶을 때는 `%0.4f`와 같은 형태로 작성한다.

`.`은 소수점 포인트, 그 뒤 숫자 `4`는 소수점 뒤 숫자의 개수를 의미한다.

```python
print("%0.4f" % 3.42134234)
```
```
3.4213
```
위 예시는 `0`을 생략하여 다음과 같이 표현되기도 한다.

```python
print("%.4" % 3.42134234)
```
```
3.4213
```
같은 표현이기 때문에 결과 값은 같다.

`0.4f`에서 `0`은 문자열의 전체 길이를 나타낸다.

`0`을 사용했을 경우는 문자열의 길이에 상관하지 않는 뜻이며, 만약 `%10.4f`라면 아래 예시와 같다.

```python
print("%10.4f" % 3.42134234)
```
```
    3.4213
```
위 예시는 소수점 다음 4자리까지만 표현하기 때문에 `3.4213`를 표시하고, 이 값을 오른쪽에 정렬하고 전체 문자열의 길이 `10`의 나머지를 공백으로 채운다.

## format 함수를 사용하는 방법
`format`을 사용하면 더 발전된 방식으로 값을 문자열에 대입할 수 있다.

#### 📌 숫자를 대입하는 방법
```python
print("I eat {0} apples".format(3))
```
```
I eat 3 apples
```

#### 📌 문자열을 대입하는 방법
```python
print("I eat {0} apples".format("five"))
```
```
I eat five apples
```

#### 📌 숫자 값을 가진 변수를 대입하는 방법
```python
number = 3
print("I eat {0} apples".format(number))
```
```
I eat 3 apples
```

#### 📌 2개 이상의 값을 대입하는 방법
`%`을 사용하여 값을 대입하는 방법과 같이 `,`로 값을 분리하여 입력한다.

다른 점은 대입되는 문자열의 인덱스를 구분한다.
```python
number = 3
day = "three"

"I ate {0} apples. so I was sick for {1} days.".format(number, day)
```
```
I ate 3 apples. so I was sick for three days.
```

### 인덱스 대신 name 사용하기
`{0}`, `{1}` 등의 인덱스를 사용하는 대신 `name`을 사용할 수 있다.
```python
print("I ate {number} apples. so I was sick for {day} days.".format(number=10, day=3))
```
```
I ate 10 apples. so I was sick for 3 days.
```

#### 인덱스와 name 혼용하여 사용하기
```python
print("I ate {0} apples. so I was sick for {day} days.".format(10, day=3))
```
```
I ate 10 apples. so I was sick for 3 days.
```

인덱스로 여러 값을 대입하고 싶다면 다음과 같이 사용하면 된다.

```python
print("I ate {0} apples. so I was sick for {day} days. {1}".format(10, 3, day=3))
```
```
I ate 10 apples. so I was sick for 3 days. 3
```
`name`을 사용한 값을 맨 뒤에 적고, 그 앞에 인덱스를 사용한 값을 넣는다.

### 정렬과 공백
#### 📌 정렬
`{0:>10}`과 같은 형태로 쓰인다.

왼쪽 정렬은 `:<`가 쓰이고, 오른쪽 정렬은 `:>`, 가운데 정렬은 `:^`가 쓰인다.

이후 `10`은 문자열의 총 길이를 10으로 맞추고 남은 공간을 공백으로 채운다는 의미이다.

- 왼쪽 정렬
    ```python
    print("{0:<10}".format("hi"))
    ```
    ```
    `hi        `
    ```
- 오른쪽 정렬
    ```python
    print("{0:>10}".format("hi"))
    ```
    ```
            hi
    ```
- 가운데 정렬
    ```python
    print("{0:^10}".format("hi"))
    ```
    ```
    `    hi    `
    ```

#### 📌 공백 채우기
공백을 특정 문자로 채우고 싶다면 정렬 시 `:`와 `>`, `<`, `^` 사이에 지정하고 싶은 문자를 넣으면 된다.
```python
print("{0:=^10}".format("hi"))
```
```
====hi====
```

### 소수점 표현
아래는 소수점 4자리까지만 표현하는 예시이다.
```python
y = 3.42134234

print("{0:0.4f}".format(y))
```
```
3.4213
```
위 `%` 포맷팅 방법과 동일한 방식이다.

동일하게 문자열의 자릿수를 맞출 수 있다.
```python
y = 3.42134234

print("{0:10.4f}".format(y))
```
```
    3.4213
```

## f문자열 포맷팅을 사용하는 방법
python `3.6`부터 사용할 수 있는 기능이다.

다음과 같이 사용한다.
```python
number = 10
day = 3

print(f'I ate {number} apples. so I was sick for {day} days.')
```
```
I ate 10 apples. so I was sick for 3 days.
```
`f문자열 포맷팅`은 변수를 가져와 참조하는 방식으로, 표현식을 지원하기 때문에 중괄호 안의 변수를 계산식과 사용할 수 있다.

```python
number = 10
day = 3

print(f'I ate {number + 3} apples. so I was sick for {day} days.')
```
```
I ate 13 apples. so I was sick for 3 days.
```

딕셔너리 또한 사용할 수 있다.
```python
dic = {'number': 10, 'day': 3}

print(f'I ate {dic["number"]} apples. so I was sick for {dic["day"]} days.')
```
```
I ate 10 apples. so I was sick for 3 days.
```

### 정렬과 공백
#### 📌 정렬하기
```python
print(f'{"hi":<10}')
print(f'{"hi":>10}')
print(f'{"hi":^10}')
```
```
hi        
        hi
    hi    
```

#### 📌 공백 채우기
```python
print(f'{"hi":=^10}')
print(f'{"hi":!<10}')
```
```
====hi====
hi!!!!!!!!
```

### 소수점 표현
```python
num = 3.42134234

print(f'{num:0.4f}')
print(f'{num:10.4f}')
```
```
3.4213
    3.4213
```
