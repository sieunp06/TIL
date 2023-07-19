## 리액트에 CSS를 적용하는 다양한 방법
### 인라인(inline) Styling 적용하기
- 가장 높은 우선순위를 가짐
```js
import React from 'react';

const styles = {
    color: blue;
    background: white;
    border-radius: 10px;
};

function Button() {
    return <Button style={styles}>Inline</Button>;
}
```

### css 파일을 import하기
- js 파일에서 css 파일을 import 해줘야 함.
```css
.btn {
    color: blue;
    background: white;
    border-radius: 10px;
}
```
```js
import React from "react";
import "./Button.css";

function Button() {
  return <button className="btn">External</button>;
}
```

### Encapsulation
![Alt text](image.png)
- Css 파일을 import하는 것은 동일하나 해당 컴포넌트에서 사용할 CSS 파일을 그 디렉토리에서 관리하여 캡슐화함.
- 가독성이 낮다는 단점이 있음.

### CSS Module
- Class Name의 중복을 해결할 수 있음.
- CSS 파일의 확장자를 `.module.css`로 작성하면 됨.
```css
.Title{
	color:red;
}
```
```js
import React from "react";
import "./Button.css";

function Button() {
  return (
    <div>
        <h1 className={styles.Title}>CSS 적용</h1>
    </div>
  )
}
```
- CSS Module이 클래스 이름이 중복되지 않도록 고유한 이름을 지어줌.