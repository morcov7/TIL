#  포크한 Repository 동기화하기

예전에 포크한 저장소가 최근에 업데이트된 사항을 반영하지 않고 있어 Repository 동기화를 진행했습니다. 

구글링해 찾아본 방법을 기록합니다.



### 저장소

- 원본 저장소 (update된 원본 저장소)

  https://github.com/devAon/DATABASE-PROGRAMING.git

- 나의 저장소 (fork된 나의 저장소)

  https://github.com/morcov7/DATABASE-PROGRAMING.git



### Git bash

```bash
1. 로컬 저장소로 이동
$ cd DATABASE-PROGRAMMING

2. 설정된 리모트 저장소 조회
$ git remote -v

3. 리모트 저장소에 원본 저장소 추가
$ git remote add upstream https://github.com/devAon/DATABASE-PROGRAMING.git

4. upstream으로부터 최신 update 가져오기
$ git fetch upstream

5. upstream → 나의 로컬 master branch로 merge
$ git checkout master
$ git merge upstream/master

6. push
$ git push origin master
```

