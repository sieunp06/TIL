## React Component
React의 Component는 클래스형과 함수형 두 가지로 나뉜다.

### 클래스형과 함수형의 차이
#### 📌 클래스형
- state와 LifeCycle API를 사용할 수 있다.
- 임의 메서드를 정의할 수 있다.

```js
import React, { Component } from 'react';

class App extends Component {
    render () {
        const text = 'Hi';
        return <div className='react'>{text}</div>
    }
}

export default App;
```

클래스형 컴포넌트는 class 키워드와 Component 상속이 필요하다.

또한 render() 메서드가 꼭 필요하다.

#### 📌 함수형
- 16.8 버전 이후 제공되는 Hook을 통해 state와 LifeCycle API를 사용할 수 있다.
- 메모리 자원을 클래스형 컴포넌트보다 덜 사용한다.
- 빌드 결과물의 크기가 클래스형 컴포넌트보다 작다.

```js
import React from 'react';

const App = () => {
    const text = 'Hi';
    return <div className='react'>{text}</div>
}

export default App;
```

함수형을 사용했을 경우, 클래스형을 사용할 때보다 간결하게 코드를 작성할 수 있다.

함수 자체가 render 함수이기 때문에 render 메서드와 Component 상속이 필요 없다.

### state 사용
#### 📌 클래스형
클래스형 컴포넌트에서 state를 사용하는 방법은 `Contructor`를 사용하는 방법과 사용하지 않는 방법이 있다.

<br>

`Constructor`를 사용하는 방법은 아래처럼 `contructor` 안에서 state를 초기 값을 설정할 수 있다.
```js
constructor(props) {
    super(props);

    this.state = {
        name: 'sieun',
        items: [],
    };
}
```

`constructor` 없이도 다음과 같이 state를 사용할 수 있다.
```js
class App extends Component {
    state = {
        name: 'sieun',
        items: [],
    }
}
```

<br>

다음과 같이 `this.setState`를 통해 state의 값을 변경할 수 있다.
```js
onClick = {() => {
    this.setState({ price: price + 100});
}}
```

#### 📌 함수형
함수형 컴포넌트는 `state`를 사용하기 위해 `useState`라는 Hook을 사용해야 한다.
```js
const App = () => {
    const [name, setName] = useState('sieun');

    const onButtonClick = {() => {
        setName('sieun1');
    }};
};
```

위 코드 중 아래 내용이 `useState` Hook을 사용하여 state를 핸들링한다.

```js
const [name, setName] = useState('sieun');
```

`useState`를 사용하면 배열이 반환되는데, 첫 번째 원소는 `state` 두 번째 원소는 `setState` 역할을 하는 함수로 구성되었다.

또한 `useState`의 파라미터는 `state`의 초기값이다.

### props 사용
#### 📌 클래스형
`this.props`를 사용한다.

```js
class App extends Component {
    render () {
        const {name, age} = this.props;
        return (
            <div>
                {name}, {age}
            </div>
        );
    };
};
```

#### 📌 함수형
함수형 컴포넌트는 자체가 render 메서드이기 때문에, 매개변수로 props를 전달받아 사용한다.
```js
const App = () => {
    return (
        <div>
            {name}, {age}
        </div>
    );
};
```

---
## references
- [https://soopiri.tistory.com/23#Class%--Component%--VS%--Functional%--Component](https://soopiri.tistory.com/23#Class%--Component%--VS%--Functional%--Component)