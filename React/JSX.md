# JSX
- jsx는 실행될 때 js로 변환되어 실행되기 때문에 js도 함께 작성될 수 있음.

## JSX에서 js와 html을 함께 사용하기
```js
import ReactDom from 'react-dom';

ReactDom.render(
    <h1>나만의 맥북 주문하기</h1>,
    document.getElementById('root')
);
```
- 이런 코드가 있을 때 `맥북`을 `product` 변수로 바꿀 수 있음.
```jsx
import ReactDom from 'react-dom';

const product = '맥북';

ReactDom.render(
    <h1>나만의 {product} 주문하기</h1>,
    document.getElementById('root')
);
```
- 중괄호 안에는 변수 뿐만 아니라 js 표현식도 사용할 수 있음.
- if문, for문, 함수 등은 사용할 수 없음.
    - EX) 대문자
        ```jsx
        import ReactDom from 'react-dom';

        const product = 'Macbook';

        ReactDom.render(
            <h1>나만의 {product.toUpperCase()} 주문하기</h1>,
            document.getElementById('root')
        );
        ```
    - 덧셈 연산자
        ```jsx
        import ReactDom from 'react-dom';

        const product = '맥북';
        const model = 'Air';

        ReactDom.render(
            <h1>나만의 {product + model} 주문하기</h1>,
            document.getElementById('root')
        );
        ```
    ### 이벤트 핸들러
    -  jsx는 이벤트 핸들러를 등록할 때 요소의 속성값으로 이벤트 핸들러를 등록함.
        ```jsx
        import ReactDom from 'react-dom';

        const product = '맥북';
        const model = 'Air';

        fuction handleClick() {
            alert('확인');
        }

        ReactDom.render(
            <h1>나만의 {product + model} 주문하기</h1>
            <button onClick={handleClick}>확인</button>,
            document.getElementById('root')
        );
        ```
## JSX 문법
### HTML과 다른 속성명
1. 속성명은 카멜 케이스로 작성한다.
    - 예외) `data-*` 속성은 기존 html 문법을 사용한다.
        ```jsx
        import ReactDOM from 'react-dom';

        ReactDOM.render(
        <div>
            상태 변경: 
            <button className="btn" data-status="대기중">대기중</button>
            <button className="btn" data-status="진행중">진행중</button>
            <button className="btn" data-status="완료">완료</button>
        </div>,
        document.getElementById('root')
        );
        ```
2. js 예약어와 같은 속성명은 사용할 수 없다.
- `for`이나 `class`처럼 js 문법에 해당하는 예약어와 똑같은 이름의 속성명은 사용할 수 없다.
- `htmlFor`이나 `className`으로 작성해줘야함.
- [리액트 문서 - 어트리뷰트의 차이](https://ko.reactjs.org/docs/dom-elements.html#differences-in-attributes)