# 명령어

> 인프런 [개발자를 위한 컴퓨터공학 1: 혼자 공부하는 컴퓨터구조 + 운영체제](https://www.inflearn.com/course/%ED%98%BC%EC%9E%90-%EA%B3%B5%EB%B6%80%ED%95%98%EB%8A%94-%EC%BB%B4%ED%93%A8%ED%84%B0%EA%B5%AC%EC%A1%B0-%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9C) `섹션 3. 명령어` 강의를 듣고 정리한 글입니다.

## 고급언어와 저급언어
- 저급 언어
    - 기계어
        ![Alt text](image.png)
        ![Alt text](image-1.png)
    - 어셈블리어
        기계어와 다르게 소스코드에 직접적으로 명시하면서 개발할 수도 있음.
        ![Alt text](image-2.png)
- 고급 언어
    ![Alt text](image-3.png)
    - 컴파일 언어
        ![Alt text](image-4.png)
        => 오류 발생 시, 소스 코드 전체가 실행되지 않음.
    - 인터프리트 언어
        - 인터프리터에 의해 한 줄씩 실행
        - 소스 코드 전체가 저급 언어로 변환되기까지 기다릴 필요가 없음.</br>
        => 오류 발생 시, 오류 발생 전까지의 코드는 실행됨.

## 명령어의 구조
![Alt text](image-6.png)

### 연산코드
수행할 연산

- 연산 코드의 종류
    - 데이터 전송
        - Move 데이터를 옮겨라.
        - STORE: 메모리에 저장하라.
        - LOAD(FETCH): 메모리에서 CPU로 데이터를 가져와라.
        - PUSH: 스택에 데이터를 저장하라.
        - POP: 스택의 최상단 데이터를 가져와라.
    - 산술/논리 연산
        - ADD/SUBTRACT/MULTIPLY/DIVIDE: 덧셈/뺄셈/곱셈/나눗셈을 수행하라.
        - INCREMENT/DECREMENT: 오퍼랜드에 1을 더하라/오퍼랜드에 1을 빼라
        - AND/OR/NOT: AND/OR/NOT 연산을 수행하라.
        - COMPARE: 두 개의 숫자 또는 True/False 값을 비교하라.
    - 제어 흐름 변경
        - JUMP: 특정 주소로 실행 순서를 옮겨라.
        - CONDITIONAL JUMP: 조건에 부합할 때 특정 주소로 실행 순서를 옮겨라.
        - CALL: 되돌아올 주소를 저장할 채 특정 주소로 실행 순서를 옮겨라.
        - RETURN: CALL을 호출할 때 저장했던 주소로 돌아가라.
    - 입출력 제어
        - READ(INPUT): 특정 입출력 장치로부터 데이터를 읽어라
        - WRITE(OUTPUT): 특정 입출력 장치로 데이터를 써라.
        - START IO: 입출력 장치를 시작하라.
        - TEST IO: 입출력 장치의 상태를 확인하라.

### 오퍼랜드(주소 필드)
연산에 사용될 데이터 혹은 연산에 사용될 데이터가 저장된 위치(주소 필드)</br>
=> 보통 연산에 사용될 데이터보다는 연산에 사용될 데이터가 저장된 위치가 오퍼랜드에 들어가는 것이 보통이기 때문에 주소 필드라고도 부름.
![Alt text](image-7.png)
주소 명령어에서 표현할 수 있는 데이터 크기가 작기 때문에 데이터를 바로 넣기보다는 주소를 넣음.
- 오퍼랜드는 없거나 하나 이상일 수 있다.
![Alt text](image-5.png)

### 명령어 주소 지정 방식(addressing modes)
- 연산에 사용할 데이터가 저장된 위치를 찾는 방법
- 유효 주소를 찾는 방법
- 명령어 주소 지정 방식의 종류
    - 즉시 주소 지정 방식(immediate addressing mode)
    ![Alt text](image-8.png)
        - 연산에 사용될 데이터를 오퍼랜드 필드에 직접 명시
        - 가장 간단한 형태의 주소지정방식
        - 연산에 사용될 데이터의 크기가 제한될 수 있지만, 빠름.
    - 직접 주소 지정 방식(direct addressing mode)
    ![Alt text](image-9.png)
        - 오퍼랜드 필드에 유효 주소를 직접적으로 명시
        - 유효 주소를 표현할 수 있느 크기가 연산 코드만큼 줄어듦 
    - 간접 주소 지정 방식(indirect addressing mode)
    ![Alt text](image-10.png)
        - 오퍼랜드 필드에 유효 주소의 주소를 명시
        - 앞선 방식보다 속도가 느림.
    - 레지스터 주소 지정 방식(register addressing mode)
    ![Alt text](image-11.png)
        - 연산에 사용될 데이터가 저장된 레지스터를 명시
        - 메모리에 접근하는 속도보다 레지스터에 접근하는 것이 빠름.
    - 레지스터 간접 주소 지정 방식(register indirect addressing mode)
    ![Alt text](image-12.png)
        - 연산에 사용될 데이터를 메모리에 저장
        - 그 주소를 저장한 레지스터를 오퍼랜드 필드에 명시