---
title: "[Docker] python으로 구성된 Image 파일 만들기"

categories:
  - tool
tags:
  - [사용법, Docker, python, Image, DockerFile]
---

# 1. ToDo
Docker 설치를 한 후에 간단하게 테스트해보자.   
그래서 DockerFile로 python으로 짜여진 코드를 실행시키는 Image 파일을 만들어보겠다.

# 2. 사용법
   
## 2-1. DockerFile, py 만들기
```bash
mkdir example_project
cd example_project
vi DockerFile
```

```bash
FROM python:3.9
WORKDIR /root
ADD . .
CMD ["python3", "hello.py"]
```

```bash
vi example.py

# In vi editor
# i
print('Hi')

# :wq
```

## 2-2. Image 파일 생성 후 실행
```bash
docker build . -t test:v1 # {name}:{version}
docker run test:v1

# output
Hi
```