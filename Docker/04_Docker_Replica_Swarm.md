# Docker Replica 생성

### 1번 접속

```powershell
	> docker exec -it b8775431edd9 mongo mongodb://mongo1:27017

> myapp:PRIMARY> db.isMaster()
> rs.status() //replica 확인

myapp:PRIMARY> use bookstore; //생성하고 
myapp:PRIMARY> db.books.save({"title":"Docker compose files"});
myapp:PRIMARY> db.books.find()
```

<br>

### 3번 접속

```powershell
E> docker exec -it 2c4d4113ca33  bash
root@2c4d4113ca33:/# mongo mongodb://mongo3:27017

myapp:SECONDARY> rs.slaveOk();
myapp:SECONDARY> show dbs;
```

- PRIMARY에서 db에 값을 추가하면 secondary에서도 바로 확인 가능하다
- PRIMARY가 종료되면 SECONDARY가 PRIMARY로 승격됨

<br>

## Service?

### 1. docker-compose

- **service** : 생성하고자 하는 container의 목록
  - = Container

- 도커 컨테이너 쉽게 만들기 위해 사용

<br>

### 2. docker-swarm

- 멀티호스트 사용 가능
- 네트워크 묶어줌

<br>

### 3. Kubernetes

- Swarm보다 더 많은 기능의 multi-host 지원

<br>

## 과제

> MYSQL replication
>
> - docker-compose 파일 작성
> - MASTER x 1, SLAVE x 1

<br>

<br>

# Docker 네트워크

- docker network  확인
  - docker network ls
  - docker inspect [컨테이너ID]

![관련 이미지](https://joont92.github.io/temp/docker-network.png)

### 1. Bridge network

- 가장 기본 네트워크
- 안에 연결되어있는 네트워크끼리는 서로 통신 가능

```dockerfile
# Docker-compose
networks:
    mysql-networks:
        driver: "bridge"
        
# command-line
docker network create --driver=bridge mongo-networks
```

<br>

### 2.  Host network
  - 윈도우와 같은 ip adress 할당해 사용
  - 단점 : 충돌 확률이 있음
  - 장점 : 포트포워딩 필요 없음

```
docker run -it --name network_host--net host ubuntu:16.0
```

<br>

### 3.  None network
  - 네트워크를 사용하지 않음
  - 로컬만 쓰고 외부 인터넷에선 못들어온다

<br>

### 4.  Container network
  - 다른 컨테이너의 ID를 부여해서 접속

<br>

### 5.  Overlay network
  - 다른 호스트 간의 네트워크 공유
  - swarm, Kubernetes 기능은 Overlay network 사용

<br>

<br>

# Docker Swarm

- 여러 docker host를 묶어주는 컨테이너 오케스트레이션
- **클러스터** : 여러개의 호스트를 하나로 만드는 것

|  이름   | 역할                                                         | 대응하는 명령어 |
| :-----: | :----------------------------------------------------------- | :-------------: |
| Compose | 단일 Host                                                    | docker-compose  |
|  Swarm  | 클러스터 구축 및 관리 (멀티 Host)                            |  docker swarm   |
| Service | 스웜에서 클러스트 안의 **서비스**(컨테이너 하나 이상의 집합)을 관리 | docker service  |
|  Stack  | 스웜에서 여러개의 서비스를 합한 전체 애플리케이션 관리       |  docker stack   |

<br>

- Swarm VS k8s
  - Swarm : 멀티호스트가 별로 없을 때
  - k8s : 멀티호스트가 복잡할때

<br>

<br>

## Docker in Docker (dind)

- Docker Hub에 docker 이미지 사용
  - 19.03.5-dind, 19.03-dind, 19-dind, stable-dind, test-dind, dind

```powershell
> docker pull docker:19.03.5-dind
```

<br>

- 사용할 dind 컨테이너 3종류

1. Register (1)
2. Manager (1)
3. Worker (3)
   - Composer file 사용
   - `docker exec -it [이름 ex)registry] bash` 명령어로 접속

<br>

```powershell
# docker host ip 확인
> docker exec -it manager hostname -i

# work03의 container 확인
> docker exec -it worker03 docker ps
```

docker swarm join --token SWMTKN-1-3ugino9zir1wkwxoq61frkbm0ynqvllhdneonbr2gmy4lxko78-3bhj2yzs1dd1rs2z190l5ditm 172.31.0.3:2377

<br>

# Docker Swarm 설정

## 1. 초기화

```powershell
# Swarm 참여 시작 (호스트가 리더)
> docker swarm init 
========
Swarm initialized: current node (07t5rb4roumomdtzm6n41ow7c) is now a manager.

To add a worker to this swarm, run the following command:

# join을 위해 복사하는 token
    docker swarm join --token SWMTKN-1-57heb03wjizubuxn4awf9unhoa76qftt95oyf6qwchfjuj1fz0-b755nkorrixm0twqt2fnoi19x 172.31.0.3:2377

To add a manager to this swarm, run 'docker swarm join-token manager' and follow the instructions.
========

# Swarm 참여 시작 (매니저가가 리더)
> docker exec -it manager docker swarm init

# worker들 swarm join시킴 (worker1,2,3 동일하게)
> docker exec -it worker01 sh
/  docker swarm join --token SWMTKN-1-57heb03wjizubuxn4awf9unhoa76qftt95oyf6qwchfjuj1fz0-b755nkorrixm0twqt2fnoi1
9x 172.31.0.3:2377
This node joined a swarm as a worker.
/  exit

# Swarm 리스트 확인 (리더인manager에서)
> docker exec -it manager docker node ls

#Swarm 탈출 (node 삭제)
> docker swarm leave
> docker swarm leave --force  //if manager

# Network 확인 (Overlay defaults)
> docker network ls
```

- swarm init하면 참여에 필요한 토큰이 나옴
- 토큰 복사 후 붙여넣기 하면 Join 가능 / 토큰은 항상 바뀜

<br>

### Swarm에서 사용하는 포트

- TCP PORT 2377 : cluster management 통신에 사용
- 7946 : node 간의 통신에 사용
- 4789 : overlay network에 사용

<br>

## 2. 이미지

- 이미지 파일을 Registry에 올리기

```powershell
> docker tag busybox:latest localhost:5000/busybox:latest
> docker push localhost:5000/busybox:latest
```
- 이미지 목록을 browser에서 확인 가능 (Register의 PORT 5000이라 설정됨)

  - http://localhost:5000/v2/_catalog

    - ```
      {"repositories":["busybox"]}
      ```

<br>

### docker registry용 이미지 생성

- docker pull registry:5000/busybox:latest
- docker images

```
> docker exec -it manager sh
/ # docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
/ # docker pull localhost:5000/busybox:latest
Error response from daemon: Get http://localhost:5000/v2/: dial tcp 127.0.0.1:5000: connect: connection refused
/ # docker pull registry:5000/busybox:latest
latest: Pulling from busybox
bdbbaa22dec6: Pull complete
Digest: sha256:edafc0a0fb057813850d1ba44014914ca02d671ae247107ca70c94db686e7de6
Status: Downloaded newer image for registry:5000/busybox:latest
registry:5000/busybox:latest
/ # docker images
REPOSITORY              TAG                 IMAGE ID            CREATED             SIZE
registry:5000/busybox   latest              6d5fcfe5ff17        13 days ago         1.22MB
```






