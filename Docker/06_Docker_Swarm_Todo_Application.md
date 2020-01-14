# :whale:Swarm을 이용한 Todo Application

##  Application 구조

> - frontend : vue
> - backend : go
> - DB : mysql

<br>

1. MYSQL 서비스를 master-slave 구조로 구축
2. MYSQL과 데이터 주고받을 API 구현
3. Nginx를 웹 애플리케이션과 API 사이에서 리버스 프록시 역할을 하도록 설정

<br>

<br>

## DB) MYSQL 서비스 구축

- Master/Slave 이미지 생성
  - 하나의 이미지를 복제해 Master, Slave로 사용
- DB 초기화
- Master-Slave간의 Replication 설정
  - Master에서의 변경사항을 slave에서도 확인 가능

<br>

### 1. Master-Slave 구조

#### mysqld.conf(MySQL 설정파일) 수정

curl http://localhost:5000/v2/catalog

<br>

<br>

# 실습

1. image 생성

   ```
   PS C:\Users\HPE\docker\day03\swarm\todo\tododb> docker build -t localhost:5000/ch04/tododb:latest .
   ```

   <br>

2. overlay 네트워크 만들기

   ```
   > docker exec -it manager sh
   # docker network create --driver=overlay --attachable todoapp
   # docker network ls
   ```

   <br>

3. DB 빌드후 register로 push

   ```
   > docker build -t localhost:5000/ch04/tododb:latest .
   > docker push localhost:5000/ch04/tododb:latest
   ```

   <br>

4. MySQL 컨테이너 확인 및 초기 데이터 저장

   - 도커 컨테이너 내 init-data.sh, tododb/sql 폴더의 SQL 파일 실행

<br>

#### 1) 따로 따로 하기

```powershell
> docker exec -it manager sh
/ # docker stack ls
/ # docker service ls
/ # docker service ps todo_mysql_master
ID                  NAME                  IMAGE                              NODE                DESIRED STATE       CURRENT STATE            ERROR               PORTS
lum633dh0xc9        todo_mysql_master.1   registry:5000/ch04/tododb:latest   b6830e3c5e83        Running             Running 13 minutes ago
** Node ID를 보고 어떤 Worker와 연결되어있는지 확인후 접속

> docker exec -it worker01 sh
/ # docker ps
CONTAINER ID        IMAGE                              COMMAND                  CREATED             STATUS              PORTS                 NAMES
5149dc7370d0        registry:5000/ch04/tododb:latest   "prehook add-server-…"   15 minutes ago      Up 15 minutes       3306/tcp, 33060/tcp   todo_mysql_master.1.lum633dh0xc949idmuuhg5bkr
/ # docker exec -it 5149dc7370d0
```

<br>

#### 2) 한꺼번에 하기

```powershell
 > docker exec -it manager `
>>     docker service ps todo_mysql_master --no-trunc `
>>     --filter "desired-state=running" `
>>     --format "docker exec -it {{.Node}} docker exec -it {{.Name}}.{{.ID}} bash"

// 아래의 것을 복사해서 붙여넣으면 master로 바로 로그인할 수 있음
docker exec -it 80e260eb3d7e docker exec -it todo_mysql_master.1.x6cqr65ieew15ktw6c1ghk30y bash

 > docker exec -it 80e260eb3d7e docker exec -it todo_mysql_master.1.x6cqr65ieew15ktw6c1ghk30y
bash

# init-data.sh
# mysql -uroot -p tododb
# show tables;
+------------------+
| Tables_in_tododb |
+------------------+
| todo             |
+------------------+
1 row in set (0.00 sec)

# select * from todo;
```

<br>

# docker-compose up 이후 해야하는것들

- docker-compose up
- docker ps (registry, manager, worker01, worker02, worker03)
- docker exec -it manager sh
  - docker swarm init (-> join token 생성됨)
- docker exec -it worker01 sh
  - docker swam join
  - (worker02, 03동일)
- docker exec -it manager sh
  - docker node ls
  - docker network create --driver=overlay --attachable todoapp
  - docker stack deploy -c /stack/visualizer.yml visualizer
  - docker stack deploy -c /stack/todo-mysql.yml  visualizer
- docker exec -it worker01 sh (OR worker02, 03)에 가서 master db 접속

```
docker exec -it manager `
    docker service ps todo_mysql_master --no-trunc `
    --filter "desired-state=running" `
    --format "docker exec -it {{.Node}} docker exec -it {{.Name}}.{{.ID}} bash"

docker exec -it manager `
    docker service ps todo_mysql_slave --no-trunc `
    --filter "desired-state=running"`
    --format "docker exec -it {{.Node}} docker exec -it {{.Name}}.{{.ID}} bash"
```

- ex) Worker01

- docker exec -it [MASTER DB Container] bash
  - $ init-data.sh
  - $ mysql -uroot -p tododb
  - select * from todo;

<br>

<br>

## 1. 스웜에서 todoapi 서비스 실행하기

- todoapi 실행하기
  - todo_app이라는 스택을 만들기

```powershell
docker exec -it manager
# docker stack deploy -c /stack/todo-app.yml todo_app
```

<br>

- todo_api 어디 설치되어있는지 확인 후 ip, PORT 확인하기

```powershell
/ # docker stack ls
NAME                SERVICES            ORCHESTRATOR
todo_app            1                   Swarm
todo_mysql          2                   Swarm
visualizer          1                   Swarm
/ # docker stack services todo_app	//서비스 이름 확인 가능!
ID                  NAME                MODE                REPLICAS            IMAGE                               PORTS
xsfjhugc9h5v        todo_app_api        replicated          2/2                 registry:5000/ch04/todoapi:latest

