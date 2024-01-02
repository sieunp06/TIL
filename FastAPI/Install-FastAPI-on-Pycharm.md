# Pycharm Community에서 FastAPI와 Uvicorn 설치하기
### 1. `python` 새 프로젝트 만들기
이때, 가상환경을 사용하도록 설정 변경 후 `Create` 버튼으로 프로젝트를 만든다.

- Interpreter type: `Custom Environment`
- Type: Virtualenv

### 2. FastAPI 설치하기
터미널 실행 후 다음 명령어를 입력한다.
```
pip install fastapi
```

경우에 따라 아래와 같은 경고 문구가 출력된다.
```
WARNING: You are using pip version 20.1.1; however, version 20.2.2 is available.
You should consider upgrading via the ‘c:\venvs\myapi\scripts\python.exe -m pip
install --upgrade pip’ command.
```

`pip`이 최신 버전이 아니라는 의미이므로 아래 명령어를 다시 입력한다.

```
python -m pip install --upgrade pip
```

### 3. Uvicorn 설치하기
터미널 실행 후 다음 명령어를 입력한다.
```
pip install uvicorn
```