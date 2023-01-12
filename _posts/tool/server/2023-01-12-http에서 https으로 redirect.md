---
title: "[Apache] HTTP에서 HTTPS으로 Redirect 하기"

categories:
  - tool
tags:
  - [사용법, Apache]
---

# 1. ToDo
[여기](https://gibum1228.github.io/tool/%EB%AC%B4%EB%A3%8C-SSL-%EC%9D%B8%EC%A6%9D%EC%84%9C-%EB%B0%9C%EA%B8%89/)를 보면 SSL 인증을 받아 https 접속이 가능해졌다.<br>
하지만 여전히 도메인으로 접속하면 http를 기본으로 접속하게 된다. 그래서 http 요청 시 https로 redirect해서 https를 보여주도록 해야 한다.<br>

# 2. 사용법
우선 httpd.conf 파일에서 필요한 모듈에 주석을 풀어줘야 한다.

```bash
LoadModule rewrite_module modules/mod_rewrite.so
```

그리고 httpd.conf나 httpd-vhost.conf 파일에서 VirtulaHost를 다음과 같이 설정해주면 된다.

```bash
<VirtualHost *:80>
	...
    RewriteEngine On
    RewriteCond %{HTTPS} off
    RewriteRule (.*) https://%{HTTP_HOST}%{REQUEST_URI} [R,L]
    ...
</VirtualHost>
```

마지막으로 Apache를 재시작해주면 http 접속 시 https로 자동 연결된다.