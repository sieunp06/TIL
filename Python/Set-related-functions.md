# 집합 관련 함수들
#### 목차
- [add](#add)
- [update](#update)
- [remove](#remove)
-----

### add
`add`는 이미 만들어진 집합에 값을 한 개 추가할 수 있다.

```python
s1 = set([1, 2, 3])
s1.add(4)
print(s1)
```
```
{1, 2, 3, 4}
```

### update
`update`는 이미 만들어진 집합에 값을 여러 개 추가할 때 사용한다.

```python
s1 = set([1, 2, 3])
s1.update([4, 5, 6])
print(s1)
```
```
{1, 2, 3, 4, 5, 6}
```

### remove
`remove`는 특정 값을 제거한다.

```python
s1 = set([1, 2, 3])
s1.remove(2)
print(S1)
```
```
{1, 3}
```

-----
## 💎 References
- [점프 투 파이썬](https://wikidocs.net/1015#_7)