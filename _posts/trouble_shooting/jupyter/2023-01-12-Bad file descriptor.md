---
title: "[Jupyter] 주피터 노트북 ERROR "Bad file descriptor""

categories:
  - trouble_shooting
tags:
  - [문제 해결, 이슈 해결, trouble shooting, 주피터 노트북, 주피터, jupyter]
---

# 1. 에러명
```bash
Bad file descriptor (C:\...\epoll.cpp:100)
```

# 2. 문제
주피터 노트북에서 커널 실행이 안 되는데 Anaconda Prompt에서 로그를 보면 위와 같은 에러가 발생했다.<br>

# 3. 해결 방법
위 에러는 pyzmq 라이브러리 충돌로 인해 그렇다고 한다.<br>
그래서 버전을 다르게 설치해주면 되기 때문에 다음 명령어를 순서대로 수행하면 된다.

```bash
pip uninstall pyzmq
pip install pyzmq==19.0.2
```