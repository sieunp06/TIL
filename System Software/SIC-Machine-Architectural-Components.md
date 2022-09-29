## SIC MACHINE ARCHITECTUAL COMPONENTS
### 메모리(Memory)
- 8-비트 바이트(8-bit bytes)들로 구성
- 3개의 연속적인 바이트들로 워드(word, 24bits)를 형성
- SIC 상의 메모리 주소는 바이트 단위의 주소 배정
- 워드의 주소는 해당 바이트들 중에서 가장 낮은 주소로 지정
- SIC 상의 메모리 크기는 총 32,768(2^15) 바이트

<br>

### 레지스터(Register)
- 총 5개의 레지스터로 구성되며, 각 레지스터의 길이는 24 비트이다.

    |Mnemonic|Number|Special Use|
    |--|--|--|
    |A|0|Accumulator: 산술명령어를 위해 사용
    |X|1|Index Register: 주소지정을 위해 사용
    |L|2|Linkage Register: 서브루틴의 리턴 주소를 저장하는데 사용
    |PC|8|Program Counter: 다음에 수행될 명령어의 주소가 저장
    |SW|9|Status Word CC(condition code) 등을 포함하는 다양한 정보저장에 사용

<br>

### 데이터 포맷(Data Formats)
- 정수(Integer): 24-비트 2진수
- 문자(Character): 8-비트 ASCII 코드

<br>

### 명령어 포멧(Instruction Formats)
![SIC 아키텍처 표준 모델](images/SIC%20Instruction%20Format.jpg)
- 모든 명령어: 24-비트 포멧

<br>

### 주소지정 모드(Addressing Modes)
- 어드레싱: 목표 주소를 계산함.

    |Mode|Indication|Target Address Calculation|
    |--|--|--|
    |Direct|x = 0|Target Address, TA = address|
    |Indexed|x= 1|Target Address, TA = address + (x)
    
    - (), parentheses: 레지스터나 메모리의 내용(값)
        - EX) (X) : 레지스터 x의 내용

<br>

### 머신언어(명령어 집합: Micro-Instruction Set)
- SIC에서는 일련의 기본적인 머신언어 집합을 제공함.
    - 레지스터 관련 명령어들
    - 정수 관련 산술 명령어들
    - 비교 명령어
    - 조건적 점프 명령어들

<br>

### 입력과 출력
- SIC's input
    - 레지스터 A의 오른쪽 끝(rightmost) 8-비트에 한번에 1-바이트 씩을 입력장치로부터 전달함.
- SIC's output
    - 레지스터 A의 오른쪽 끝(rightmost) 8-비트로부터 한번에 1-바이트씩 출력장치에 전달함.
- Input/Output instructions
    - TD(Test Device), RD(Read Data), WD(Write Data)
    - TD(Test Device)
        - 명시된 입출력 장치가 1-바이트의 데이터를 주거나 받을 준비가 되어 있는가를 검사하는 명령어
        - "<" : 장치가 준비됨
        - "=" : 장치가 준비되지 않음
    - RD(Read Data)
        - TD-명령어 실행결과에 따라 입력 준비가 완성된 상태에서 실행됨.
        - 수신한 데이터의 바이트 수만큼 이런 과정을 반복해야 함.
    - WD(Write Data)
        - TD-명령어의 실행 결과에 따라 출력 준비가 완성된 상태에서 실행됨.
        - 송신할 데이터의 바이트 수만큼 이런 과정을 반복해야 함.