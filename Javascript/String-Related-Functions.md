# String 관련 함수들
#### 목차
- [String.length](#stringlength)
- [String.prototype.charAt(number): string](#stringprototypecharatnumber-string)
- [String.prototype.indexOf(searchstring, fromIndex): number](#stringprototypeindexofsearchstring-fromindex-number)
- [String.prototype.lastIndexOf(searchString, fromIndex): number](#stringprototypelastindexofsearchstring-fromindex-number)
- [String.prototype.replace(searchValue, replaceValue): string](#stringprototypereplacesearchvalue-replacevalue-string)
- [String.prototype.split(separator, limit): string[]](#stringprototypesplitseparator-limit-string)
- [string.prototype.substring(start, end): string](#stringprototypesubstringstart-end-string)
- [string.prototype.slice(start, end): string](#stringprototypeslicestart-end-string)
- [string.prototype.repeat(count): string](#stringprototyperepeatcount-string)
- [string.prototype.includes(searchString, position): boolean](#stringprototypeincludessearchstring-position-boolean)

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
lastIndexOf(searchString)   // number
```
```js
lastIndexOf(searchString, fromIndex)    // number
```
`lastIndexOf`는 `searchString`이 마지막으로 발견된 index를 반환한다.

두 번째 매개변수인 `fromIndex`는 검색 시작 위치를 `fromIndex`로 이동하여 역방향으로 검색한다.

만약 문자열에 `searchString`이 존재하지 않으면 `-1`를 반환한다.

```js
const str = 'Hello World';

console.log(str.lastIndexOf('World'));
console.log(str.lastIndexOf('o', 5));
console.log(str.lastIndexOf('x', 8));
```
```
6
4
-1
```

## String.prototype.replace(searchValue, replaceValue): string
```js
replace(searchValue, replaceValue)  // string
```
- searchValue: 문자 or 정규표현식
- replaceValue: 문자 or 콜백함수

`replace`는 `searchValue`로 전달한 문자 혹은 정규표현식을 대상 문자열에서 검색하여 `replaceValue`로 대체한다.

대상 문자열에 `searchValue`가 다수 검색될 경우, 첫 번째만 대체된다.
```js
const str = 'Hello World!';

console.log(str.replace('world', 'Sieun'));
```
```
Hello Sieun!
```

`replaceValue`에 함수를 전달할 경우 다음 예시와 같다.
```js
const camelCase = 'hello World';

console.log(camelCase.replace("World", match => match.toUpperCase()));
```
```
hello WORLD
```

## String.prototype.split(separator, limit): string[]
```js
split(separator)    // string[]
```
```js
split(separtor, limit)  // string[]
```
`split`은 `separator`를 기준으로 분리된 배열을 반환한다.

```js
const str = "How are you doing?";

console.log(str.split(''));
console.log(str.split(""));

console.log(str.split());

console.log(str.split('', 3));
```
```
['How', 'are', 'you', 'doing?']
['H','o','w',' ','a','r','e',' ','y','o','u',' ','d','o','i','n','g','?']

['How are you doing?']

['How', 'are', 'you']
```

## string.prototype.substring(start, end): string
```js
substring(start, end)   // string
```
`substring`은 `start`부터 `end` 바로 앞의 인덱스까지의 문자열을 반환한다.

```js
const str = 'Hello World";

console.log(str.substring(1, 4));
```
```
ell
```

## string.prototype.slice(start, end): string
```js
slice(start, end)   // string
```
`slice`는 `substring`과 동일한 기능을 하지만, 음수 인덱스를 전달할 수 있다.

```js
const str = 'hello world';

console.log(str.slice(-5));
```
```
world
```

## string.prototype.repeat(count): string
```js
repeat(count)   // string
```
`repeat`은 `count` 만큼 대상 문자열을 반복한다.

```js
'abc'.repeat(2);
```
```
abcabc
```

## string.prototype.includes(searchString, position): boolean
```js
includes(searchString);
```
```js
includes(searchString, position);
```

`includes`는 대상 문자열에 `searchString`이 존재하는지 검사하고 이에 대한 결과를 반환한다.

`position`은 검색할 위치를 지정한다.

```js
const str = 'hello world';

str.includes('hello');
str.includes('hello', 0);
str.includes('hello', 2);
```
```
true
true
false
```

---
## 💎 References
- [https://inpa.tistory.com/entry/JS-%F0%9F%93%9A-String-%EB%A9%94%EC%86%8C%EB%93%9C-%E2%9C%8F%EF%B8%8F-%EC%A0%95%EB%A6%AC](https://inpa.tistory.com/entry/JS-%F0%9F%93%9A-String-%EB%A9%94%EC%86%8C%EB%93%9C-%E2%9C%8F%EF%B8%8F-%EC%A0%95%EB%A6%AC)