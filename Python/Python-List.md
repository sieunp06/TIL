# List
#### 목차
1. [리스트(List)](#리스트list)
2. [리스트의 인덱싱과 슬라이싱](#리스트의-인덱싱과-슬라이싱)
    - [리스트의 인덱싱](#리스트의-인덱싱)
    - [리스트의 슬라이싱](#리스트의-슬라이싱)
3. [리스트의 연산](#리스트의-연산)
4. [리스트 관련 함수들](#리스트-관련-함수들)
    - [append](#append)
    - [sort](#sort)
    - [reverse](#reverse)
    - [index](#index)
    - [insert](#insert)
    - [remove](#remove)
    - [pop](#pop)
    - [count](#count)
    - [extend](#extend)

-----
## 리스트(List)
파이썬의 리스트(List)는 다음과 같은 형식을 가진다.
```python
리스트명 = [요소1, 요소2, 요소3, ...]
```
```python
a = [1, 2, 3, 4, 5]
```

### 리스트 생성
#### 📌 여러 가지 리스트의 생성
```python
a = []
b = [1, 2, 3]
c = ['Life', 'is', 'too', 'short']
d = [1, 2, 'Life', 'is']
e = [1, 2, ['Life', 'is']]
```
특이하게 파이썬의 리스트는 `d`와 `e`처럼 숫자와 문자열을 또는 숫자와 리스트를 함께 리스트의 요소값으로 가질 수 있다.

#### 📌 빈 리스트 생성
빈 리스트를 생성하려면 `list()`를 사용하거나 리스트 안의 요소값을 아무 것도 작성하지 않으면 된다.
```python
a = list()
```
```python
a = []
```

## 리스트의 인덱싱과 슬라이싱
### 리스트의 인덱싱
파이썬의 리스트는 인덱싱이 가능하다. 
```python
a = [1, 2, 3]
print(a[0])
print(a[1])
print(a[2])
```
```
1
2
3
```

리스트의 마지막 요소를 인덱싱 하고 싶을 때는 다음 두 가지 방법을 사용할 수 있다.
```python
a = [1, 2, 3]
print(a[2])
print(a[-1])
```
```
3
3
```

#### 📌 중첩 리스트의 인덱싱
```python
a = [1, 2, ['a', 'b', 'c']]
print(a[0])
print(a[1])
print(a[2])
```
```
1
2
['a', 'b', 'c']
```
마지막 요소를 인덱싱했을 때, `['a', 'b', 'c']`가 출력된다. 

해당 마지막 요소 안에 있는 요소들을 인덱싱하려면 다음과 같이 하면 된다.

```python
a = [1, 2, ['a', 'b', 'c']]
print(a[2][0])
print(a[2][1])
print(a[2][2])
```
```
a
b
c
```
`java`에서 2차원 배열을 인덱싱하는 방법과 동일하다.

### 리스트의 슬라이싱
다음 예시는 리스트의 `0`번 인덱스부터 `1`번 인덱스까지 슬라이싱한다.
```python
a = [1, 2, 3, 4, 5]
print(a[0:2])
```
```
[1, 2]
```

`:` 앞이나 뒤를 비우면 다음과 같이 처음이나 끝까지 슬라이싱할 수 있다.
```python
a = [1, 2, 3, 4, 5]
print(a[:2])
print(a[2:])
```
```
[1, 2]
[3, 4, 5]
```

#### 📌 중첩 리스트의 슬라이싱
중첩 리스트 또한 같은 슬라이싱 방법을 사용한다.
```python
a = [1, 2, 3, ['a', 'b', 'c'], 4, 5]
print(a[2:5])
print(a[3][:2])
```
```
[3, ['a', 'b', 'c'], 4]
['a', 'b']
```


## 리스트의 연산
#### 📌 리스트 더하기(+)
```python
a = [1, 2, 3]
b = [4, 5, 6]
print(a + b)
```
```
[1, 2, 3, 4, 5, 6]
```

#### 📌 리스트 반복하기
```python
a = [1, 2, 3]
print(a * 3)
```
```
[1, 2, 3, 1, 2, 3, 1, 2, 3]
```

#### 📌 리스트 길이 구하기
```python
a = [1, 2, 3]
print(len(a))
```
```
3
```

## 리스트 관련 함수들
### append
`append`는 리스트에 요소를 추가한다.

```python
a = [1, 2, 3]
a.append(4)
print(a)
```
```
[1, 2, 3, 4]
```

### sort
`sort`는 리스트를 정렬한다.

```python
a = [1, 4, 3, 2]
a.sort()
print(a)
```
```
[1, 2, 3, 4]
```

숫자 외에도 문자를 알파벳 순으로 정렬할 수 있다.
```python
a = ['a', 'c', 'b']
a.sort()
print(a)
```
```
['a', 'b', 'c']
```

문자열도 문자와 마찬가지로 알파벳 순으로 정렬한다.

### reverse
`reverse`는 리스트를 역순으로 뒤집는다.

```python
a = ['a', 'c', 'b']
a.reverse()
print(a)
```
```
['b', 'c', 'a']
```

### index
`index`는 리스트에 매개변수로 전달한 값이 있으면 해당 값의 인덱스를 반환한다.

```python
a = [1, 2, 3]
print(a.index(3))
print(a.index(1))
```
```
2
0
```

만약 리스트에 값이 없을 경우, 오류가 발생한다.

### insert
`insert`는 두 개의 매개변수 a, b를 받아 a번째 위치에 b를 삽입한다.

```python
a = [1, 2, 3]
a.insert(0, 4)
print(a)
```
```
[4, 1, 2, 3]
```

### remove
`remove`는 매개변수로 하나의 값을 받고, 첫 번째로 나오는 해당 값을 삭제한다.

```python
a = [1, 2, 3, 1, 2, 3]
a.remove(3)
print(a)
```
```
[1, 2, 1, 2, 3]
```

### pop
#### 📌매개변수가 없을 때
```
pop()
```
`pop()`은 리스트의 맨 마지막 값을 리턴하고 해당 값을 삭제한다.
```python
a = [1, 2, 3]
print(a.pop())
print(a)
```
```
3
[1, 2]
```

#### 📌매개변수가 하나일 때
```
pop(x)
```
`pop(x)`는 리스트의 x 번째 요소를 반환하고 해당 요소를 삭제한다.
```python
a = [1, 2, 3]
print(a.pop(1))
print(a)
```
```
2
[1, 3]
```

### count
`count`는 하나의 매개변수를 받고, 리스트 안에서 해당 매개변수가 몇 개 있는지 반환한다.

```python
a = [1, 2, 3, 1]
print(a.count(1))
```
```
2
```

### extend
`extend`는 하나의 리스트를 매개변수로 갖고, 해당 리스트를 더한다.
```python
a = [1, 2, 3]
a.extend([4, 5])
print(a)
```
```
[1, 2, 3, 4, 5]
```

-----
## 💎 References
- [점프 투 파이썬](https://wikidocs.net/14#_11)