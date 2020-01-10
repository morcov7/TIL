:whale2:[Docker_study/02_vd][https://github.com/morcov7/Docker_study/tree/master/02_vd]



# Docker 명령어

- `docker container stats `
  - 사용 현황 확인하기

- 복습) IMAGE (Template) ----> Container(실체화)

<br>

<br>

## javascript 변경하고 싶을 때

### 1. index.js 변경 후 재빌드

- 글자 하나 바꾸기 위해 **서버를 재시작**해서 굉장히 비효율적
- 윈도우즈 안에 있는 파일을 수정시 무조건 재빌드해야함

```powershell
> code index.js
	//수정
> docker build -t ddsd0000/simpleweb:modified .
	//서버 재시작
> docker stop b5b83b949967
> docker rm b5b83b949967
> docker run -d -p 8080:8080 ddsd0000/simpleweb:modified
	//웹브라우저로 확인
```

- build시 보이는 6가지의 단계는 6가지의 instruction을 수행하는 단계들을 뜻함

<br>

### 2. Container에서 수정

- Container에 추가적 command 전달
  -  `exec` `-it` 
    - `i` : 입력 커맨드, `t` : 가상 터미널
- 이것도 **서버 재시작**해야해서 비효율적

```powershell
> docker ps
> docker exec -it bd0928146bc3 sh

/home/node # vi index.js
// 서버 재시작
```

- `docker ps` = `docker container ls`

<br>

### 3. Volume Mount

- Windows의 디렉토리 == Container 디렉토리
- 컨테이너의 **데이터 퍼시스턴스 기법**

<br>

#### Docker 공유폴더

- *docker run -v [windows]:[container]*
- `-v`옵션 : 디렉토리를 직접 마운트 걸어주는 방법 / 호스트 폴더, 컨테이너 폴더 마운트
- -v /my/datadir:/var/lib/mysql
  - 호스트 PC : Container
  - 호스트 PC안 /my/datadir의 데이터 파일을 Container안 var/lib/mysql와 공유하겠다

```powershell
> docker run -v C:\Users\HPE\docker\day01\simpleweb:/home/node -d -p 8080:8080 ddsd0000/simpleweb:modified
> docker ps
> docker exec -it 94de4df38b8c

/home/node # vi index.js
	// 윈도우에서 index.js 변경시 쉘 내에도 바로 반영됨을 확인 가능

> docker restart 94de4df38b8c
	// 재시작하면 브라우저에서도 반영됨
```

<br>

<br>

## :sparkles:데이터 볼륨에 MySQL 데이터 저장하기

- 컨테이너가 삭제되어도 MYSQL 데이터는 유지된다

(Dockerfile)

```dockerfile
FROM busybox
VOLUME /var/lib/mysql
CMD ["bin/true"]
```

- `VOLUME` : volume mount 사용하기 위한 Instruction
  - 폴더 새로 생성해 볼륨 마운트로 사용하겠다

<br>

```powershell
> docker build -t example/mysql-data:latest .
> docker run -d --name mysql-data example/mysql-data
//이때 셸을 실행하는 것이 전부이기 때문에 컨테이너가 바로 종료됨

<!-- mysql 띄우기 -->
> docker images (mysql image 확인)
> docker run -d --rm --name mysql `
    -e "MYSQL_ALLOW_EMPTY_PASSWORD=yes" `
    -e "MYSQL_DATABASE=volume_test" `
    -e "MYSQL_USER=example" `
    -e "MYSQL_PASSWORD=example" `
    --volumes-from mysql-data `
    mysql:5.7
    
> docker exec -it mysql mysql -uroot -p volume_test
```

- powershell은 \ 대신 ` 사용

- `--volumes-from mysql-data \`
- mysql-data 컨테이너를 볼륨 데이터로 쓴다
  
- docker exec -it mysql sh `+` mysql -uroot -p volume_test

  - mysql 이라는 컨테이너에

  - mysql이라는 command를 전달 / root 유저로 volume_test 데이터베이스로 로그인하겠다

<br>

<br>

## Dockerfile의 성능 개선?

(Dockerfile)

```dockerfile
FROM node:alpine

WORKDIR /home/node
COPY ./package.json ./package.json
COPY ./index.js ./index.js
RUN npm install
CMD ["npm", "start"]
```

- `Cache 기능` 활용 → 순서 변경

```dockerfile
FROM node:alpine

