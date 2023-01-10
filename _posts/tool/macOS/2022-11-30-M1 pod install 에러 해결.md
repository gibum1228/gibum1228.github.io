---
title: "[MAC/M1] M1칩 M1 칩에서 pod install 이슈 해결하기"

categories:
  - tool
tags:
  - [tool, 운영체제, MAC, M1, pod install ,issue]
---

MacBook M1 칩에서 Firebase를 이용해 Swift 프로젝트를 진행하려고 했는데 pod install 하는 과정에서 에러가 발생했다.   
M1 칩 이슈라고는 하는데 구글링을 해보면 다른 고수분들이 해답을 찾아 올렸다.   
그래서 에러에 대한 별다른 설명은 필요 없으니 내가 찾은 해답 코드를 공유하겠다.   

```zsh
sudo arch -x86_64 gem install ffi
```

```zsh
# podFile이 있는 경로에서
arch -x86_64 pod install
```