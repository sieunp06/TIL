### 목차

---
## React Hooks
React의 `Hook`은 React 16.8에 새로 추가된 기능이다.

`Hook`을 사용하면 상

## React Hooks 규칙
### 최상위에서만 Hook을 호출해야 한다.


### React 함수 내에서만 Hook을 호출해야 한다.

## React Hooks 종류
React의 `Hook`에는 아래 Hook들이 존재한다.

- useState
- useEffect
- useContext
- useReducer
- useCallback
- useRef
- useImplerativeHandle
- useLayoutEffect
- useDebugValue

또한 사용자가 직접 Hook을 커스텀할 수도 있다.

### useState
`useState` Hook을 통해 가변적인 상태 관리가 가능하도록 한다.

```js
import React, { useState } from 'react';

const Counter = () => {
    const [value, setValue] = useState(0);
    return (
        <div>
            <p> 현재 카운터 값은 <b></b> 입니다.</p>
            <button onClick={() => setValue(value + 1)}>+1</button>
            <button onClick={() => setValue(value - 1)}>-1</button>
        </div>
    )
}
```

위 예시에서는 `useState` Hook을 사용하여 `value` 라는 state를 관리한다.

```js
const [value, setValue] = setState(0);
```

위 코드는 `value`라는 state를 `setValue` 함수를 통해 관리하고, `setState` 메서드의 매개변수를 0으로 줌으로써 해당 state의 초기값을 0으로 설정한다는 의미이다.

#### useState 여러 개 사용하기
```js
import React, { useState } from 'react';

const Info = () => {
    const [name, setName] = useState('');
    const [nickname, setNickName] = useState('');

    const onChangeName = e => {
        setName(e.target.value);
    };

    const onChangeNickName = e => {
        setNickName(e.target.valeu);
    }

    return (
        <div>
            <div>
                <input value={name} onChange={onChangeName} />
                <input value={nickname} onChange={onChangeNickName} />
            </div>
            <div>
                <div>
                    <b>이름: </b> {name}
                </div>
                <div>
                    <b>닉네임: </b> {nickname}
                </div>
            </div>
        </div>
    );
};
```

---
## references
- [https://velog.io/@velopert/react-hooks](https://velog.io/@velopert/react-hooks)