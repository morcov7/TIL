# :ocean:Kubernetes

![image](https://subicura.com/assets/article_images/2019-05-19-kubernetes-basic-1/kubernetes-logo.png)

- Docker Container 운영을 자동화하기 위한 컨테이너 오케스트레이션 툴
  - 컨테이너 배포 및 배치
  - Scale in , Scale out
  - Service discovery

<br>

### kubectl

kubectl은 쿠버네티스를 다루기 위한 명령행 도구 (Command Line Interface)이다.

- 다운로드 링크

https://storage.googleapis.com/kubernetes-release/release/v1.17.0/bin/windows/amd64/kubectl.exe

- 따로 설치시
  - C:\Program Files\Docker\Docker\resources\bin 복사

<br>

<br>



## POD

![kubernetes 배포 방식에 대한 이미지 검색결과](https://subicura.com/assets/article_images/2019-05-19-kubernetes-basic-1/workload.png)



- POD는 k8s의 최소 단위를 의미한다

- Container가 모인 집합체 단위이며 하나 이상의 컨테이너로 구성된다
- Container에서는 서로 연결 가능한  중복되지 않는 Post 제공해준다
- 고유의 IP 할당
  - Cluster 내부에서 접속 (외부에서는 X)
- Pod에 문제가 생길시 Pod 삭제 후 자동으로 생성
  - IP 변경됨



- job : 반복적으로 하는 작업 
- cronjob : job 관리
- Deployment : POD 배포
- **Service** : POD를 *외부로 공개하기 위한 네트워크의 정보*

<br>

<br>

## Kubernetes 주요 개념

| 리소스             | 용도                                                         |
| ------------------ | ------------------------------------------------------------ |
| 노드               | 컨테너가 배치되는 서버                                       |
| 네임스페이스       | 쿠버네티스 클러스터 안의 가상 클러스터                       |
| 파드               | 컨테이너 집합중 가장 작은  단위로 컨테이너의 실행 방법을 정의한다 |
| 레플리카세트       | 같은 스펙을 갖는 파드를 여러개  생성하고 관리하는 역할 수행  |
| 디플로이먼트       | 레플리카 세트의 리비젼을  관리한다.                          |
| 서비스             | 파트의 집합에 접근하기 위한  경로를 정의한다.                |
| 인그레스           | 서비스를 쿠버네티스 클러스터  외부로 노출시킨다.             |
| 컨피그맵           | 설정정보를 정의하고 파드에  전달한다.                        |
| 퍼시스트볼륨       | 파드가 사용할 스토리지의 크기  및 종류를 정의                |
| 퍼시스트볼륨클레임 | 퍼시스트 볼륨을 동적으로 확보                                |
| 스토리지클래스     | 퍼시스턴트 볼륨이 혹보하는  스토리지의 종류를정의            |
| 스테이트풀세트     | 같은 스펙으로 모두 동일한  파드를 여러개 생성하고 관리한다.  |
| 잡                 | 상주 실행을 목적으로 하지 않는  파드를 여러개 생성하고 정상적인 종료를 보장 |
| 크론잡             | 크론문법으로 스케줄되는 잡                                   |
| 시크릿             | 인증정보 같은 기밀 데이터를  정의한다.                       |
| 롤                 | 네임스페이스 안에서 조작가능한  쿠버네티스 리소스의 규칙을 정의한다. |
| 롤바인딩           | 쿠버네티스 리소스 사용자와 롤을  연결 짓는다                 |
| 클러스터롤         | 클러스터 전체적으로 조작 가능한  쿠버네티스 리소스의 규칙을 정의한다. |
| 클러스터롤 바인딩  | 쿠버네티스 리소스 사용자와  클러스터롤을 연결짓는다          |
| 서비스계정         | 파드가 쿠버네티스 리소스를  조작할때 사용하는 계정           |



## Kubernetes vs Swarm

### Swarm

- 여러대의 호스트를 묶어 기초적인 컨테이너 오케스트레이션 기능 제공
- 간단한 멀티 컨테이너 구축

<br>

### K8s

- Swarm의 기능 + a

<br>

<br>

## Kubernetes 사용하기

### k8s 정보 확인

```powershell
> kubectl version
> kubectl get all
```

<br>

### K8s dashboard 설치

``` powershell
> kubectl apply -f http://raw.githubusercontent.com/kubernetes/dashboard/v1.8.3/src/deploy/recommended/kubernetes-dashboard.yaml
> kubectl get pods --namespace=kube-system -l k8s-app=kubernetes-dashboard

> kubectl proxy	//dashboard가동

http://localhost:8001/api/v1/namespaces/kube-system/services/https:kubernetes-dashboard:/proxy 에 접속
```

```powershell
> kubectl proxy --port=8001 --address=<접속할 IP> --accept-hosts='^*$'
```

<br>

<br>

## Kubernetes POD

### 1. Linux에서 실행하기

- 브라우저에서 8000번으로 접속

```powershell
PS C:\Users\HPE> cd C:\Users\HPE\docker\day04
PS C:\Users\HPE\docker\day04> code .
PS C:\Users\HPE\docker\day04> dir


    디렉터리: C:\Users\HPE\docker\day04


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----     2020-01-14   오후 4:54             73 Dockerfile
-a----     2020-01-14   오후 4:54            186 hello.js
-a----     2020-01-14   오후 4:54            195 pod.yml


PS C:\Users\HPE\docker\day04> node -v
v12.14.0
PS C:\Users\HPE\docker\day04> node hello.js
```

<br>

### 2. Dockerfile 실행하기

- 브라우저에서 8100번으로 접속

```powershell
> docker build -t ddsd0000/hello:latest .	//이미지 빌드
> docker run -p 8100:8000 ddsd0000/hello:latest	//실행
> docker push ddsd0000/hello:latest		//이미지 push
```

<br>

### 3. kubernetes에서 실행하기

### * POD 만들기

#### 1) dashboard로 실행

- 실제 운영단계에선 사용하지는 않음
- dashboard에서 `+CREATE` 눌러 **pod.yml** 파일 만들기 `UPLOAD`

```yml
apiVersion: v1
kind: Pod
metadata:
  name: hello-pod
  labels:
    app: hello
spec:
  containers:
  - name: hello-container
    image: ddsd0000/hello
    ports:
    - containerPort: 8000
```

- PODS 섹션 들어가면 새로운 POD 확인 가능
- `PODS` 섹션에서 hello-pod 선택 후 `EXEC` 선택하면 터미널 모드 가능

<br>

- **service.yml** 파일 생성하기

```yml
apiVersion: v1
kind: Service
metadata:
  name: hello-svc
spec:
  selector:
    app: hello
  ports:
    - port: 8200
      targetPort: 8000
  type: LoadBalancer	
  externalIPs:
  - 59.29.224.176
```

-  `+CREATE` 후 `UPLOAD`
- http://59.29.224.176:8200/  접속하면 확인 가능

- type
  - **Loadbalancer** : 외부로 노출
  - **clusterIP** : 내부에서만 사용 (Default)



#### 2) kubectrl 이용

```powershell
> kubectl apply -f *.yml	// kubectl에 등록하기
> kubectl delete -f hello-pod.yaml	//pod 삭제

> kubectl get pods	//파드 목록 확인
> kubectl get all
> kubectl get services
```





