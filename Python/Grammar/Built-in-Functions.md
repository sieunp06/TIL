# [Python] 내장 함수
#### 목차
- [abs](#abs)
- [all](#all)
- [any](#any)
- [chr](#chr)
- [ord](#ord)
- [dir](#dir)
- [divmod](#divmod)
- [enumerate](#enumerate)
- [eval](#eval)
- [filter](#filter)
- [hex](#hex)
- [oct](#oct)
- [id](#id)
- [input](#input)
- [int](#int)
- [isinstance](#isinstance)
- [len](#len)
- [list](#list)
- [map](#map)
- [max](#max)
- [min](#min)
- [pow](#pow)
- [range](#range)
- [round](#round)
- [sorted](#sorted)
- [str](#str)
- [sum](#sum)
- [tuple](#tuple)
- [type](#type)
- [zip](#zip)
-----

### abs
`abs`는 어떠한 숫자를 입력받았을 때, 그 절댓값을 반환하는 함수이다.
```python
print(abs(3))
print(abs(-3))
print(abs(-1.2))
```
```
3
3
1.2
```

### all
`all`은 반복 가능한 값을 입력받고 해당 값이 모두 참이면 `True`를, 하나라도 거짓이 존재하면 `False`를 반환한다.

```python
print(all([1, 2, 3]))
```
```
True
```
위 예제의 `[1, 2, 3]`은 모든 요소가 참이기 때문에 `True`를 반환한다.

```python
print(all([1, 2, 3, 0]))
```
```
False
```
위 예제의 요소 중 `0`은 `False`를 의미하기 때문에 나머지 요소가 모두 참이더라도 `False`를 반환한다.

> 📌 [[Python] 자료형의 참과 거짓](https://github.com/sieunp06/TIL/blob/main/Python/True-and-False-of-Data-Types.md)

### any
`any`는 모든 요소가 모두 참이면 `True`를 반환하는 `all`과 달리 하나라도 참이 있으면 `True`를, 모든 요소가 거짓이면 `False`를 반환한다.

```python
print(any([1, 2, 3]))
```
```
True
```
위 예제는 `all`과 같이 모든 요소가 참이기 때문에 `any`를 사용했을 때도 `True`를 반환한다.

```python
print(any([1, 2, 3, 0]))
```
```
True
```
위 예제는 `0`가 거짓을 뜻하지만, 다른 요소에 참이 포함되어 있기 떄문에 `True`를 반환한다.

```python
print(any(["", 0]))
```
```
False
```
위 예제는 모든 요소가 `False`기 떄문에 `False`를 반환한다.

> 📌 [[Python] 자료형의 참과 거짓](https://github.com/sieunp06/TIL/blob/main/Python/True-and-False-of-Data-Types.md)

### chr
`chr`은 유니코드 숫자 값을 입력받아 그 코드에 해당하는 문자를 리턴한다.

```python
print(chr(97))
```
```
a
```

### ord
`ord`는 `chr`와 반대로 문자의 유니코드 숫자값을 반환한다.

```python
print(ord('a'))
```
```
97
```

### dir
`dir`은 객체가 지닌 변수나 함수를 보여준다.

```python
print(dir([1, 2, 3]))
```
```
['__add__', '__class__', '__class_getitem__', '__contains__', ....
```

### divmod
`divmod`는 두 개의 숫자 a와 b를 매개변수로 받아 a를 b로 나눈 몫과 나머지를 튜플로 반환한다.

```python
print(divmod(7, 3))
```
```
(2, 1)
```

### enumerate
`enumerate`는 순서가 있는 데이터(리스트, 튜플, 문자열)를 입력 받아 인덱스 값을 포함하는 `enumerate 객체`를 반환한다.

`enumerate`는 보통 `for`문과 함께 사용한다.
```python
for element in enumerate(['body', 'foo', 'bar']):
    print(element)
```
```
(0, 'body')
(1, 'foo')
(2, 'bar')
```
`enumerate`객체에 인덱스와 해당 인덱스 값이 포함되어 있는 것을 볼 수 있습니다.

만약 인덱스와 해당 인덱스의 원소를 다른 변수에 할당하고 싶다면, 아래처럼 인자풀기(unpacking)을 하면 된다.

> 📌 [[Python] Packing과 Unpacking]()

```python
for i, name in enumerate(['body', 'foo', 'bar']):
    print(i, name)
```
```
0 body
1 foo
2 bar
```

#### 시작 인덱스 변경
시작 인덱스를 변경하고 싶다면 `start` 인자를 추가하면 된다.

```python
for i, name in enumerate(['body', 'foo', 'bar'], start=1):
    print(i, name)
```
```
1 body
2 foo
3 bar
```

### eval
`eval`은 문자열로 구성된 표현식을 매개변수로 받아 이를 실행한 결과값을 반환한다.

```python
print(eval("1 + 2"))
```
```
3
```

### filter
`filter`는 원하는 조건에 맞게 데이터를 걸러내며, 형식은 아래와 같다.

```python
filter(함수, 반복_가능한_데이터)
```
반복 가능한 데이터를 요소 순서대로 함수 호출했을 때 그 반환값이 참인 값만 걸러서 리턴한다.

```python
def positive(n):
    return n > 0


print(list(filter(positive, [1, -3, 2, 0, -5, 6])))
```
```
[1, 2, 6]
```
위 예시는 `[1, -3, 2, 0, -5, 6]`의 요소들을 순서대로 `positive` 함수에 적용하여 리턴값이 `True`인 값만 모아 `list`로 출력한다.

### hex
`hex`는 입력받은 정수를 16진수 문자열로 변환하여 반환한다.

```python
print(hex(234))
```
```
0xea
```

### oct
`oct`는 정수를 8진수로 변환하여 반환한다.

```python
print(oct(34))
```
```
0o42
```

### id
`id`는 객체를 입력받아 해당 객체의 고유 주소값(레퍼런스)를 반환한다.

```python
a = 3
print(id(3))
print(id(a))
```
```
140709802885992
140709802885992
```

### input
`input`은 사용자 입력을 받는다.

```python
a = input()     # input: hi
print(a)
```
```
hi
```

`input`의 입력 인수로 문자열을 전달하면 그 문자열은 프롬프트가 된다.

```python
a = input("say hi: ")     # input: hi
print(a)
```
```
say hi: hi
hi
```

### int
`int`는 문자열로 주어진 숫자나 소수점이 있는 숫자를 정수로 변환하여 반환한다.

이때, 정수가 입력되면 그대로 리턴한다.

```python
print(int('3'))
print(int(3.3))
```
```
3
3
```

#### radix
`radix` 인자는 `radix` 진수로 표현된 문자열을 10진수로 변환하여 반환한다.
```python
print(int('11', 2))
```
```
3
```
위 예제는 `radix`가 2이기 때문에 2진수로 표현된 `11`을 10진수로 변환하여 반환한다.

### isinstance
`isinstance`는 아래와 같은 형식을 가지고 있다.
```python
isinstance(object, class)
```
첫 번째 매개변수로 받은 객체가 두 번째 매개변수인 클래스의 인스턴스인지 판단한다.

```python
class Person: pass

a = Person()
print(isinstance(a, Person))
```
```
True
```
위 예제에서 `a`는 `Person` 클래스에 의해 생성된 인스턴스이기 떄문에 `True`를 반환한다.

```python
b = 3
print(isinstance(b, Person))
```
```
False
```
위 예제에서 `b`는 `Person`에 의해 생성된 인스턴스가 아니기 때문에 `False`를 반환한다.

### len
`len`은 매개변수로 받은 요소의 전체 개수를 반환한다.
```python
print(len("python"))
print(len([1, 2, 3]))
```
```
6
3
```

### list
`list`는 반복 가능한 데이터를 입력받아 이를 리스트로 변환하여 반환한다.

```python
print(list("python"))
```
```
['p', 'y', 't', 'h', 'o', 'n']
```

만약 `list` 함수에 리스트를 입력하면 해당 리스트를 복사하여 반환한다.

```python
a = [1, 2, 3]
b = list(a)
print(b)
```
```
[1, 2, 3]
```

### map
`map`은 함수와 반복 가능한 데이터를 매개변수로 받고, 입력받은 데이터의 각 요소를 함수에 적용한 결과를 반환한다.

```python
def two_times(x):
    return x * 2

print(list(map(two_times, [1, 2, 3, 4])))
```
```
[2, 4, 6, 8]
```

### max
`max`는 반복 가능한 데이터를 입력 받아 최댓값을 반환한다.

```python
print(max([1, 2, 3]))
print(max("python"))
```
```
3
y
```

### min
`min`은 `max`와 반대로 반복 가능한 데이터의 각 요소 중 최솟값을 반환한다.

```python
print(min([1, 2, 3]))
print(min("python"))
```
```
1
h
```

### pow
`pow`는 두 개의 숫자 매개변수 x, y를 받으며, x를 y 제곱한 값을 반환한다.

```python
print(pow(2, 4))
```
```
16
```

### range
`range`는 입력받은 숫자에 해당하는 범위 값을 반복 가능한 객체로 만들어 반환한다.

#### 📌 인수가 하나일 경우
```python
range(stop)
```
인수가 하나일 경우, 시작 숫자는 자동으로 0이 된다.
```python
print(list(range(5)))
```
```
[0, 1, 2, 3, 4]
```

#### 📌 인수가 2개일 경우
```python
range(start, stop)
```
인수가 두 개일 경우, 시작 숫자와 끝 숫자를 나타낸다.
```python
print(list(range(5, 10)))
```
```
[5, 6, 7, 8, 9]
```

#### 📌 인수가 3개일 경우
```python
range(start, stop, step)
```
인수가 세 개일 경우, 시작 숫자와 끝 숫자 그리고 숫자 사이의 거리를 나타낸다.
```python
print(list(range(1, 10, 2)))
```
```
[1, 3, 5, 7, 9]
```

### round
`round`는 숫자를 입력받아 이를 반올림해 반환한다.

```python
print(round(4.6))
print(round(4.2))
```
```
5
4
```

반올림하여 표시하고 싶은 소수점 자릿수도 지정할 수 있다.
```python
print(round(5.678, 2))
```
```
5.68
```

### sorted
`sorted`는 입력 데이터를 정렬하고 정렬한 결과를 리스트로 반환한다.

```python
print(sorted([3, 1, 2]))
print(sorted(['a', 'c', 'b']))
print(sorted("zero"))
print(sorted((3, 2, 1)))
```
```
[1, 2, 3]
['a', 'b', 'c']
['e', 'o', 'r', 'z']
[1, 2, 3]
```

`sorted`는 객체 자체를 정렬만 하고 정렬된 결과를 리턴하지는 않는다.
```python
a = [3, 1, 2]
print(sorted([3, 1, 2]))
print(a)
```
```
[1, 2, 3]
[3, 1, 2]
```

### str
`str`은 문자열 형태로 객체를 변환하여 반환한다.

```python
print(str(3))
```
```
3
```

### sum
`sum`은 입력 데이터의 합을 계산하여 반환한다.

```python
print(sum([1, 2, 3]))
```
```
6
```

### tuple
`tuple`은 반복 가능한 데이터를 튜플로 변환하여 반환한다.
```python
print(tuple("abc"))
print(tuple([1, 2, 3]))
```
```
('a', 'b', 'c')
(1, 2, 3)
```

### type
`type`은 입력값의 자료형을 반환한다.

```python
print(type("abc"))
print(type([1, 2, 3]))
```
```
<class 'str'>
<class 'list'>
```

### zip
`zip`은 동일한 개수로 이루어진 데이터들을 묶어서 반환한다.
```python
print(list(zip([1, 2, 3], [4, 5, 6])))
print(list(zip([1, 2, 3], [4, 5, 6], [7, 8, 9])))
print(list(zip("abc", "def")))
```
```
[(1, 4), (2, 5), (3, 6)]
[(1, 4, 7), (2, 5, 8), (3, 6, 9)]
[('a', 'd'), ('b', 'e'), ('c', 'f')]
```

-----
## 💎 References
- [점프 투 파이썬](https://wikidocs.net/32)