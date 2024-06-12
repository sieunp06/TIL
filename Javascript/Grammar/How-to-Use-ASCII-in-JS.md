## charCodeAt()
`charCodeAt()`은 문자의 아스키 번호를 반환한다.

```js
'A'.charCodeAt();   // 65
```

## codePointAt()
`codePointAt()`은 `charCodeAt()`과 마찬가지로 문자의 아스키 번호를 반환한다.

```js
'A'.codePointAt();  // 65
```

## String.fromCharCode(ASCII Number)
`String.fromCharCode(ASCII Number)`은 아스키 번호의 문자를 반환한다.

```js
String.fromCharCode(65, 66, 67);    // ABC
```

> 🤔 charCodeAt()과 codePointAt()의 차이<br>
> 둘의 차이점은 charCodeAt()은 `UFT-16` codePointAt()은 `Unicode`이다.<br>
> [출처](https://stackoverflow.com/questions/36527642/difference-between-codepointat-and-charcodeat)