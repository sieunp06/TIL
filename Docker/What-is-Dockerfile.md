### 목차
- [Dockerfile이란?](#dockerfile이란)
- [Dockerfile 명령어](#dockerfile-명령어)
  - [FROM](#from)
  - [ARG](#arg)
  - [RUN](#run)
    - [RUN의 옵션들](#run의-옵션들)
  - [CMD](#cmd)
  - [LABEL](#label)
  - [MAINAINER(deprecated)](#mainainerdeprecated)
  - [EXPOSE](#expose)
  - [ENV](#env)
  - [ADD](#add)
  - [COPY](#copy)
  - [ENTRYPOINT](#entrypoint)
- [references](#references)
---
## Dockerfile이란?
`Dockerfile`은 Docker image를 생성하기 위한 지시사항을 작성하는 텍스트 파일이다.

## Dockerfile 명령어
다음은 `Dockerfile`의 명령어들이다.

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

또한 `FROM`은 하나의 `Dockerfile`에 여러 개의 이미지를 생성하거나 하나의 빌드 단계를 다른 단계의 종속성으로 사용할 때 여러번 사용할 수 있다.

<br>

- (optional) `AS <name>`을 추가함으로써 새 빌드 단계에 이름을 부여할 수 있다.
    ```dockerfile
    FROM [--platform=<platform>] <image> [AS <name>]
    ```
- (optional) `tag`와 `digest`는 생략시 기본적으로 `latest` 태그를 붙인다.
    ```dockerfile
    FROM [--platform=<platform>] <image>[:<tag>] [AS <name>]
    ```
    ```dockerfile
    FROM [--platform=<platform>] <image>[@<digest>] [AS <name>]
    ```
- (optional) `--platform` 플래그는 멀티 플랫폼 이미지를 참조할 경우, 이미지의 플랫폼을 지정하는 데에 사용한다.

### ARG
`ARG`는 `FROM` 앞에서 변수를 선언할 수 있도록 한다.

```dockerfile
ARG CODE_VERSION=latest
FROM base:${CODE_VERSION}
```

`FROM` 전에 선언된 `ARG`는 빌드 단계 바깥에 있기 때문에, `FROM` 사용 이후에는 어떤 명령어에도 사용할 수 없다.

만약 빌드 단계 안에 있는 `ARG` 값에 `FROM` 이전에 사용된 `ARG` 값을 사용하려면 다음과 같이 하면 된다.

```dockerfile
ARG VERSION=latest
FROM busybox:$VERSION
ARG VERSION
RUN echo $VERSION > image_version
```

### RUN
`RUN`은 현재 이미지의 위에 새 레이어를 만들기 위한 명령들을 실행한다.

> 📌 [Docker의 레이어]()

```dockerfile
# Shell form:
RUN [OPTIONS] <command> ...
```
```dockerfile
# Exec form:
RUN [OPTIONS] [ "<command>", ... ]
```

`Shell form`이 가장 흔하게 사용된다.

#### RUN의 옵션들
```dockerfile
RUN [OPTIONS] <command> ...
```

위 형태에서 `OPTIONS`에 들어갈 수 있는 옵션들은 다음과 같다.
- `--mount`
- `--network`
- `--security`

### CMD
`CMD`는 이미지에서 컨테이너를 실행할 때 실행될 명령어들을 설정한다.

`CMD`는 하나만 사용할 수 있고, 만약 여러 개의 `CMD`를 사용하면 맨 마지막에 작성한 `CMD`만 실행된다.

- 명령어와 인자들을 리스트처럼 작성하는 형태
    ```dockerfile
    CMD ["excutable", "param1", "param2"] 
    ```
- `ENTRYPOINT` 명령어에 인자를 리스트처럼 작성하여 넘기는 형태
    ```dockerfile
    CMD ["param1","param2"]
    ```
- Shell 명령처럼 작성하는 형태
    ```dockerfile
    CMD <command1> <param1> <param2>
    ```

### LABEL
`LABEL`은 이미지에 메타 데이터들을 추가하는 명령어이다.

key-value 형태로 작성되며, 한 이미지에서 여러 개의 `LABEL`이 사용될 수 있다.

```dockerfile
LABEL "com.example.vendor"="ACME Incorporated"
LABEL com.example.label-with-value="foo"
LABEL version="1.0"
LABEL description="This text illustrates \
that label-values can span multiple lines."
```

다음과 같이 한 줄에 여러 `LABEL`을 작성할 수도 있다.

```dockerfile
LABEL multi.label1="value1" multi.label2="value2" multi.label3="value3"
```

또한 한 명령어에 여러 개의 `LABEL`을 작성할 수 있다.

```dockerfile
LABEL multi.label1="value1" \
      multi.label2="value2" \
      other="value3"
```

### MAINAINER(deprecated)
`MAINTAINER`은 이미지 생성자에 대한 정보를 설정한다.

하지만 해당 명령어는 `LABEL`로 충분히 대체될 수 있으며, `LABEL`이 더욱 유연하기 때문에 `MAINTAINER`보다 `LABEL`을 사용하는 것을 권장한다.

```dockerfile
LABEL org.opencontainers.image.authors="SvenDowideit@home.org.au" 
```

### EXPOSE
`EXPOSE`는 컨테이너가 실행 중에 특정 포트에서 수신해야 함을 도커에 알린다.

```dockerfile
EXPOSE <port> [<port>/<protocol>...]
```

```dockerfile
EXPOSE 80/tcp
```

### ENV
`ENV`는 환경변수를 key-value 형태로 설정한다.

```dockerfile
ENV MY_NAME="John Doe"
ENV MY_DOG=Rex\ The\ Dog
ENV MY_CAT=fluffy
```

### ADD
```dockerfile
ADD [OPTIONS] <src> ... <dest>
ADD [OPTIONS] ["<src>", ... "<dest>"]
```

`ADD`는 파일, 디렉토리 또는 외부 파일 url를 `src`에서 복사해 `dest` 경로에 추가한다.

<br>

```dockerfile
ADD hom* /mydir/
```
다음 예시는 빌드 컨텍스트의 루트에서 `hom`으로 시작하는 모든 파일을 추가하는 예시이다.

### COPY
```dockerfile
COPY [OPTIONS] <src> ... <dest>
COPY [OPTIONS] ["<src>", ... "<dest>"]
```

`COPY`는 `ADD`와 동일하게 파일 또는 디렉토리를 `src`에서 복사해 `dest` 경로에 추가한다.

하지만 `ADD`와 다른 것은 `ADD`는 외부 url도 복사할 수 있지만, `COPY`는 외부 url은 복사할 수 없다.

### ENTRYPOINT
`ENTRYPOINT`는 `CMD`와 유사하게 실행파일로 실행할 컨테이너를 구성할 수 있다.

```dockerfile
# Exec form
ENTRYPOINT ["executable", "param1", "param2"]
```
```dockerfile
# Shell form
ENTRYPOINT command param1 param2
```

`CMD`와 다른 점은 컨테이너 실행 시 param 값을 대체할 수 없다는 점이다.

---
## references
- [https://docs.docker.com/reference/dockerfile/](https://docs.docker.com/reference/dockerfile/)