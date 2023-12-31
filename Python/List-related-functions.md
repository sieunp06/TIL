# 리스트 관련 함수들
#### 목차
- [append](#append)
- [sort](#sort)
- [reverse](#reverse)
- [index](#index)
- [insert](#insert)
- [remove](#remove)
- [pop](#pop)
- [count](#count)
- [extend](#extend)

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