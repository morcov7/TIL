# OpenStack

### 1. OpenStack?

-  모든 타입의 클라우드 컴퓨팅 플랫폼을 개발하고 관리할 수 있는 오픈 소스 소프트웨어 기반의 클라우드 운영체제
- 서비스형 **IaaS (Infrastructure as a Service)**를 구축하게 해주는 오픈소스 플랫폼
- 컴퓨팅, 스토리지, 네트워크와 같은 자원들을 모으고 이들을 제어하고 운영하는 클라우드 운영체제



### 2. OpenStack의 구성요소

![img](https://t1.daumcdn.net/cfile/tistory/99127B505C70CF9927)

- Cinder : 네트워크로부터 remote storage를 사용



### 3. OpenStack의 서비스

#### Snapshot

- volume snapshot

- instance snapshot
  - instance의 rootdisk backup
  - 새로운 볼륨 생성 O : cinder스토리지에 root disk  사용
  -  새로운 볼륨 생성 X : nova 스토리지에 root disk 사용
  - scale out 방식으로 확장



#### Object Storage (Swift)

- AWS의 bucket과 유사한 Container
- Windows의 partition 개념 → 폴더 생성해 object 관리 가능
- 구글 Drive 등은 Swift 계열의 object Storage 사용한 것



### 4. Identity Service (Keystone)

#### 서비스 특징

- openstack 인증 서비스 , 사용자 관리, 서비스 카탈로그 관리 위한 

  통합 서비스를 제공하는 오픈스택의 핵심 구성요소

- 서비스 카탈로그 : 오픈스택이 설치된 환경에서 사용 가능한 서비스들의 집합

- MariaDB 형태로 user 등 관리

  

#### 서비스 구조

- 사용자
- Credentials : ID PW
- 인증
- 토큰 : 자원 접근시 사용
- Tenant (지금은 Project)
- 서비스 ex) Nova, Swift ...
- End Point : 네트워크로 접근 가능한 주소 (ver, port, ip 등 URL 형태)
- Role
  - _ member_ : 일반



#### CLI Command

1. 권한과 관련된 환경설정

   - [root@controller ~]# cat keystonerc_admin 

2. 설정하기

   - [root@controller ~]# source keystonerc_admin 
   - [root@controller ~]# . keystonerc_admin 
   - [root@controller ~(keystone_admin)]# openstack user list

3. unset 만들기

   - cp keystonerc_admin unset_keystonerc_admin

   - 패턴제거

     ​	:%s/=.*//

   - .unset_keystonerc_admin

4. CLI로 프로젝트 생성

   - openstack user create --password abc123 --project demo demo
   - openstack role add --project demo --user demo _ member_
   - openstack role list --project demo --user demo



- systemctl enable : 다음 부팅시 재실행

- systemctl start : 즉시 실행
-  mysql -uroot -p : MariaDB 실행



- 프로세스 확인
  - netstat -an | grep 11211
  - ss -nlp | grep 11211



#### Manual 설치

- packstack으로 자동 설치하는 것 보다 수동으로 하나하나 설치
- https://docs.openstack.org/install-guide/