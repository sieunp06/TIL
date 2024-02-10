# Docker Commands
### 목차
1. [✨컨테이너 조작 관련 커맨드](#✨컨테이너-조작-관련-커맨드)
    - [컨테이너를 실행하는 start](#컨테이너를-실행하는-start)
    - [컨테이너를 정지하는 stop](#컨테이너를-정지하는-stop)
    - [도커 이미지로부터 컨테이너를 생성하는 create](#도커-이미지로부터-컨테이너를-생성하는-create)
    - [도커 이미지를 다운받고 컨테이너를 생성해 실행하는 run](#도커-이미지를-다운받고-컨테이너를-생성해-실행하는-run)
    - [정지 상태의 컨테이너를 삭제하는 rm](#정지-상태의-컨테이너를-삭제하는-rm)
    - [실행 중인 컨테이너 속에서 프로그램을 실행하는 exec](#실행-중인-컨테이너-속에서-프로그램을-실행하는-exec)
    - [컨테이너 목록을 출력하는 ls](#컨테이너-목록을-출력하는-ls)
    - [도커 컨테이너와 도커 호스트 간에 파일을 복사하는 cp](#도커-컨테이너와-도커-호스트-간에-파일을-복사하는-cp)
    - [도커 컨테이너를 이미지로 변환하는 commit](#도커-컨테이너를-이미지로-변환하는-commit)
2. [✨이미지 조작 관련 커맨드](#✨이미지-조작-관련-커맨드)
    - [도커 허브 등의 레포지토리에서 이미지를 다운받는 pull](#도커-허브-등의-레포지토리에서-이미지를-다운받는-pull)
    - [도커 이미지를 삭제하는 rm](#도커-이미지를-삭제하는-rm)
    - [다운받은 이미지 목록을 출력하는 ls](#다운받은-이미지-목록을-출력하는-ls)
    - [도커 이미지를 생성하는 build](#도커-이미지를-생성하는-build)
3. [✨볼륨 조작 관련 커맨드](#✨볼륨-조작-관련-커맨드)
    - [볼륨을 생성하는 create](#볼륨을-생성하는-create)
    - [볼륨의 상세 정보를 출력하는 inspect](#볼륨의-상세-정보를-출력하는-inspect)
    - [볼륨의 목록을 출력하는 ls](#볼륨의-목록을-출력하는-ls)
    - [현재 마운트되지 않은 볼륨을 모두 삭제하는 prune](#현재-컨테이너가-접속하지-않은-네트워크를-모두-삭제하는-prune)
    - [지정한 볼륨을 삭제하는 rm](#지정한-볼륨을-삭제하는-rm)
4. [✨네트워크 조작 관련 커맨드](#✨네트워크-조작-관련-커맨드)
    - [컨테이너를 도커 네트워크에 연결하는 connect](#컨테이너를-도커-네트워크에-연결하는-connect)
    - [컨테이너의 도커 네트워크 연결을 해제하는 disconnect](#컨테이너의-도커-네트워크-연결을-해제하는-disconnect)
    - [도커 네트워크를 생성하는 create](#도커-네트워크를-생성하는-create)

---

## ✨컨테이너 조작 관련 커맨드
- 컨테이너 실행 및 종료
- 컨테이너 목록 확인

```
docker container [하위 커맨드] [옵션]
```

### 컨테이너를 실행하는 start
```
docker container start [옵션]
```
```
docker start [옵션]
```
- `-i`

### 컨테이너를 정지하는 stop
```
docker container stop
```
```
docker stop
```

### 도커 이미지로부터 컨테이너를 생성하는 create
```
docker container create [옵션]
```
```
docker create [옵션]
```
- `--name`
- `-e`
- `-p`
- `-v`

### 도커 이미지를 다운받고 컨테이너를 생성해 실행하는 run
```
docker container run [옵션]
```
```
docker run [옵션]
```
- `-name`
- `-e`
- `-p`
- `-v`
- `-d`
- `-i`
- `-t`

### 정지 상태의 컨테이너를 삭제하는 rm
```
docker container rm [옵션]
```
```
docker rm [옵션]
```
- `-f`
- `-v`

### 실행 중인 컨테이너 속에서 프로그램을 실행하는 exec
```
docker container exec [옵션]
```
```
docker exec [옵션]
```
- `-i`
- `-t`

### 컨테이너 목록을 출력하는 ls
```
docker container ls [옵션]
```
```
docker ps [옵션]
```
- `-a`

### 도커 컨테이너와 도커 호스트 간에 파일을 복사하는 cp
```
docker container cp [옵션]
```
```
docker cp [옵션]
```

### 도커 컨테이너를 이미지로 변환하는 commit 
```
docker container commit [옵션]
```
```
docker commit [옵션]
```

## ✨이미지 조작 관련 커맨드
- 이미지 다운 및 검색
- 이미지와 관련된 기능 실행

```
docker image [하위 커맨드] [옵션]
```

### 도커 허브 등의 레포지토리에서 이미지를 다운받는 pull
```
docker image pull [옵션]
```
```
docker pull [옵션]
```

### 도커 이미지를 삭제하는 rm
```
docker image rm [옵션]
```
```
docker rmi [옵션]
```

### 다운받은 이미지 목록을 출력하는 ls
```
docker image ls [옵션]
```

### 도커 이미지를 생성하는 build
```
docker image build [옵션]
```
```
docker build [옵션]
```
- `-t`

## ✨볼륨 조작 관련 커맨드
- 볼륨 생성, 목록 확인 및 삭제 등

### 볼륨을 생성하는 create
```
docker volume create [옵션]
```
- `--name`

### 볼륨의 상세 정보를 출력하는 inspect
```
docker volume inspect [옵션]
```

### 볼륨의 목록을 출력하는 ls
```
docker volume ls [옵션]
```
- `-a`

### 현재 마운트되지 않은 볼륨을 모두 삭제
```
docker volume prune [옵션]
```

### 지정한 볼륨을 삭제하는 rm
```
docker volume rm [옵션]
```

## ✨네트워크 조작 관련 커맨드
```
docker network [하위 커맨드] [옵션]
```

### 컨테이너를 도커 네트워크에 연결하는 connect
```
docker network connect [옵션]
```

### 컨테이너의 도커 네트워크 연결을 해제하는 disconnect
```
docker network disconnect [옵션]
```

### 도커 네트워크를 생성하는 create
```
docker network create [옵션]
```

### 도커 네트워크의 상세 정보를 출력하는 inspect
```
docker network inspect [옵션]
```

### 도커 네트워크의 목록을 출력하는 ls
```
docker network ls [옵션]
```

### 현재 컨테이너가 접속하지 않은 네트워크를 모두 삭제하는 prune
```
docker network prune [옵션]
```

### 지정된 네트워크를 삭제하는 rm
```
docker network rm [옵션]
```

## references
- [그림과 실습으로 배우는 도커 & 쿠버네티스](https://product.kyobobook.co.kr/detail/S000001766500)