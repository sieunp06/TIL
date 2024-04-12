### 목차
- [Conditional Rendering 조건부 렌더링](#conditional-rendering-조건부-렌더링)
  - [if - else 문](#if---else-문)
  - [삼항연산자](#삼항연산자)
  - [논리연산자 \&\&](#논리연산자-)

---
## Conditional Rendering 조건부 렌더링

### if - else 문
`React`에서 조건부 렌더링을 사용하는 가장 쉽고 단순한 방법이다.

<br>

```js
function HelloApple() {
    return <span>Hello, Apple!</span>;
}

function HelloBanana() {
    return <span>Hello, Banana!</span>
}
```

`HelloApple`, `HelloBanana` 두 컴포넌트가 있을 때, prop으로 전달받은 과일 이름에 따라 각각 다른 컴포넌트를 렌더링하려면 다음과 같이 `if-else`문을 구성하면 된다.

```js
function Fruit(props) {
    const currentFruit = props.fruitName;

    if (currentFruit === 'Apple') {
        return <HelloApple />;
    } else {
        return <HelloBanana />;
    }
}
```

### 삼항연산자
아래와 같이 삼항연산자를 사용하여 조건부 렌더링을 할 수 있다.

```js
render () {
    const isLoggedIn = this.state.isLoggedIn;
    return (
        <div>
            The user is <b>{isLoggedIn ? 'currently' : 'not'}</b> logged in.
        </div>
    );
}
```

위 예시는 작은 텍스트 블록을 삼항연산자을 통해 조건부 렌더링을 한 예시이다.

<br>

작은 블록의 경우 삼항연산자를 사용하여 조건부 렌더링을 하였을 때, 조건부 렌더링의 의도를 명확히 파악할 수 있다.

하지만 아래 예시처럼 큰 블럭을 삼항연산자로 조건부 렌더링 하였을 때는 덜 명확하게 보일 수 있다.

```js
render () {
    const isLoggedIn = this.state.isLoggedIn;
    return (
        {isLoggedIn ? (
            <LogoutButton onClick={this.handleLogoutClick} />
        ) : (
            <LoginButton onClick={this.handleLoginClick} />
        )}
    );
}
```

### 논리연산자 && 
논리연산자 `&&`을 사용하여 조건부 렌더링을 할 수 있다.

`&&`은 왼쪽 값이 `참`인 경우, 오른쪽 값이 렌더링 된다.

```js
function Fruit(props) {
    const currentFruit = props.fruitName;

    return (
        {currentFruit === 'Apple' && <HelloApple />}
        {currentFruit === 'Banana' && <HelloBanana />}
    );
}
```

---
## reference
- [https://reactjs-kr.firebaseapp.com/docs/conditional-rendering.html](https://reactjs-kr.firebaseapp.com/docs/conditional-rendering.html)