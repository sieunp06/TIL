### 목차
1. [Dockerfile이란?](#dockerfile이란)
2. [Dockerfile 옵션들](#dockerfile-옵션들)
    - [FROM](#from)
    - [LABEL](#label)
    - [ENV](#env)
---
## Dockerfile이란?
`Dockerfile`은 Docker image를 생성하기 위한 지시사항을 작성하는 텍스트 파일이다.

## Dockerfile 옵션들
다음은 `Dockerfile`의 옵션들이다.

- ADD
- ARG
- CMD
- COPY
- ENTRYPOINT
- ENV
- EXPOSE
- FROM
- HEALTHCHECK
- LABEL
- MAINTAINER
- ONBUILD
- RUN
- SHELL
- STOPSIGNAL
- USER
- VOLUME
- WORKDIR

<br>

다음은 `Dockerfile`의 형태다.

```
INSTRUCTION argument
```

각 명령어(`INSTRUCTION`)은 대소문자 상관없이 사용 가능하지만, 대문자로 사용하는 것이 명령어와 내용을 구별하기 쉽다.

### FROM
`FROM`은 새 빌드 명령을 초기화하고 다음 명령에 대한 베이스 이미지를 세팅한다.

그렇기 때문에, `Dockerfile` 맨 처음에 작성되어야 한다.

```dockerfile
FROM [--platform=<platform>] <image> [AS <name>]
```
```dockerfile
FROM [--platform=<platform>] <image>[:<tag>] [AS <name>]
```
```dockerfile
FROM [--platform=<platform>] <image>[@<digest>] [AS <name>]
```