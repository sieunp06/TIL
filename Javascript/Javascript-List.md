# List 배열
### 목차
1. [배열의 생성과 참조](#배열의-생성과-참조)
    - [📌 배열 리터럴을 이용하는 방법](#📌-배열-리터럴을-이용하는-방법)
    - [📌 Array 객체의 생성자를 이용하는 방법](#📌-array-객체의-생성자를-이용하는-방법)
    - [📌 new 연산자를 이용한 Array 객체 생성 방법](#📌-new-연산자를-이용한-array-객체-생성-방법)
2. [배열 관련 메서드들](#배열-관련-메서드들)
    - [✨ push](#✨-push)
    - [✨ pop](#✨-pop)
    - [✨ concat](#✨-concat)
    - [✨ reverse](#✨-reverse)
    - [✨ join](#✨-join)
    - [✨ split](#✨-split)
    - [✨ splice](#✨-splice)
    - [✨ slice](#✨-slice)
    - [✨ find](#✨-find)
    - [✨ filter](#✨-filter)
    - [✨ map](#✨-map)
    - [✨ some](#✨-some)
    - [✨ reduce](#✨-reduce)
    - [✨ sort](#✨-sort)

---
## 배열의 생성과 참조
#### 📌 배열 리터럴을 이용하는 방법
```js
var member = ['egoing', 'sieun'];

console.log(member[0]);
console.log(member[1]);
```
```
egoing
sieun
```

#### 📌 Array 객체의 생성자를 이용하는 방법
```js
var member = Array('egoing', 'sieun');

console.log(member[0]);
console.log(member[1]);
```
```
egoing
sieun
```

#### 📌 new 연산자를 이용한 Array 객체 생성 방법
```js
var member = new Array('egoing', 'sieun');

console.log(member[0]);
console.log(member[1]);
```
```
egoing
sieun
```

## 배열 관련 메서드들
### ✨ push
`push`는 배열의 끝에 n개의 요소를 추가한다.
```js
const numArr = [1, 2, 3, 4, 5];
numArr.push(6, 7, 8);

console.log(numArr);
```
```
[1, 2, 3, 4, 5, 6, 7, 8]
```

### ✨ pop
`pop`은 배열의 마지막 요소를 제거한 후 이를 반환한다.
```js
const numArr = [1, 2, 3, 4, 5];
numArr.pop();       // 5 제거 후 반환
const num = numArr.pop();   // 4 제거 후 반환

console.log(num);
console.log(numArr);
```
```
4
1, 2, 3
```

### ✨ concat
`concat`은 n 개의 배열을 합친다.

이때 새로운 배열을 생성해 반환한다.

```js
const first = [1, 2, 3, 4, 5];
const second = [6, 7, 8];

const result = first.concat(second);

console.log(result);
```
```
[1, 2, 3, 4, 5, 6, 7, 8]
```

### ✨ reverse
`reverse`는 배열이 뒤집어 진다. 즉, 순서가 반대가 된다.

이때 새로운 배열을 생성하는 것이 아닌 원래 배열에 덮어씌운다.

```js
const numArr = [1, 2, 3, 4, 5];
numArr.reverse();

console.log(numArr);
```
```
[5, 4, 3, 2, 1]
```

### ✨ join
`join`은 배열을 문자열로 합쳐 반환한다.

인자로는 배열의 각 요소 사이에 들어갈 `separator`를 넣는다. 

이때 `separator`의 기본 값은 `,`이다.

```js
const strArr = ['hello', 'my', 'name', 'is'];

console.log(strArr.join());
console.log(strArr.join(" "));
```
```
hello,my,name,is
hello my name is
```

### ✨ split
`split`은 문자열을 첫 번째 인자로 입력한 `separator`를 기준으로 자른다.

이때, 두 번째 인자로 `limit`을 입력하여 몇 개까지 자를 것인지를 정할 수 있으며, 입력하지 않으면 처음부터 끝까지 `separator` 대로 자른다.

```js
const stringEx = '010-1111-2222';
const splitedNum = stringEx.split('-');

console.log(splitedNum);
```
```
['010', '1111', '2222']
```

### ✨ splice
`splice`는 배열의 원래 요소를 다른 요소로 대체하거나 다른 요소를 배열에 삽입한다.

```js
splice(start, number, spliceElements)
```
- `start`: 시작 배열 인덱스
- `number`: 지울 요소의 개수
- `spliceElements`: 삽입할 요소(여러 개 가능)

```js
const animal = ['rabbit', 'cow', 'pig', 'dog', 'cat', 'human', 'monkey'];

const animal1 = animal.splice(2, 0, 'fish');
const animal2 = animal.splice(1, 3, 'frog');
const animal3 = animal.splice(0, 0, '1', '2');

console.log(animal1);
console.log(animal2);
console.log(animal3);
```
```
['rabbit', 'cow', 'pig', 'fish', 'dog', 'cat', 'human', 'monkey']
['rabbit', 'frog', 'cat', 'human', 'monkey']
['1', '2', 'rabbit', 'cow', 'pig', 'dog', 'cat', 'human', 'monkey']
```

### ✨ slice
`slice`는 원하는 요소를 새 배열로 추출한다.

이때 원래의 배열은 수정되지 않고 새 배열로 복제되어 반환된다.

```js
slice(startIndex, endIndex)
```
- `startIndex`: 시작 인덱스
- `endIndex`: 마지막 인덱스(선택, 입력하지 않았을 때 배열의 끝까지)

```js
const animal = ['rabbit', 'cow', 'pig', 'dog', 'cat', 'human', 'monkey'];

console.log(animal.slice(3));
console.log(animal.slice(2, 5));
```
```
['dog', 'cat', 'human', 'monkey']
['pig', 'dog', 'cat', 'human']
```

### ✨ find
`find`는 배열 내 특정 조건을 만족하는 값을 반환한다.

이때, 첫 번째로 만족하는 값을 반환한다.

```js
const animal = [
    {
        id: 0,
        diverse: 'cat',
        isIt: true
    },
    {
        id: 1,
        diverse: 'carrot',
        isIt: false
    },
    {
        id: 1,
        diverse: 'dog',
        isIt: true
    }
]

const animalFind = animal.find(animalId => (animal.id === 1));

console.log(animalFind);
```
```
{id: 1, diverse: 'carrot', isIt: false}
```

### ✨ filter
`find`는 조건을 만족한 첫 번째 요소만을 반환하지만, `filter`는 전체 배열 중 조건에 맞는 요소를 모두 반환한다.

```js
const animal = [
    {
        id: 0,
        diverse: 'cat',
        isIt: true
    },
    {
        id: 1,
        diverse: 'carrot',
        isIt: false
    },
    {
        id: 1,
        diverse: 'dog',
        isIt: true
    }
]

const animalFind = animal.filter(animalId => (animal.id === 1));

console.log(animalFind);
```
```
[{id: 1, diverse: 'carrot', isIt: false}, {id: 1, diverse: 'dog', isIt: true}]
```

### ✨ map
`map`은 모든 배열의 요소들을 조건에 맞게 새로운 배열을 만들어 반환한다.

```js
const addArr = [1, 2, 3];

let result = addArr.map((num) => {
    return num + 5;
})

console.log(result);
```
```
[6, 7, 8];
```

### ✨ some
`some`은 콜백 함수에 어떤 조건을 넣고, 배열의 요소가 해당 조건을 만족하는지 불린 값을 반환한다.

하나라도 조건에 해당하면 `true`를 반환한다.

```js
const arr1 = [1, 2, 3, 4, 5];
const arr2 = [1, 2, 3, 4, 11];

console.log(arr1.some(isIt => isIt > 10));
console.log(arr1.some(isIt => isIt > 10));
```
```
false
true
```

### ✨ reduce
`reduce`는 반복해서 배열의 값을 반환하는데, 이 값이 누적된다.
```js
reduce(initialValue, currentValue, currentIndex, array)
```
- `initialValue`: 처음 값(defualt: 1)
- `currentValue`: 현재 값
- `currentIndex`: 현재 인덱스
- `array`: 배열

```js
const arr = [1, 2, 3, 4, 5];

const result = arr.reduce((pre, curr, curl, arr) => (pre + curr), 0);

console.log(result);
```
```
15
```

### ✨ sort
`sort`는 배열의 순서를 정렬한다.

```js
const num = [5, 4, 3, 2, 1];
num.sort();

console.log(num);
```
```
[1, 2, 3, 4, 5]
```

이때, Javascript에서는 아스키코드 순으로 정렬되기 때문에 이를 주의한다.

```js
const numArrStrange = [10, 24, 115, 34, 264];
numArrStrange.sort();

console.log(numArrStrange);
```
```
[10, 115, 24, 264, 34]
```

이때 다음과 같이 작성하면 된다.

```js
var arrNum = [10, 24, 115, 34, 264];
 
let result = arrNum.sort(function(a, b){
	return a - b;
}) ;
 
console.log(result);
```
```
[10, 24, 34, 115, 264]
```

---
## References
- [생활코딩 open tutorials - 배열](https://opentutorials.org/course/743/4736)
- [https://tcpschool.com/javascript/js_array_basic](https://tcpschool.com/javascript/js_array_basic)
- [https://jae04099.tistory.com/entry/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EB%B0%B0%EC%97%B4-%ED%95%A8%EC%88%98-%EC%A0%95%EB%A6%AC](https://jae04099.tistory.com/entry/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EB%B0%B0%EC%97%B4-%ED%95%A8%EC%88%98-%EC%A0%95%EB%A6%AC)