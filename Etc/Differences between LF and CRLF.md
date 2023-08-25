# LF와 CRLF의 차이
## LF, CR
타자기를 사용하던 때에 줄 바꿈을 하던 방식들.

- CR(Carrige Return): 현재 커서를 줄 올림 없이 가장 앞으로 옮기는 동작(\r)
- LF(Line Feed): 커서는 그 자리에 둔 상황에서 종이만 한 줄 올려 줄을 바꾸는 동작(\n)
- CRLF(CR + LF): (\r\n)

## OS 별 줄바꿈
### Linux(유닉스 계열)
LF가 기본 값(\n)
### Windows
CRLF가 기본 값(\r\n)

## LF를 사용해야 하는 이유
CRLF와 LF의 바이트코드가 다르기 때문에 형상관리 툴에서 다른 코드로 인식하기 때문에 Commit 할 때 줄바꿈 타입이 다른 경우 변경하지 않은 파일에 대해서도 변경된 것으로 인식하기 때문에 LF로 통일함.

# references
- https://velog.io/@dev_yong/CRLF%EC%99%80-LF%EC%B0%A8%EC%9D%B4%EC%9D%98-%EC%9D%B4%ED%95%B4
