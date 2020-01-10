# Git 언어 설정 바꾸기

- Docker에 대해 공부하고 있는 repository의 언어가 javascript로 자동 설정되어서 변경하고자한다.

- .gitattribute 파일 생성

  <br>

```bash
*.js linguist-vendored
```

- javascript 파일을 무시하고 싶으면 이 라인을 추가하면 된다.

  <br>

```
폴더명 ex) club/* linguist-vendored
```

- 특정 폴더 내의 파일을 무시하고 싶으면 이 라인 추가