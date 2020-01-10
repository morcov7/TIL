# :cloud:Docker

## 01 도커의 기초

### Docker

- 컨테이너 기반의 오픈소스 가상화 플랫폼
- 백엔드 프로그램 (DB), 메시지 큐, OS 등을 **컨테이너로 추상화** 가능 (Docker의 Image 사용)
- 일반 PC, AWS, Azure 등에서 실행 가능
- Go언어로 만들어짐



### 가상화

- 이미지 : 부팅 가능한 OS 설치 가능한 단일파일 

- 템플릿 : 어떤 Object를 만들기 위한 base

  

![호스트 운영체제형 가상화  container에 대한 이미지 검색결과](https://img1.daumcdn.net/thumb/R720x0.q80/?scode=mtistory2&fname=http%3A%2F%2Fcfile8.uf.tistory.com%2Fimage%2F992A274E5B13A2B5294584)

- **기존** 호스트 운영체제형 가상화
  - OS를 가상화 / 독립적인 OS
  - 용량↑ 느림
  - OS의 기능을 사용하는 목적으로는 적합
- **컨테이너** 기반 가상화
  - 다른 OS 공통적인 부분을 재사용 / 추가
  - Guest OS 필요 X
  - 용량↓ 빠름



### Docker Engine

 = Docker Host = Docker Servers



### Docker Image

- 컨테이너 실행에 필요한 파일, 설정 값 등을 포함
- 상태값 X, Immutable
- 이미지의 실체화 = **Container**
- 1) 직접 만들기 2) 기존의 이미지에 +a
  - Docker Hub site



### 오케스트레이션 도구

- 컨테이너 기반의 가상화를 조립해주고 사용하게끔 해주는 도구
- DockerSwarm / Kubernetes / Apache Mesos



## 02 Docker 사용하기

### Dockerfile

```
FROM ubuntu:16.04

COPY helloworld /usr/loca/bin
RUN chmod +x /usr/local/bin/helloworld

CMD["helloworld"]
```

- 이미지파일 만들기 위한 Dockerfile

- 인스트럭션

  - `FROM` : 도커 이미지의 바탕이 될 베이스 이미지 지정
  - `COPY` : 도커가 동작중인 호스트 머신의 파일(출발지) 등을 컨테이너 안으로 복사 (목적지) 
  - `RUN`  : 컨테이너 안에서 실행할 명령 정의
  - `CMD` : 도커 컨테이너 안에서 실행할 프로세스를 지정 / 하나만와야함 / 제일 마지막에 오는게 일반적
    - CMD ["go", "run", "/echo/main.go"] : 명령을 공백으로 나눈 배열로 나타냄

  


### Docker image Build

```
$ docker image build -t helloworld:latest .
```

- `.` → 현재 디렉토리에 있는 Dockerfile
- `-t` : 이미지명 지정




### Docker image Run

```
$ docker container run helloworld:latest
  docker run helloworld:latest
```

- `run` = **create + start**
  - create : OS 실체화
  - start : container 가동ㄴ
- `docker run` 
-  `docker create`

- `docker start`
- `docker stop`



## Docker를 사용하는 의의

- 변하지 않는 샐행환경으로 멱등성(Idempotency)확보
- 코드를 통한 실행환경 구출, 애플리케이션 구성 : IAC (Infrastructure as Code)
- 실행환경, 애플리케이션 일체화 → 이식성 향상
- 시스템 구성 애플리케이션, 미들웨어 관리 용이



### IAC (Infrastructure as Code), 불변 인프라

- 코드 기반으로 인프라 정의



## Docker Download

- https://hub.docker.com/에서 로그인 후 다운로드

```powershell
> docker login
> docker version
```



## 02 Docker 컨테이너 배포

1. 도커 이미지 다운
2. 다운받은 도커 이미지로 컨테이너 만들기



### Docker Image Download

```powershell
> docker image --help

> docker image ls	//로컬에 존재하는 이미지 목록
> docker image pull gihyodocker/echo:latest	//이미지 다운 from hub
```



### Docker Run (도커 이미지로 컨테이너 실행)

```powershell
> docker run gihyodocker/echo:latest
> docker container ls	//실체화된 컨테이너 목록 보기
> docker stop [Container ID] or [Container Name]
> docker run -p 8080 gihyodocker/echo:latest
CONTAINER ID        IMAGE                     COMMAND                  CREATED             STATUS              PORTS                     NAMES
d612095b486b        gihyodocker/echo:latest   "go run /echo/main.go"   20 seconds ago      Up 18 seconds       0.0.0.0:32768->8080/tcp   great_feynma

> curl http://localhost:32768

> docker run -p 9000:8080 gihyodocker/echo:latest

> docker run -d -p 8080 gihyodocker/echo:latest

> docker run --name myweb1 -d -p 8080 gihyodocker/echo:latest
```

- `curl` : HTTP 요청 보내기 / 파워셸에서 웹브라우저 접근시 사용
- `-p`: 포트번호 부여
  - -p 9000:8080 (호스트가 사용할 포트 : 9000 / 도커에 8080)
- 30000~32768 중에 랜덤 숫자 할당됨
- docker container ls = docker ps : 현재 실행중인 컨테이너
- docker ps `-a `: 중지된 컨테이너까지
- `-d` option : 데몬으로 백그라운드 실행
- `--name` : 컨테이너 이름 설정



### Container 삭제

```powershell
docker ps -a
docker container rm 63cb5efeee62
docker container prune	//stop된 컨테이너 삭제

docker stop $(docker ps-q)
```

- stop이나 rm시 한꺼번에 하기
  - docker stop 31 68 앞 두글자 가능
  - docker stop $(docker ps-q)
  - docker rm $(docker ps -qa)



### 이름 중복 오류

```
The container name "/myweb1" is already in use by container "6986d13a02c488cf063ae5ae6c242eb35567b755f34c0cca2e74935cee32b3da".
```

- 이름 중복시 오류 발생
- 해결
  - docker stop myweb1
  - docker rm myweb1
  - docker stop myweb1 & docker rm myweb1







