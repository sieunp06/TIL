# 디렉티브 태그
- JSP 페이지를 어떻게 처리할 것인지를 설정하는 태그
- <%@ ... %> 사용

<br>

|디렉티브 태그|형식|설명|
|---|--|--|
|page|<%@ page ... %>|JSP 페이지에 대한 정보 설정|
|include|<%@ include ... %>|JSP 페이지의 특정 영역에 다른 문서를 포함|
|talib|<%@ taglib ... %>|JSP 페이지에서 사용할 태그 라이브러리 설정|

<br>

## Page 디렉티브 태그
- 현재 JSP 페이지에 대한 정보를 설정하는 태그
- JSP 컨테이너가 JSP 페이지를 실행하는데 필요한 정보 설정
- JSP 페이지의 최상단에 선언하는 것을 권장
```
<%@ page 속성1="값" [속성2="값2" ... ] %>       // <% 과 @ 사이에 공백이 없어야 함.
```

### Page 디렉티브 태그의 속성

|속성|설명|기본 값|
|--|--|--|
|language|현재 JSP 페이지가 사용할 프로그래밍 언어를 설정함|java|
|contentType|현재 JSP 페이지가 생성할 문서의 콘텐츠 유형을 설정함|text/html|
|pageEncoding|현재 JSP 페이지의 문자 인코딩을 설정함|ISO-8859-1|
|import|현재 JSP 페이지가 사용할 자바 클래스를 설정함||
|session|현재 JSP 페이지의 세션 사용 여부를 설정함|true|
|buffer|현재 JSP 페이지의 출력 버퍼 크기를 설정함|8KB|
|autoFlush|출력 버퍼의 동작 제어를 설정함|true|
|isThreadSafe|현재 JSP 페이지의 멀티스레드 허용 여부를 설정함|true|
|info|현재 JSP 페이지에 대한 설명을 설정함||
|errorPage|현재 JSP 페이지에 오류가 발생했을 때 보여줄 오류 페이지를 설정||
|isErrorPage|현재 JSP 페이지가 오류 페이지인지 여부를 설정함|false|
|isELIgnored|현재 JSP 페이지의 표현 언어(EL) 지원 여부를 설정함|false|
|isScriptingEnabled|현재 JSP 페이지의 스크립트 태그 사용 여부를 설정함||

<br>

### language 속성
- JSP 페이지에서 사용할 프로그래밍 언어를 설정하는 데 사용
- default: java
- JSP 컨테이너가 자바 이외의 언어를 지원할 수 있도록 하기 위함.
- 예시
    ```
    <!@ page language="java" %>
    ```

<br>

### contentType 속성
- 현재 JSP 페이지의 컨텐츠 유형을 설정하는 데 사용
- 컨텐츠 유형: text/html, text/xml, text/plain 등
- default: text/html
- 예시
    - text/html 컨텐츠 유형 설정
        ```
        <!@ page contentType="text/html" %>
        ```
    - utf-8 문자열 세트 설정
        ```
        <!@ page contentType="text/html:charset=utf-8" %>
        ```

<br>

### PageEncoding 속성
- 현재 JSP 페이지의 문자 인코딩 유형을 설정하는 데 사용
- default: ISO-8859-1
- 예시
    ```
    <!@ page pageEncoding="ISO-8859-1" %>
    ```

<br>

### import 속성
- 현재 JSP 페이지에서 사용할 자바 클래스를 설정하는 데 사용
- 둘 이상의 자바 클래스를 포함하는 경우, 쉼표로 구분하여 연속으로 여러 개의 자바 클래스 설정 가능
- 예시
    - java.io.* 설정
        ```
        <!@ page import="java.io.*" %>
        ```
    - java.io.*과 java.lang.* 설정
        ```
        <!@ page import="java.io.*, java.lang.*" %>
        ```
        ```
        <!@ page import="java.io.*" %>
        <!@ page import="java.lang.*" %>
        ```