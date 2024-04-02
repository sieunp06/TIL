# does not exist on type ‘jsx.intrinsicelements’ 해결

## 🐛ISSUE

![picture](./images/does%20not%20exist%20on%20type%20'jsx.intrinsicelements'/picture1.png)

`Typescript` 로 개발을 하다 위 사진과 같이 `introPage` 부분에서 다음 오류가 발생하였다.

```
Property 'introPage' does not exist on type 'JSX.IntrinsicElements'.
```

위 오류가 발생한 코드는 다음과 같다.

```tsx
import React from 'react';

import { introPage } from './pages';

const App = () => {
    return <introPage />;
};

export default App;
```

이름을 잘못 작성했을 때 발생하는 오류라고 한다.

## ✨SOLUTIONS

`Typescript`에서는 컴포넌트의 첫글자를 대문자로 작성해야 하기 때문에 다음과 같이 변경한다.

`introPage` → `IntroPage`

```tsx
import React from 'react';

import { IntroPage } from './pages';

const App = () => {
    return <IntroPage />;
};

export default App;

```

---

## references

- [https://velog.io/@prkyw1206/does-not-exist-on-type-jsx.intrinsicelements-문제-해결](https://velog.io/@prkyw1206/does-not-exist-on-type-jsx.intrinsicelements-%EB%AC%B8%EC%A0%9C-%ED%95%B4%EA%B2%B0)