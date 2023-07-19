# react-router-dom 총 정리
## React Router의 기본 구조
```js
<BrowserRouter>
    <Routes>
        <Route />
    </Routes>
</BrowserRouter>
```
- BrowserRouter: History 객체를 생성하고, 초기 위치를 상태로 만들며, URL을 참조한다.
- Routes: Route Config라는 경로 객체 트리를 생성한다. 
- Route: `path`를 통해 URL을 분기시킬 수 있다. 중첩해서 사용할 수 있다.(중첩 라우팅)

## Navigating
### \<Link>
```js
<Link to="path" />
```
### Navigation Function
```js
let navigate = useNavigate();

useEffect(() => {
    setTimeout(() => {
        navigate("/path");
    }, 30000);
}, []);
```
```js
<li onClick={() => navigate("/path")}>
```

## useParams, useSearchParams
URL의 params와 쿼리스트링을 쉽게 받을 수 있도록 하는 hook
### useParams
📃 App.js
```js
<Route path="/register/:id/:name"> element={ <Component /> }/>
```

📃 Component.js
```js
import { useParams } from 'react-router-dom';

function Component() {
    const params = useParams();
    console.log(params);
}
```

=> `/register/1/Charles`로 접속 시 아래 객체가 콘솔에 찍힌다.
```
{
    id: 1,
    name: 'Charles'
}
```
### useSearchParams
📃 App.js
```js
<Route path="/register"> element={ <Component /> }/>
```

📃 Component.js
```js
import { useSearchParams } from 'react-router-dom';

function Component() {
    const [ searchParams, setSearchParams ] = useSearchParams();

    console.log(searchParams.get('id'));
    console.log(searchParams.get('name'));
}
```
=> `/register?id=1&name=Charles`로 접속 시 `1`과 `Charles`가 콘솔창에 찍힌다.
## react-router-dom 사용하기
### 설치
```
npm install react-router-dom
```
### 예제
회원가입 페이지에서 루트 페이지로 이동하는 코드

📃 App.js
```js
function App() {
  return (
    <BrowserRouter>
      <Routes>
        <Route path="/" element = {<LoginPage />} />
        <Route path="/signup"  element = {<SignupPage />} />
      </Routes>
    </BrowserRouter>
  );
}

export default App;
```

📃 SignupPage.js
```js
function SignupPage (props) {
    return (
        <div>
            <section>
                <Title></Title>
                <LoginSignupInsert title="ID"></LoginSignupInsert>
                <LoginSignupInsert title="PW"></LoginSignupInsert>
                <LoginSignupSubmitButton title="Signup"></LoginSignupSubmitButton>
            </section>
            <StyledSection>
                <Link to="/">
                    <LoginSignupTextButton title="You have account already?"></LoginSignupTextButton>
                </Link>
            </StyledSection>
            <StyledSection>
                <SocialLoginSignupButton name="kakao"></SocialLoginSignupButton>
                <SocialLoginSignupButton name="google"></SocialLoginSignupButton>
            </StyledSection>
        </div>
    );
}

```

## References
- [운동개발좋아 블로그 - [ React ] React Router 사용법(v6)](https://charles098.tistory.com/182)