## SIC/XE MACHINE ARCHITECTUAL COMPONENTS
### 메모리(Memory)
- 8-비트의 바이트들(8-bit bytes)로 구성
- 3개의 연속적인 바이트들로 워드(word, 24 bits)를 형성
- SIC/XE상의 메모리 주소는 바이트 단위의 주소 배정
- SIC/XE 상의 메모리 크기는 총 1메가(megabyte) (2^20) 바이트이다.

<br>

### 레지스터(Register)
- SIC의 레지스터에 추가적으로 4개의 레지스터를 제공함.
- 각 레지스터의 길이는 24 비트이다.

    |Mnomonic|Number|Special Use|
    |--|--|--|
    |B|3|Base register: 어드레싱을 위해 사용됨.|
    |S|4|General working register: 특정 목적의 사용이 정의되어 있지 않음.|
    |T|5|General working register: 특정 목적의 사용이 정의되어 있지 않음.|
    |F|6|Floating-point accumulator: 실수 연산 레지스터(48-비트)|

<br>

### 데이터 포멧(Data Formats)
- 실수(Floating-point): 48-비트 실수 데이터 타입을 표현하는 포맷
    ![SIC 아키텍처 표준 모델](images/SICXE%20Data%20Format.jpg)
    - Sign of the floating-point number
        - 부호를 나타냄
        - S = 0(positive)
        - S = 1(negative)
    - Exponent
        - 0과 2047 사이의 부호 없는 2진수
    - Fraction(f)
        - 0과 1 사이의 값을 갖음
    
    - 절대값: +/- f*2^(e-1024)

<br>

### 명령어 포맷(Instruction Formats)
- 포맷-1 <br>
    <img src="images/SICXE%20format1.jpg" height="80">
- 포맷-2 <br>
    <img src="images/SICXE%20format2.jpg" height="80">
- 포맷-3 <br>
    <img src="images/SICXE%20format3.jpg" height="80">
- 포맷-4 <br>
    <img src="images/SICXE%20format4.jpg" height="80">

<br>

### 주소 지정 모드(Addressing Modes)
- Relative Addressing: SIC 주소지정 모드 + 추가 2개의 주소지정 모드(포맷-3에 적용)

    |Modes|Indication|Target address calculation|
    |--|--|--|
    |Base relative|b=1,p=0|Target Address, TA=(B)+disp (0<=disp<=4095)|
    |Program-counter relative|b=0,p=1|Target Address, TA=(PC)+disp (-2048<=disp<=2047)|

    - Base relative
        - 포맷-3 명령어의 disp 필드는 12-비트의 unsigned integer로 표현
    - Program-counter relative
        - 포맷-3 명령어의 disp 필드는 12-비트의 signed integer로 표현
        - 음의 정수는 2의 보수로 표현