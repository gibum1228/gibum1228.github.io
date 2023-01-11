---
title: "[Docker] Docker run 명령어 실행 에러 - ~ remove the container ~"

categories:
  - trouble_shooting
tags:
  - [문제 해결, 이슈 해결, trouble shooting, docker, container]
---

# 1. 에러명
```bash
You have to remove (or rename) that container to be able to reuse that name.
```

# 2. 문제
Docker Container가 살아있어서 run 실행 시 작동이 안 될 때가 있는데 이런 에러를 해결하기 위한 명령어들이 있다.   

# 3. 해결 방법
```bash
# 현재 실행중인 컨테이너 목록 보기
docker ps

# 원하는 컨테이너 멈추기
docker stop <컨테이너명>

# 컨에티너 죽이기
docker kill <컨테이너명>
```