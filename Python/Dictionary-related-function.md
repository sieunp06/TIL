# 딕셔너리 관련 함수들
#### 목차
- [keys](#keys)
- [values](#values)
- [items](#items)
- [clear](#clear)
- [get](#get)
- [in](#in)

-----

### keys
`keys`는 딕셔너리의 key만을 모아 `dict_keys` 객체를 반환한다.

```python
a = {'name': 'pey', 'phone': '010-9999-9999', 'birth': '1118'}
print(a.keys())
```
```
dict_keys(['name', 'phone', 'birth'])
```

`dict_keys` 객체는 `for`문에서 사용할 수 있다.

```python
for k in a.keys():
    print(k)
```
```
name
phone
birth
```

즉, 리스트와 같이 사용할 수 있지만 리스트 관련 함수들은 사용할 수 없다.
> 📌 [[Python] 리스트 관련 함수들](https://github.com/sieunp06/TIL/blob/main/Python/List-related-functions.md)

`dict_keys`를 리스트로 변환하려만 아래와 같이 하면 된다.

```python
print(list(a.keys()))
```
```
['name', 'phone', 'birth']
```

### values
`values`는 `keys`와 유사하게 딕셔너리의 value 값만을 모아 `dict_vales` 객체를 반환한다.

```python
a = {'name': 'pey', 'phone': '010-9999-9999', 'birth': '1118'}
print(a.values())
```
```
dict_values(['pey', '010-9999-9999', '1118'])
```

### items
`items`는 딕셔너리의 key, value 쌍을 튜플로 묶은 값을 `dict_items` 객체로 반환한다.

```python
a = {'name': 'pey', 'phone': '010-9999-9999', 'birth': '1118'}
print(a.items())
```
```
dict_items([('name', 'pey'), ('phone', '010-9999-9999'), ('birth', '1118')])
```

### clear
`clear`는 딕셔너리 안의 모든 값을 삭제한다.

```python
a = {'name': 'pey', 'phone': '010-9999-9999', 'birth': '1118'}
a.clear()
print(a)
```
```
{}
```

### get
`get`은 key 값으로 value 값을 반환받는다.

```python
a = {'name': 'pey', 'phone': '010-9999-9999', 'birth': '1118'}
print(a.get('name'))
print(a.get('phone'))
```
```
pey
010-9999-9999
```

#### 📌 찾는 값이 없을 때
`a.get('name')`은 `a['name']`과 결과가 같다.

둘의 차이점은 찾는 값이 딕셔너리의 key가 아닐 때이다.

```python
a = {'name': 'pey', 'phone': '010-9999-9999', 'birth': '1118'}
print(a.get('email'))
```
```
None
```
`get`을 사용하여 key가 아닌 값을 찾을 때, `None`을 반환하는 것을 알 수 있다.

```python
a = {'name': 'pey', 'phone': '010-9999-9999', 'birth': '1118'}
print(a['email'])
```
```
Traceback (most recent call last):
  File , line 2, in <module>
    print(a['email'])
          ~^^^^^^^^^
KeyError: 'email'
```
하지만 `a[x]`를 사용했을 때, 찾는 값이 없으면 오류가 발생한다.

#### 📌 디폴트 값 설정하기
만약 딕셔너리 안에 찾으려는 key가 없을 때, 디폴트 값을 가져올 수 있도록 하려면 아래와 같이 하면 된다.

```python
a = {'name': 'pey', 'phone': '010-9999-9999', 'birth': '1118'}
print(a.get('email', 'foo'))
```
```
foo
```

### in
`in`은 매개변수로 받는 key가 딕셔너리 안에 존재하는지를 조사한다.

```python
a = {'name': 'pey', 'phone': '010-9999-9999', 'birth': '1118'}
print('name' in a)
print('email' in a)
```
```
True
False
```

-----
## 💎 References
- [점프 투 파이썬](https://wikidocs.net/16#_8)