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