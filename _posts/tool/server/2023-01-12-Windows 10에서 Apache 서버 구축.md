---
title: "[Apache] Windows 10에서 서버 구축하기"

categories:
  - tool
tags:
  - [사용법, Apache, Windows, 서버 구축]
---

# 1. ToDo
윈도우즈에서 Apache 환경을 구축하는건 쉽다.<br>
우선 [여기](https://www.apachelounge.com/download/)에서 32/64 bit 환경에 맞춰 Apache 파일을 다운 받아 서버 폴더로 지정할 경로에 압축을 푼다.<br>

# 2. 사용법
```
Apache2
  bin
  conf
```

그럼 위와 같은 디렉토리 구조를 가지는데 `conf/`로 이동한 다음에 `httpd.conf`를 메모장 프로그램으로 연다.<br>

> ![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fr8jAO%2FbtrmQ23CeiW%2FAWdzqvMQCSrRHxC86LKmkk%2Fimg.png)

메모장에서 검색 기능을 사용하기 위해 Ctrl + F를 눌러 SRVROOT를 검색하고 찾았으면 루트 디렉토리를 본인의 Apache2 압축을 푼 path로 변경해준다.<br>
그리고 cmd를 관리자 권한으로 연 다음에 현재 위치를 */Apache2/bin/으로 이동하고 다음 명령을 수행하면 Apache Service가 설치된다.<br>

```bash
httpd.exe -k install
```

반대로 Apache Service를 삭제하고 싶다면 다음 명령을 수행하면 된다.<br>

```bash
httpd.exe -k uninstall
```