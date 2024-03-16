### 목차
1. [CRA](#cra)
2. [CRA의 사용](#cra의-사용)
    - [React 앱 생성](#react-앱-생성)
    - [템플릿 설정](#템플릿-설정)
    - [패키지 매니저 설정](#패키지-매니저-설정)

---
## CRA
`CRA`는 `Create React App`의 약자로, 리액트 앱을 쉽게 설정 및 생성할 수 있는 명령어이다.

`CRA`를 사용하면 프로젝트 개발 초기 세팅에 필요한 여러 라이브러리나 패키지의 설치 및 웹팩, 바벨, EsLint 등의 설정 없이 간편하게 리액트 앱을 시작할 수 있다.

## CRA의 사용
### React 앱 생성
아래 명령어들로 `my-app`이라는 리액트 프로젝트를 손쉽게 생성할 수 있다.

#### 📌 npx
```
npx create-react-app my-app
```

#### 📌 npm
```
npm init react-app my-app
```

#### 📌 Yarn
```
yarn create react-app my-app
```

### 템플릿 설정
새 리액트 앱을 만들 때 `--template [template-name]`을 붙여 리액트 앱의 템플릿을 지정할 수 있다.

```
npx create-react-app my-app --template [template-name]
```

#### 📌 Typescript
```
npx create-react-app my-app --template typescript
```

### 패키지 매니저 설정
템플릿 설정과 동일하게 새 리액트 앱을 만들 때, `npm`과 `Yarn` 중에서 패키지 매니저를 고를 수 있다.

#### 📌 npm
```
npx create-react-app my-app
```

#### 📌 yarn
```
yarn create react-app my-app
```

----
## references
- [https://create-react-app.dev/docs/getting-started](https://create-react-app.dev/docs/getting-started)