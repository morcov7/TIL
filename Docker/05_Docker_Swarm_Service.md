# :rabbit2:Docker Swarm

- 서비스 : 컨테이너들이 grouping되어있는것
- 스택 : 하나의 Application

 <br>

 <br>

## Registry 이용해 로컬을 허브사이트처럼 이용하기

### Image push

  - registry라는 이름을 사용

  ```powershell
  # docker pull registry:5000/busybox:latest
  ```

 <br>

<br>

## Docker Service

- 어플리케이션을 구성하는 일부 컨테이너를 제어하기 위한 단위
- `docker service` 명령어는 swarm상태(multi host)이어야 실행 가능
- manager에 가서 command 전달

<br>

### 1. docker service create

```powershell
> docker exec -it manager sh

# docker service create --replicas 1 --publish 80:8080 --name echo registry:5000/example/echo:latest

# docker service ls
# docker service rm [서비스아이디]
# docker service ps echo	//Service 상세보기


```

- *매니저로 80이 들어오면 8080의 서비스로 응담*
  - *윈도우에서 8080을 사용하면 80으로 응답됨*
- `--replicas` : 복제작업 / 동시 여러 서비스 작동
- `--publish` : 포트 포워딩
- `--name mybusybox` :  이름은 mybusybox로
- `registry:5000/busybox` : registry 내 이미지를 사용해서

<br>

### 2. docker service scale

: docker service replica 숫자 셋팅

```powershell
# docker service scale echo=3
# docker service ls
ID                  NAME                MODE                REPLICAS            IMAGE                               PORTS
nxd43699tawi        echo                replicated          3/3                 registry:5000/example/echo:latest   *:80->8080/tcp
```

- REPLICAS가 3/3로 바뀐걸 확인할 수 있다
- scale out : data 늘리는것
- scale in : data 줄이는것

<br>

<br>

# Docker Stack

### Stack 

- 하나 이상의 서비스를 그룹으로 묶은 단위
- 스택을 사용하는 서비스 그룹은 Overlay Network에 속함

<br>

## 1. Overlay 네트워크 생성

```powershell
/ # docker network create --driver=overlay --attachable ch03
jv9dbk77vrhaqvngzn3cu7a5f
/ # docker network ls
```



## 2. yml파일 stack에 배포

### my-webapi.yml

```yaml
version: "3"
services:
    api:
        image: registry:5000/example/echo:latest
        deploy:
            replicas: 3
            placement:
                constraints: [node.role != manager]
        networks:
            - ch03

    nginx:
        image: gihyodocker/nginx-proxy:latest
        deploy:
            replicas: 3
            placement:
                constraints: [node.role != manager]
        environment:
            BACKEND_HOST: echo_api:8080
        networks:
            - ch03
        depends_on:
            - api

networks:
    ch03:
        external: true
```

- constraints: [node.role != manager]
  - 현재 swarm의 role이 manager가 아닌 곳에 사용

<br>

### stack deploy

- *docker stack deploy -c "file path" [stack name]*

```powershell
docker exec -it manager sh
/ # cd /stack/
/stack # ls
/stack # ls
my-webapi.yml
/stack # docker stack deploy -c /stack/my-webapi.yml echo
```

<br>

### Stack service

```powershell
/stack # docker stack ls
/stack # docker stack services echo	//stack의 내용 중 서비스 확인

/stack # docker service ps echo_api //어디에 서비스 붙는지 확인
```

- 서비스의 이름은 스택+서비스이름으로 보임
  - echo_api / visualizer_app 등

<br>

<br>

# Docker Visualizer

### visualizer.yml

```yaml
version: "3"
services:
    app:
        image: dockersamples/visualizer
        ports:
            - "9000:8080"
        volumes:
            - /var/run/docker.sock:/var/run/docker.sock
        deploy:
            mode: global
            placement:
                constraints: [node.role == manager]
```

```powershell
> docker container exec -it manager docker stack deploy -c /stack/visualizer.yml visualizer

--or--

# docker stack deploy -c /stack/visualizer.yml visualizer
```

- localhost:9000에 접속하면 확인 가능

<br>

<br>

## HAProxy

- Visualizer는 외부 호스트에서 접속 가능
- 외부 호스트에서 요청시 목적 서비스 보내줌!

<br>

### ch03-webapi.yml

```yaml
version: "3"
services:
    nginx:
        image: gihyodocker/nginx-proxy:latest
        deploy:
            replicas: 3
            placement:
                constraints: [node.role != manager]
        environment:
            SERVICE_PORTS: 80
            BACKEND_HOST: echo_api:8080
        networks:
            - ch03
        depends_on:
            - api
    api:
        image: registry:5000/example/echo:latest
        deploy:
            replicas: 3
            placement:
                constraints: [node.role != manager]
        networks:
            - ch03


networks:
    ch03:
        external: true
```

<br>

### ch03-ingres..yml

```yaml
version: "3"

services:
    haproxy:
        image: dockercloud/haproxy
        networks:
            - ch03
        volumes:
            - /var/run/docker.sock:/var/run/docker.sock
        deploy:
            mode: global
            placement:
                constraints:
                    - node.role == manager
        ports:
            - 80:80
            - 1936:1936 # for stats page (basic auth. stats:stats)
networks:
    ch03:
        external: true
```

```
docker stack deploy -c /stack/ch03-webapi.yml echo
docker stack deploy -c /stack/ch03-ingress.yml ingress
```

<br>

## Docker Stack 쌓기

Windows (8000) → Manager(80) → HAProxy(80) → nginx(8080)