### 목차
- [docker-compose란?](#docker-compose란)
- [docker-compose를 사용하는 이유](#docker-compose를-사용하는-이유)
- [docker-compose.yml](#docker-composeyml)
  - [YAML 버전 정의](#yaml-버전-정의)
    - [version](#version)
  - [서비스 정의](#서비스-정의)
    - [services](#services)
    - [image](#image)
    - [links](#links)
    - [environment](#environment)
    - [command](#command)
    - [depends\_on](#depends_on)
    - [ports](#ports)
    - [build](#build)
    - [extends](#extends)

---
## docker-compose란?
`docker-compose`는 단일 서버에서 여러 개의 컨테이너를 하나의 서비스로 정의해 컨테이너의 묶음으로 관리할 수 있는 작업 환경을 제공하는 관리 도구이다.

## docker-compose를 사용하는 이유
여러 개의 컨테이너가 하나의 서비스로 동작할 때 `docker-compose`를 사용하지 않는다면, 각각의 컨테이너들을 하나하나 생성해야 한다.

즉, `웹 서버` 컨테이너와 `데이터베이스` 컨테이너가 존재한다면 이 둘을 각각 생성해서 실행해야 한다.

이때, `docker-compose`는 `docker-compose.yml`을 읽어 각각의 컨테이너들을 순차적으로 생성시켜준다.

## docker-compose.yml
### YAML 버전 정의
#### version
`version`을 사용하여 `YAML`의 버전을 정의할 수 있다.

```yaml
version: '3.0'
```

### 서비스 정의
#### services
`services`는 docker-compose로 생성할 컨테이너 옵션을 정의한다.
```yaml
services:
    my_container_1:
        image: ...
    my_container_2:
        image: ...
```
위 예시에서는 `my_container_1` 컨테이너와 `my_container_2` 컨테이너가 하나의 서비스로서 관리된다.

#### image
`image`는 서비스 컨테이너를 생성할 때 쓰일 이미지의 이름을 설정한다.

```yaml
services:
    my_container_1:
        image: sieunp06/composetest:mysql
```

#### links
`links`는 다른 서비스에서 서비스명만으로 접근할 수 있도록 한다.

```yaml
services:
    web:
        links:
            - db
            - db:database
            - redis
```

#### environment
`environment`는 서비스의 컨테이너 내부에서 사용할 환경변수를 지정한다.

이때, 딕셔너리나 배열 형태로 사용할 수 있다.

```yaml
services:
    web:
        environment:
            - MYSQL_ROOT_PASSWORD=mypassword
            - MYSQL_DATABASE_NAME=mydb
```
```yaml
services:
    web:
        environment:
            - MYSQL_ROOT_PASSWORD:mypassword
            - MYSQL_DATABASE_NAME:mydb
```

#### command
`command`는 컨테이너가 실행될 때 수행할 명령어를 설정한다.

```yaml
services:
    web:
        image: sieunp0606/composetest:web
        command: apachetl -DFOREGROUND
```
```yaml
services:
    web:
        image: sieunp0606/composetest:web
        command: [apachetl, -DFOREGROUND]
```

#### depends_on
`depends_on`은 특정 컨테이너에 대한 의존 관계를 나타낸다.

`depends_on`에 작성된 컨테이너가 먼저 생성되고 실행된다.

```yaml
services:
    web:
        image: sieunp0606/composetest:web
        depends_on:
            - mysql
    mysql:
        image: sieunp0606/composetest:mysql
```

`links`도 `depends_on`과 같이 컨테이너 생성 순서를 정의하지만, `depends_on`은 서비스의 이름으로만 접근할 수 있다.

#### ports
`ports`는 서비스의 컨테이너를 개방할 포트를 설정한다.

```yaml
services:
    web:
        image: sieunp0606/composetest:web
            ports:
                - "8080"
                - "8081-8085"
                - "80:80"
```

#### build
`build`는 build 항목에 정의된 Dockerfile에서 이미지를 빌드해 서비스의 컨테이너를 생성하도록 설정한다.

```yaml
services:
    web:
        build: ./composetest
        image: sieunp0606/composetest:web
```

위 예시는 `./composetest` 경로에 저장된 Dockerfile로 이미지를 빌드해 컨테이너를 생성한다.

```yaml
services:
    web:
        build: ./composetest
        context: ./composetest
        dockerfile: myDockerfile
        args:
            HOST_NAME: web
            HOST_CONFIG: self_config
```
또한 `build`에서는 Dockerfile의 이름과 Dockerfile에서 사용할 인자 값을 설정할 수 있다.

만약 위 예시와 같이 `image`를 지정하지 않으면 `[프로젝트 이름]:[서비스 이름]`으로 자동 설정된다.

#### extends
`extends`는 다른 YAML 파일이나 현재 YAML 파일에서 서비스 속성을 상속받도록 설정할 수 있다.

<br>

만약 다음 두 개의 YAML 파일이 있다고 한다면, `extend_compose.yml`의 `extend_web` 서비스의 옵션을 그대로 갖게 된다.
```yaml
version: '3.0'
    services:
        web: 
            extends:
                file: extend_compose.yml
                service: extend_web
```
```yaml
version: '3.0'
    services:
        extend_web:
        image: ubuntu:14.04
        ports:
            - "80:80"
```

이때, `depends_on`, `links`, `volumn_from` 옵션은 의존성을 가지고 있기 때문에 extends로 상속할 수 없다.

---
## references
- [시작하세요! 도커/쿠버네티스](https://product.kyobobook.co.kr/detail/S000001766450)