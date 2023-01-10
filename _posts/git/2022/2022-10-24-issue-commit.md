---
title: "[Git] Commit 메세지에 issue 연결하기"

categories:
  - git
tags:
  - [깃, 깃허브, M1, github, issue, 이슈와 커밋 연결]
---

git commit을 통해 커밋 메세지 입력 시 issue와 연결하기 위해서는 **#[이슈번호]**로 표기하면 된다.

하지만 -m 명령어가 아닌 notepad++나 다른 text 도구를 사용할 경우 #가 주석 기능을 수행하기 때문에 #[이슈번호]를 입력해도 커밋 메세지에는 보이지 않는다.

그렇기 때문에 #[이슈번호]를 사용하기 위해 주석 기능을 하는 문자를 바꾸면 된다.

```bash
git config --global core.commentChar ";"
```

;를 사용하는 이유는 모르지만 관례인 것 같다.