# String 관련 함수들
#### 목차
- [String.length](#stringlength)
- [String.prototype.charAt(number): string](#stringprototypecharatnumber-string)
- [String.prototype.indexOf(searchstring, fromIndex): number](#stringprototypeindexofsearchstring-fromindex-number)
- [String.prototype.lastIndexOf(searchString, fromIndex): number](#stringprototypelastindexofsearchstring-fromindex-number)

---
## String.length
`length`는 문자열의 문자 개수를 반환한다.
```js
const str1 = 'Hello';
console.log(str1.length);
```
```
5
```

## String.prototype.charAt(number): string
```js
charAt(number)   // string
```
`charAt`은 매개변수로 전달한 index 값에 위치하는 문자를 반환한다.

만약 전달받은 매개변수의 값이 문자열의 길이보다 크면 빈 문자열을 반환한다.
```js
const str = 'Hello';

console.log(str.charAt(0));
console.log(str.charAt(1));
console.log(str.charAt(2));
console.log(str.charAt(3));
console.log(str.charAt(4));

console.log(str.charAt(5));
```
```
H
e
l
l
o
```

## String.prototype.indexOf(searchstring, fromIndex): number
```js
indexOf(searchString)   // number
```
```js
indexOf(searchString, fromIndex)    // number
```
`indexOf`는 문자열에 `searchString`이 처음으로 발견된 index를 반환한다.

두 번째 매개변수 `fromIndex`를 넣음으로써 몇 번째 인덱스부터 검사하는지를 설정할 수 있다.

만약 문자열에 `searchString`이 존재하지 않으면 `-1`를 반환한다.

```js
const str = 'Hello World!';

console.log(str.indexOf('l'));
console.log(str.indexOf('or'));
console.log(str.indexOf('or', 8));
```
```
2
7
-1
```

## String.prototype.lastIndexOf(searchString, fromIndex): number
```js

```

---
## 💎 References
- [https://inpa.tistory.com/entry/JS-%F0%9F%93%9A-String-%EB%A9%94%EC%86%8C%EB%93%9C-%E2%9C%8F%EF%B8%8F-%EC%A0%95%EB%A6%AC](https://inpa.tistory.com/entry/JS-%F0%9F%93%9A-String-%EB%A9%94%EC%86%8C%EB%93%9C-%E2%9C%8F%EF%B8%8F-%EC%A0%95%EB%A6%AC)