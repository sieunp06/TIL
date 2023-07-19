## React 프로젝트의 초기 디렉토리 구조
아래는 `CRA` 후 우리가 볼 수 있는 디렉토리 구조이다.
```
my-app
├─ node_modules
├─ public 
│   ├─ index.html
│   └─ favicon.ico
│   └─ manifest.json
├─ src
│   ├─ App.css
│   ├─ App.js
│   ├─ App.test.js
│   ├─ index.css
│   ├─ index.js
│   └─ lego.svg
├─ .gitignore
├─ package.json
└─ README.md
```
### node_modules
리액트 앱을 처음 만들었을 때 기본적으로 설치되는 패키지들을 모아놓은 폴더.</br>
=> 보통 `.gitignore`을 통해 git에 올리지 않는다.
### public
React 프로젝트의 정적(static) 파일들이 저장된 폴더

- <strong>favicon.ico</br></strong>
    브라우저 탭에 나타나는 아이콘.
- <strong>index.html</br></strong>
    브라우저를 나타내는 html 파일.
- <strong>manifest.json</br></strong>
    PWA(Progressive Web Application)에 필수로 포함돼야 하는 파일.
    ```json
    {
        "short_name": "React App",
        "name": "Create React App Sample",
        "icons": [
            {
                "src": "favicon.ico",
                "sizes": "64x64 32x32 24x24 16x16",
                "type": "image/x-icon"
            },
            {
                "src": "logo192.png",
                "type": "image/png",
                "sizes": "192x192"
            },
            {
                "src": "logo512.png",
                "type": "image/png",
                "sizes": "512x512"
            }
        ],
        "start_url": ".",
        "display": "standalone",
        "theme_color": "#000000",
        "background_color": "#ffffff"
        }
    ```
    - `short_name`: 
    - `name`: 웹앱 설치 배너에서 사용됨.
    - `icons`: 홈 화면에 추가할 때 사용할 이미지.
    - `start_url`: 웹앱 시작시 실행되는 url 주소
    - `display`: 디스플레이 유형(fullscreen, standalone, browse)
    - `theme_color`: 상단 툴바 색상
    - `orientation`: 화면 방향(landscape, portrait) 강제 지정 가능.
### src
```
├─ src
│   ├─ App.css
│   ├─ App.js
│   ├─ App.test.js
│   ├─ index.css
│   ├─ index.js
│   └─ lego.svg
```
- <strong>App.js</br></strong>
    CRA 후 기본적으로 제공하는 예제 파일
- <strong>App.css, App.test.js</br></strong>
    `App.js`의 css 파일과 test 파일
### package.json
설치한 패키지의 정보가 담긴 파일.

-------------

## Reference
- [004_Create React App 기본 구조에 대해 알아보자!](https://sangseophwang.tistory.com/89)
- [폴더 구조](https://create-react-app.dev/docs/folder-structure/)