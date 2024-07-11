### 목차
- [Emotion](#emotion)
  - [@Emotion/react](#emotionreact)
    - [css() 함수](#css-함수)
      - [✨ 객체형](#-객체형)
      - [✨ 문자형](#-문자형)
      - [❓ 객체형, 문자형 두 유형 중 어떤 유형을 사용해야 할까?](#-객체형-문자형-두-유형-중-어떤-유형을-사용해야-할까)
    - [Labels](#labels)
    - [가변 스타일링](#가변-스타일링)
    - [composition](#composition)
    - [Nested Selectors](#nested-selectors)
    - [Media Query](#media-query)
    - [Global Styles](#global-styles)
  - [@emotion/styled](#emotionstyled)
    - [Props에 따라 변경하기](#props에-따라-변경하기)
  - [references](#references)

---
# Emotion
`Emotion`은 Styled-components와 거의 비슷한 기능을 제공하는 `CSS-in-JS` 라이브러리이다.

아래 두 패키지를 사용할 수 있다.

- @Emotion/react
- @Emotion/styled

## @Emotion/react
@Emotion/react 를 사용하려면 파일 최상단에 다음 코드를 추가해야 한다.

```ts
/** @jsxImportSource @emotion/react */
```

만약 매번 파일 최상단에 다음 코드를 사용하는 것이 번거롭다면, `tsconfig.json` 파일의 `compilerOptions`에 다음 내용을 추가하면 된다.

```json
"jsxImportSource": "@emotion/react"
```

### css() 함수
`css()` 함수는 CSS 스타일 선언 내용을 인자로 받는다.

객체형, 문자형 두 가지 방법으로 넘길 수 있다.

<br>

#### ✨ 객체형
```ts
/** @jsxImportSource @emotion/react */
import { css } from '@emotion/react';

function MyComponent() {
	return (
		<div
			css={css({
				backgroundColor: "yellow",
			})}
		>
			노랑
		</div>
	);
};
```
위 예시처럼 케밥 스타일을 사용하여야 한다.

#### ✨ 문자형
```ts
/** @jsxImportSource @emotion/react */
import { css } from '@emotion/react';

function MyComponent() {
	return (
		<div
			css={css`
				background-color: yellow;
			`}
		>
			노랑
		</div>
	);
};
```

<br>

#### ❓ 객체형, 문자형 두 유형 중 어떤 유형을 사용해야 할까?
`Emotion` 공식 문서에서는 가급적 객체형을 사용하라고 권장한다.

객체형을 사용하면 문자형과 달리 `css` 호출이 필요하지 않기 때문이다.

### Labels
`emotion`은 `Styled-component`와 유사하게 컴포넌트의 `className`을 임의로 생성한다.

이때, `@emotion/babel-plugin`을 사용하여 커스터마이징이 가능하다.

### 가변 스타일링
`Emotion`은 컴포넌트에 넘어온 `props`에 따라 스타일을 다르게 적용할 수 있다.

```ts
import { css } from '@emotion/react';

const colors = {
	default: "rgb(36, 41, 47)",
	danger: "rgb(207, 34, 46)",
	outline: "rgb(9, 105, 218)",
};

function Button({ children, variant = "default" }) {
  return (
    <button
      css={{
        borderRadius: "6px",
        border: "1px solid rgba(27, 31, 36, 0.15)",
        backgroundColor: "rgb(246, 248, 250)",
        color: colors[variant],
        fontFamily: "-apple-system, BlinkMacSystemFont, sans-serif",
        fontWeight: "600",
        lineHeight: "20px",
        fontSize: "14px",
        padding: "5px 16px",
        textAlign: "center",
        cursor: "pointer",
        appearance: "none",
        userSelect: "none",
      }}
    >
      {children}
    </button>
  );
}
```

위 예시는 `prop`으로 넘어온 `variant` 값에 따라 색을 변경할 수 있도록 하는 예시이다.

만약 `props`을 통해 바꾸고 싶은 스타일이 여러 개라면 다음과 같이 하면 된다.

```ts
const colors = {
  default: "rgb(36, 41, 47)",
  danger: "rgb(207, 34, 46)",
  outline: "rgb(9, 105, 218)",
};

const sizeStyles = {
  sm: {
    fontSize: "12px",
    padding: "3px 12px",
  },
  md: {
    fontSize: "14px",
    padding: "5px 16px",
  },
  lg: {
    fontSize: "16px",
    padding: "9px 20px",
  },
};

function Button({ children, size = "md", variant = "default" }) {
  return (
    <button
      css={{
        borderRadius: "6px",
        border: "1px solid rgba(27, 31, 36, 0.15)",
        backgroundColor: "rgb(246, 248, 250)",
        color: colors[variant],
        fontFamily: "-apple-system, BlinkMacSystemFont, sans-serif",
        fontWeight: "600",
        lineHeight: "20px",
        ...sizeStyles[size],
        textAlign: "center",
        cursor: "pointer",
        appearance: "none",
        userSelect: "none",
      }}
    >
      {children}
    </button>
  );
}
```

`props`를 통해 스타일이 전달될 때 컴포넌트 요소에 이미 적용된 스타일이 있으면, `props`를 통해 전달된 스타일이 우선된다.

```ts
import { css } from '@emotion/react'

const pinkInput = css`
  background-color: pink;
`
const RedPasswordInput = props => (
  <input
    type="password"
    css={css`
      background-color: red;
      display: block;
    `}
    {...props}
  />
)

render(
  <div>
    <RedPasswordInput placeholder="red" />
    <RedPasswordInput placeholder="pink" css={pinkInput} />
  </div>
)
```

### composition
`Composition`은 `css()` 함수에서 전달된 스타일 선언 내용을 다른 스타일 블록에 보관하여 스타일을 함께 구성하는 기능이다.

```ts
import { css } from '@emotion/react';

const base = css`
	color: hotpink;
`;

function MyComponent() {
	return (
		<div
			css={css`
				${base};
				background-color: #eee;
			`}
		>
			hotpink
		</div>
	);
};
```

위 예시에서는 `base`를 통해 선언된 스타일과 컴포넌트에서 `css()` 함수를 통해 직접 선언된 스타일 두 가지가 적용된다.

즉, 다음 두 가지 스타일이 적용된다.

- `color: hotpink`
- `background-color: #eee`

<br>

```ts
import { css } from '@emotion/react';

const danger = css`
	color: red;
`;

const base = css`
	background-color: yellow;
	color: turquoise;
`;

function MyComponent() {
	return (
		<div>
			<div css={base}>turquoise</div>
			<div css={[danger, base]}>
				turquoise
			</div>
			<div css={[base, danger]}>red</div>
		</div>
	);
};
```
`Composition`은 사용 순서대로 스타일이 병합되기 때문에, 위 예시처럼 동일한 color 스타일을 가지고 있을 경우 나중에 적용한 스타일을 우선한다.

### Nested Selectors
`Emotion`은 중첩 선택자를 사용할 수 있다.

```ts
import { css } from '@emotion/react';

const paragraph = css`
	color: turquoise;
	
	a {
		border-bottom: 1px solid currentColor;
		cursor: pointer;
	}
`;

function MyComponent() {
	return (
		<p css={paragraph}>
			Some text. <a>A link with a bottom border.</a>
		</p>
	);
}
```
```ts
import { css } from '@emotion/react';

const paragraph = css`
	color: turquoise;
	
	header & {
		color: green;
	}
`;

function MyComponent() {
	return (
		<div>
	    <header>
	      <p css={paragraph}>This is green since it's inside a header</p>
	    </header>
	    <p css={paragraph}>This is turquoise since it's not inside a header.</p>
	  </div>
	);
}
```

### Media Query
```ts
import { css } from '@emotion/react';

function MyComponent() {
	return (
		<p
			css={css`
				font-size: 30px;
				@media (min-width: 420px) {
					font-size: 50px;
				}
			`}
		>
			Some text!
		</p>
	);
}
```

### Global Styles
`resets`나 `font faces` 등과 같은 전역적으로 적용해야 하는 스타일은 Global을 사용하여 적용하면 된다.
```ts
import { Global, css } from '@emotion/react';

function MyComponent() {
	<div>
		<Global
			styles={css`
				.some-class {
					color: hotpink !important;
				}
			`}
		/>
		<div className="some-class">This is hotpink now!</div>			
	</div>
}
```
Global 아래에 사용된 컴포넌트들은 Global에 적용한 스타일이 적용된다.

## @emotion/styled
@emotion/styled는 Styled-component과 같이 스타일을 붙인 컴포넌트를 만들어 사용할 수 있다.
```ts
import styled from '@emotion/styled';

const Button = styled.button`
	color: turquoise;
`;

function MyComponenet() {
	return (
		<Button>This my button component.</Button>
	)
};
```
​
### Props에 따라 변경하기
```ts
import styled from '@emotion/styled';

const Button = styled.button`
	color: ${props => (props.primary ? 'hotpink' : 'turquoise')};
`;

const Container = styled.div(props => ({
  display: 'flex'
  flexDirection: props.column && 'column'
}));

function MyComponenet() {
	return (
	  <Container column>
            <Button>This is a regular button.</Button>
            <Button primary>This is a primary button.</Button>
	  </Container>
  )
};
```

---
## references
- [https://emotion.sh/docs/introduction](https://emotion.sh/docs/introduction)