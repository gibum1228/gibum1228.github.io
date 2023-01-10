---
title: "[Git] fatal: refusing to merge unrelated histories 에러 해결"

categories:
  - git
tags:
  - [깃, 깃허브, git, error, 에러]
---

로컬 저장소에서 리모트 저장소에 push 할 경우 아래와 같은 메세지가 뜨는 경우가 있습니다.
```bash
hint: Updates were rejected because the tip of your current branch is behind
hint: its remote counterpart. Integrate the remote changes (e.g.
hint: 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```

이는 로컬 저장소와 리모트 저장소가 같지 않기 때문에 병합을 해주어야 합니다.   
병합을 하기 위해서는
```bash
git pull remote 별명 브런치명
```
을 해주어야 하는데 다음과 같은 에러가 발생하지 않으면 push해주면 되지만, 이런 에러가 발생한다면?
```bash
 * branch            master     -> FETCH_HEAD
fatal: refusing to merge unrelated histories
```
--allow-unrelated-histories 옵션을 추가해줘야 합니다.
```bash
git pull remote 별명 브런치명 --allow-unrelated-histories
```
이 속성은 서로 관련 기록이 없는 이질적인 두 프로젝트를 병합할 때 기본적으로 거부하는데, 이를 허용해주기 위한 속성입니다.