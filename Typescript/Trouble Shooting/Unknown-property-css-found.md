# Unknown property 'css' found 해결
## 🐛ISSUE
![Picture1](images/Unknown%20property%20css%20found/Unknown-property-css-found.png)

React 프로젝트에 `Emotion`을 적용하는 과정에서 위 오류가 발생하였다.

## ✨SOLUTIONS
오류 메시지를 읽어봤을 때, `eslint` 관련 오류기 때문에 `eslint` 설정을 변경하여 해결한다.

```json
// .eslint.json
{
    "rules": {
        "react/no-unknown-property": ["error", { "ignore": ["css"] }]
    }
}
```

`.eslint.json` 파일의 `rules` 부분에 `"react/no-unknown-property": ["error", { "ignore": ["css"] }]`를 추가하여 해결한다.

<br>

나 같은 경우 위 코드를 추가했을 때, lint 관련 오류가 발생하여 `Quick Fix`로 해결하였다.


---

## references
- [https://velog.io/@muscatcola/React-React-emotion-%EC%98%A4%EB%A5%98-%ED%95%B4%EA%B2%B0-eslint-Unknown-property-css-found-reactno-unknown-property](https://velog.io/@muscatcola/React-React-emotion-%EC%98%A4%EB%A5%98-%ED%95%B4%EA%B2%B0-eslint-Unknown-property-css-found-reactno-unknown-property)