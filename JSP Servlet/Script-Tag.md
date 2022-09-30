# 스크립트 태그
- <% ... %> 형태 사용
- JSP 컨테이너가 자바 코드가 삽입되어 있는 스크립트 태그를 처리하고, 나머지는 HTML 코드나 일반 코드로 간주

|스크립트 태그|형식|설명|
|--|--|--|
|선언문(declaration)|<%! ... %>|자바 변수나 메소드를 정의하는 데 사용|
|스크립틀릿(scriptlet)|<% ... %>|자바 로직 코드를 작성하는 데 사용|
|표현문(expression)|<%= ... %>| 변수, 계산식, 메소드 호출 결과를 문자열 형태로 출력하는 데 사용|

<br>

- 예시
    ```
    <html>
    <head>
    <title>Scripting Tag</title>
    </head>
    <body>
        <h2>Scripting Tag</h2>
        // 선언문 태그 - 자바 변수와 메소드
        <%! 
            int count = 3;
            
            Scripting makeItLower(String data) {
                return data.toLowerCase();
        } 
        
        // 스크립틀릿 태그 - 자바 로직 코드
        %>

        <%
            for (int i = 1; i <= count; i++) {
                out.println("Java Server Pages " + i + ".<br>");
            }
        %>

        // 표현문 태그 - 문자열 출력
        <%=makeItLower("Hello World")%>
    </body>
    </html>
    ```
