### 목차
- [ReactNode vs ReactChild vs ReactElement](#reactnode-vs-reactchild-vs-reactelement)
  - [ReactNode](#reactnode)
  - [ReactChild](#reactchild)
  - [ReactElement](#reactelement)

---
## ReactNode vs ReactChild vs ReactElement
`ReactNode`와 `ReactChild` 그리고 `ReactElement`는 React의 컴포넌트 타입이다.

<br>

세 컴포넌트 타입의 범위는 ReactNode > ReactChild > ReactElement 순이다.

### ReactNode
`ReactNode`는 string, number, boolean, null, undefined 등의 원시타입과 ReactElement, ReactFragment, ReactProtal 등의 타입을 모두 허용한다.

즉, 후술할 타입들 중에 가장 범위가 넓다.
```ts
type ReactNode = ReactElement | string | number | ReactFragment | ReactPortal | boolean | null | undefined;
```

### ReactChild
`ReactChild`는 ReactElement와 ReactText, 즉 일부 원시타입을 허용한다.

```ts
type ReactChild = ReactElement | ReactText;
```

이때, `ReactText` 타입은 다음과 같이 string과 number 타입을 포함한다.

```ts
type ReactText = string | number;
```

### ReactElement
`ReactElement`는 `createElement`의 리턴타입이다.

```ts
function createElement<P extends {}> (
    type: FunctionComponent<P> | ComponentClass<P> | string,
    props?: Attributes & P | null,
    ...children: ReactNode[]): ReactElement<P>;
```

<br>

`ReactElement`는 `ReactNode`와 다르게 원시타입을 허용하지 않고, 완성된 `jsx` 요소만을 허용한다.

```ts
interface ReactElement<P = any, T extends string | JSXElementConstructor<any> = string | JSXElementConstructor<any>> {
    type: T;
    props: P;
    key: Key | null;
}
```

---
## references
- [https://merrily-code.tistory.com/209](https://merrily-code.tistory.com/209)
- [https://velog.io/@chojs28/React-Component-Types](https://velog.io/@chojs28/React-Component-Types)