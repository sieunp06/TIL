### 목차
- [em과 rem(root em)](#em과-remroot-em)
  - [em](#em)
  - [rem](#rem)
- [vh(vertical height)와 vw(vertical width)](#vhvertical-height와-vwvertical-width)
- [vmin과 vmax](#vmin과-vmax)
- [ch와 ex](#ch와-ex)
  - [ch](#ch)
  - [ex](#ex)
- [reference](#reference)

--- 
## em과 rem(root em)
### em
`em`은 부모 태그의 크기를 상속받아 크기를 지정한다.

### rem
반면 `rem`은 `root em`이다. 

즉, 부모 태그의 크기를 상속받는 것이 아니라 최상위 태그의 크기를 상속받아 크기를 지정한다.

## vh(vertical height)와 vw(vertical width)
`vh`와 `vw`는 뷰포트의 높이와 너비 값에 따른 `%` 값으로 크기를 지정한다.

예를 들어 브라우저 높이값이 900px일 때, 1vh는 9px이 된다.

## vmin과 vmax
`vmin`과 `vmax`는 너비값과 높이값에 따라 최대, 최소값을 지정할 수 있다.

## ch와 ex
### ch
`ch`는 글꼴 단위로, monospace 폰트의 N의 너비값을 부여 받은 단위이다.

즉, `40ch`은 40개의 문자열을 포함하고 있는 크기라는 의미이다.

### ex
`ex` 단위는 현재 폰트의 `x-높이값` 또는 `em`의 절반 값이라고 할 수 있다.

이때, `x-높이값`은 소문자 `x`의 높이값이다.

`ex` 단위는 폰트의 중간 값을 알아내기 위해 자주 사용한다.

---
## reference
- [https://webclub.tistory.com/356](https://webclub.tistory.com/356)