/ # docker service ps todo_app_api	//서비스가 어디서 설치되었는지 확인 가능

PS C:\Users\HPE\docker\day03\swarm> docker exec -it worker01 sh
/ # docker ps
CONTAINER ID        IMAGE                               COMMAND                  CREATED             STATUS              PORTS                 NAMES
82001ff8c2c9        registry:5000/ch04/todoapi:latest   "todoapi"                3 minutes ago       Up 2 minutes                              todo_app_api.1.sfw5ngyhc67nmppwpe4wtdlmq
5149dc7370d0        registry:5000/ch04/tododb:latest    "prehook add-server-…"   28 minutes ago      Up 28 minutes       3306/tcp, 33060/tcp   todo_mysql_master.1.lum633dh0xc949idmuuhg5bkr
/ # docker exec -it 82001ff8c2c9 bash

root@82001ff8c2c9:/# hostname -i
10.0.2.11

```

<br>

- netstat-tools 설치 후 포트 확인

```powershell
root@82001ff8c2c9:/# apt-get update
root@82001ff8c2c9:/# apt-get install -y net-tools

root@82001ff8c2c9:/# netstat -ntpl
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name
tcp        0      0 127.0.0.11:37563        0.0.0.0:*               LISTEN      -
tcp6       0      0 :::8080                 :::*                    LISTEN      1/todoapi
```

<br>

<br>

## 2. 핸들러 구현하기

C:\Users\HPE\docker\day03\swarm\todo\todoapi\handler.go

### servGET

- **GET**은 요청 파라미터 status를 받아 todo 테이블에서 **SELECT 쿼리**를 실행하고 조건이 일치하는 JSON 포맷으로 된 레코드의 배열 응답

```powershell
root@82001ff8c2c9:/# echo "current: todo_app_api.1"
current: todo_app_api.1

root@82001ff8c2c9:/# curl -XGET http://localhost:8080/todo?status=TODO
[{"id":8,"title":"인그레스 구축하기","content":"외부에서 스웜 클러스터에 접근하게 해주는 인그레스 구축","status":"TODO","created":"2020-01-14T02:03:31Z","updated":"2020-01-14T02:03:31Z"}]
```

<br>

### servPOST

- **POST**는 새로운 TODO를 **추가**한다. JSON으로 내용을 전달

```powershell
root@82001ff8c2c9:/# curl -XPOST -d '{"title":"Test1", "content":"Test Content1"}' http://localhost:8080/todo

** 확인 **
root@82001ff8c2c9:/# curl -XGET http://localhost:8080/todo?status=TODO
[{"id":9,"title":"Test1","content":"Test Content1","status":"TODO","created":"2020-01-14T02:46:31Z","updated":"2020-01-14T02:46:31Z"},{"id":8,"title":"인그레스 구축하기","content":"외부에서 스웜 클러스터에 접근하게 해주는 인그레스 구축","status":"TODO","created":"2020-01-14T02:03:31Z","updated":"2020-01-14T02:03:31Z"}]
```

<br>

### servPUT

- **PUT**는 이미 추가된 TODO를 **수정**한다. id로 레코드를 지정한다.

```powershell
curl -XPUT -d '{"id":10, "title":"Test1 modified", "content":"Test Content1 modifiedddd", "status":"DONE"}' http://localhost:8080/todo

/# curl -XGET http://localhost:8080/todo?status=DONE
[{"id":10,"title":"Test1 modified","content":"Test Content1 modifiedddd","status":"DONE","created":"2020-01-14T02:50:51Z","updated":"2020-01-14T02:51:28Z"},{"id":9,"title":"Test1 modified","content":"Test Content1 modifiedddd","status":"DONE","created":"2020-01-14T02:46:31Z","updated":"2020-01-14T02:49:47Z"},{"id":1,"title":"MySQL 도커 이미지 만들기","content":"MySQL 마스 터와 슬레이브를 환경 변수로 설정할 수 있는 MySQL 이미지 생성","status":"DONE","created":"2020-01-14T02:03:31Z","updated":"2020-01-14T02:03:31Z"},{"id":2,"title":"MySQL 스택 만들기","content":"MySQL 마스터 및 슬레이브 서비스로 구성된 스택을 스웜 클러 root@82001ff8c2c9
```

<br>

<br>

## 3. 서비스 로그 확인

```powershell
> docker exec -it manager sh
/ # docker service logs -f todo_app_api
```

<br>

<br>



# Nginx

Nginx는 가볍고 높은 성능을 가진 웹 서버이다. 트래픽이 많은 웹사이트의 확장성을 위해 설계된 비동기 이벤트 기반 구조의 웹 서버입니다.

> 라우팅은 어떤 네트워크 안에서 통신 데이터를 보낼 경로를 선택하는 과정이다.

<br>

- 로그 생성이 편리
- 캐시 제어
- 애플리케이션 수정 없이 임의로 라우팅 설정이 가능

<br>

### Nginx 이미지 생성 후 push하기

```powershell
PS C:\Users\HPE\docker\day03\swarm\todo\todonginx> docker build -t localhost:5000/ch04/nginx:latest .

> docker push localhost:5000/ch04/nginx

** 주석 해제 **

> docker exec -it manager sh
/ # docker stack deploy -c /stack/todo-app.yml todo_app
```

<br>

```powershell
/ # apt-get update
/ # apt-get install -y net-tools
/ # apt-get install -y curl
```

