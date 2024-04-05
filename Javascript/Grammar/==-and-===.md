# ==와 ===
### 📌 == 
```js
console.log(1 == 2)   // false
console.log(1 == 1)   // true

console.log("one" == "two")     // false
console.log("one" == "one")     // true
```
`java`의 `==`와 같은 의미를 가진다.

즉, 우리가 아는 평범한 `==`이다.

### 📌 ===
```js
console.log(1 == '1')   // true
console.log(1 === '1')  // false
```
`===`는 같은 값을 가지고 있더라도 데이터형이 다른 경우 `false`를 반환한다.

즉, 같은 값 그리고 같은 데이터형으로 가지고 있어야 `true`라고 판단한다.

### 📌 != 와 !== 
`!=`와 `!==`는 `==`와 `===`의 부정의미이며, 둘의 관계와 정확하게 동일하다.

---
## References
- [생활코딩 open tutorials - 비교](https://opentutorials.org/course/743/4722)