WORKDIR /home/node
COPY ./package.json ./package.json
RUN npm install

COPY ./index.js ./index.js
CMD ["npm", "start"]
```

- 가장 시간이 오래 걸리는 작업 : `FROM`, `RUN`
- npm install : node.js가 dependency 다운받는데 cache 사용하면 속도 빨라짐

<br>

<br>

## Docker Compose

- **yaml 포맷**으로 기술된 설정 파일로 열어 컨테이너의 실행을 한번에 관리
- 도커 애플리케이션을 정의하고 실행하는 도구
- 기존 Dockerfile보다 편리해서 많이 사용됨

<br>

(docker-compose.yml)

```yaml
version: "3"
services:
    mytest1:
        image: example/mytest1:latest
        ports:
        	- 9000:8080
    mytest2:
        image:
        ports:
volumes: 
network:
---------------------- 풀어쓰면,,
services.mytest1.image=
services.mytest.ports=
services.mytest2.image=
```

- yml파일은 **띄어쓰기**가 똑같아야함

- `service`  단위 만 변경 가능
  - service 단위는 container 단위로 생각 가능
- 하나의 파일로 run 가능

<br>

### 1. 기존 Dockerfile

- Dockerfile
- 빌드해서 이미지
- run해서 컨테이너 실행

<br>

### 2. Docker-Compose

- YAML format에 docker-compose 생성
- `docker-compose up`
  - `-d` 옵션 사용 가능
- docker-compose up --build
- `docker-compose down`

<br>

```yaml
version: '3'
services:
    mysql1:
        image: mysql:5.7
        ports:
            - "13306:3306"
        environment:
            - MYSQL_ALLOW_EMPTY_PASSWORD=true
    mysql2:
        image: mysql:5.7
        ports:
            - "23306:3306"
        environment:
            - MYSQL_ALLOW_EMPTY_PASSWORD=true
    mysql-data:
        build: .
```

- 현재 폴더 내의 Dockerfile을 가지고 만들고자 할때는

  - mysql-data:

    build: .

<br>

- 위와 동일

```powershell
> docker run -d -p 3306:3306 -e MYSQL_ALLOW_EMPTY_PASSWORD=true --name my-mysql mysql:5.7
```

<br>

```powershell
> docker-compose up
PS C:\Users\HPE> cd .\docker\day02\vd\
PS C:\Users\HPE\docker\day02\vd> docker-compose down
```

- compose down은 해당 compose파일이 있는 디렉토리로 이동해야함

<br>

```yaml
version: "3"
services:
    simpleweb:
        build: .  //OR image: ddsd0000/simpleweb
        
```

<br>

<br>

## DockerCompose로 MongoDB 설치하기

> 1. mongoDB를 Docker container로 실행
> 2. Dockerfile 작성 (mongodb 설치를 위한 이미지 생성)
>    - Composefile로 만들어보기
> 3. Dockerfile의 Image build
>    - ddsd0000/mymongodb:latest
> 4. Mongodb container 생성 → 실행
> 5. Client에서 mongodb 테스트
>    - $ mongo -h<IP> -p<PORT>
>    - mongo> show dbs;
>    - mongo> use bookstore;
>    - mongo> db.books.save('{"title":"Docker compose sample"}');
>    - mongo> db.books.find();

<br>

### Dockerfile

- mongoDB 실행 명령어
  - `mongod`
- MongoDB pull하고 실행하는 Dockerfile 작성

```dockerfile
FROM mongo
CMD ["mongod"]
```

<br>

### BUILD & RUN

```powershell
> docker build -t ddsd0000/mymongodb:latest .
> docker run -d -p 27017:27017 ddsd0000/mymongodb:latest
```

<br>

### mongoDB 접속

```powershell
> docker exec -it b7b9013d4b9a bash
> mongo

	// mongodb에 접속
> show dbs;
> use bookstore
> db.books.save({'title':'Docker compose sample'});
> db.books.find();
```

<br>

<br>

## MongoDB 3대 설치 (Primary x1, Secondary x2)

> 1. Replica Set
>
>    ex) mongod --replSet myapp --dbpath /폴더지정 --port 27017 --bind_ip_all
>
> 2. dd

<br>

- Dockerfile

```dockerfile
FROM mongo
CMD ["mongod", "--replSet", "myapp"]
```

<br>

- mongod --dbpath / data --replSet myapp

```
rs.initiate(); 		//초기화
rs.add("ip:port")	//노드 추가
```

