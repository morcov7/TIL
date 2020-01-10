:whale2:[Docker_study/01_simpleweb][https://github.com/morcov7/Docker_study/tree/master/01_simpleweb]



# 1. Node.js

1. 개발
   1. package.json
   2. index.js
2. npm install : 의존성 해결
3. npm start → node index.js

<br>

### package.json

```javascript
{
    "dependencies": {
        "express": "*"
    },
    "scripts": {
        "start": "node index.js"
    }
}
```

- express는 dependency 설치시 함께 설치됨

<br>

### index.js

```javascript
const express = require('express');
const app = express();

app.get('/', (req, res) => {
    res.send('Hi, there');
});

app.listen(8080, () => {
    console.log('Listening on port 8080')
});
```

- 웹서버 띄우고 포트는 8080
- /로 끝나는 주소값을 받으면 

<br>

#### powershell

```powershell
> npm install
	node_modules가 생성됨
> npm start
	@ start C:\Users\HPE\docker\day01\simpleweb
	node index.js

	Listening on port 8080
```

- npm install
  - dependency install (가장 먼저 해야함)
- npm start 
  - 서버 기동

<br>

<br>

# 2. 위의 과정을 Docker로

> 1. 개발
>    1. package.json
>    2. index.js
> 2. npm install : 의존성 해결
> 3. npm start → node index.js

<br>

1. FROM → Node 사용가능한 이미지
2. RUN → NPM INSTALL 실행
3. CMD → NPM START

<br>

## Dockerfile 만들기

- 이름은 Dockerfile 로 (대문자로시작)

#### Dockerfile

```dockerfile
FROM alpine
RUN npm install
CMD ["npm", "start"]
```

- alpine : 5M 작은 리눅스

<br>

#### Build


```powershell
> docker image build

> docker build -t  ddsd0000/simpleweb:latest .
```

<br>

## Dockerfile Build

- 바로 buid**시 오류!**
- 해결

1. NODE 설치

   RUN curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.11/install.sh | bash

2. BASE IMAGE 교체

<br>

#### Dockerfile

```dockerfile
FROM node:alpine
RUN npm install
CMD ["npm", "start"]
```

- node가 포함되어있는 alpine 이미지 설치

<br>

### Build

```powershell
> docker build -t  ddsd0000/simpleweb:latest .
> docker image ls
REPOSITORY           TAG                 IMAGE ID            CREATED             SIZE
ddsd0000/simpleweb   latest              a98df66746ab        46 seconds ago      111MB
```

- `-t` 계정/이름 . (현재디렉토리)

<br>

<br>

## Docker Container 만들기

```powershell
> docker run -d ddsd0000/simpleweb:latest
> docker ps
	// 아무것도없음
> docker ps -as
	// 죽어있음 WHY? -> 로그확인
```

<br>

#### 기본 명령어

- run = create+start
- image ls ( images )
- image pull, push
- image rm ( rmi )
  - docker rmi
- container ls ( ps )
- ps -a 
- container stop
- container rm
  - docker rm

<br>

#### 로그파일 확인

```powershell
> docker logs 9bbc0974e264
```

<br>

<br>

## package 파일이 X라서 오류

#### Dockerfile

```
FROM node:alpine
COPY ./package.json ./package.json
COPY ./index.js ./index.js
RUN npm install
CMD ["npm", "start"]
```

- COPY [소스] [대상]

<br>

#### Build

```powershell
> docker rm 52636f558226
> docker rmi beddc66acf04
	// 기존 none 이미지, 컨테이너 삭제
> docker run -d ddsd0000/simpleweb:latest
> docker ps
> docker stop 3e0b63ceea09
	// port 연결
> docker run -d -p 8080:8080 ddsd0000/simpleweb:latest
```

<br>

## 계정에 이미지 올리기

```powershell
> docker push ddsd0000/simpleweb:latest
```

- /이미지실제 이름 : tag(버전)
- docker hub의 계정으로 이동하면 이미지 등록된걸 볼 수 있음

<br>

## 계정의 이미지 다운

```powershell
> docker pull ddsd0000/simpleweb:latest
> docker images
> docker run -d -p 8080:8080 ddsd0000/simpleweb:latest
```

- `tag` 새롭게 이름 붙이기

<br>

##  Container에 추가 명령어 할당

- `exec` 사용!

```powershell
> docker exec -it
> docker exec -it upbeat_hugle hostname		//호스트네임
> docker exec -it upbeat_hugle hostname -i	//컨테이너의 ip address
> docker exec -it upbeat_hugle sh	//윈도우즈 호스트가 아니라 컨테이너로 이동
	(컨테이너로 로그인됨)
/ # hostname
57bb1ced1c2f
/ # hostname -i
172.17.0.2
```

- `-i` : input / 추가적인 커맨드를 입력모드로
- `-t`  : 가상의 컨테이너 터미널 모드에서  프롬프트 사용
- `exec -it` + `컨테이너 정보` + `명령어`
- `sh` : 쉘스크립트로 사용하겠다

<br>

<br>

# 3. COPY 디렉터리 변경

- 원래는 root 디렉토리에 다 copy되는데 바람직하지 X

- 따라서 /home/node에 copy하도록 변경

  <br>

```dockerfile
FROM node:alpine

WORKDIR /home/node
COPY ./package.json ./package.json
COPY ./index.js ./index.js
RUN npm install
CMD ["npm", "start"]
```

- `WORKDIR` : 현재 디렉토리를 변경해줌
- npm 명령도 문제 없이 실행됨

<br>

```powershell
> docker build -t ddsd0000/simpleweb:latest .
> docker images
> docker run -d -p 8080:8080 ddsd0000/simpleweb:latest
> docker ps
> docker exec -it 9fb37629f261 sh
/home/node # ls
index.js           node_modules       package-lock.json  package.json
/home/node # vi index.js
	--  index.js를 살짝 바꿈 --
/home/node # exit

> docker restart 9fb37629f261
```

- docker restart : ID 변경시키지 않으면서 새로운 변경된 사항 실행 가능

<br>

<br>

# 3. 그외 명령어

### 포트 포워딩

- 컨테이너, 호스트가 쓰는 포트 매핑 필요
- 9000:8080
  - 9000 : 도커 엔진 /도커 호스트 포트
  - 윈도우 호스트 포트 부여 X 랜덤으로 부여됨
  - 8080 : 컨테이너 포트
  - 호스트로 9000으로 호출해야지만 8080컨테이너로 넘어감

<br>

### 이미지 빌드

- 도커파일 가지고 이미지 만들어줌
- 나만의 이미지를 만들고싶으면 도커파일 만든다

<br>

### docker search

- 이미지 검색

- ex) docker search mysql

  <br>

### docker tag

- 이름 새로 붙이기

```powershell
> docker tag ddsd0000/simpleweb:latest ddsd0000/simpleweb:modified
```

- 이미지는 동일 / 태그만 다름

<br>

<br>

## 컨테이너 실행

#### docker run 

- `-d`: 백그라운드 모드
- `-rm` : 컨테이너 stop과 동시에 rm 실행되는 옵션
- `-p`: 호스트와 컨테이너 포트 연결
- `--name` : 컨테이너 이름 설정
- `-it` : 터미널에서 입력모드

<br>

### mysql 다운

```powershell
> docker run -d -p 3306:3306 --name mysql mysql:5.7
> docker run -e MYSQL_ALLOW_EMPTY_PASSWORD=true -d -p 3306:3306 --name mysql mysql:5.7
> docker exec -it mysql bash
```

- `-e` 를 사용해 password 부분 설정
- host이름 = container 이름

<br>

### 기본 명령어

- docker system prune -a
  - 쓰지 않는 모든 것들 다 비움