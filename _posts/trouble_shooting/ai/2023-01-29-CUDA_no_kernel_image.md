---
title: "[CUDA] 디바이스에서 실행할 수 있는 커널 이미지가 없음"

categories:
  - trouble_shooting
tags:
  - [문제 해결, 이슈 해결, trouble shooting, CUDA]
---

# 1. 에러명
```bash
CUDA error: no kernel image is available for execution on the device
CUDA kernel errors might be asynchronously reported at some other API call,so the stacktrace below might be incorrect.
For debugging consider passing CUDA_LAUNCH_BLOCKING=1.
```

# 2. 문제
PyTorch를 사용해 학습을 하려다가 다음과 같은 CUDA 에러가 발생했다. 그래서 왜 이런 에러가 발생했는지 조사를 해봤는데 PyTorch 라이브러리와 CUDA 버전이 맞지 않아 발생한 간단한 에러이다. 따라서 PyTorch와 CUDA에 대한 호환성을 맞춰 주기 위해 각자 환경에 맞는 버전을 설치해야 한다.<br>

# 3. 해결 방법
[이전 버전 설치 방법](https://pytorch.org/get-started/previous-versions/)에 나와있는 코드를 그대로 복사 붙여넣기 하던가, 최신 버전 설치를 하고 싶다면 [여기](https://pytorch.org/get-started/locally/)에서 해당 코드를 복사 붙여넣기 하면 된다.<br>

위에는 CUDA 버전에 맞는 PyTorch를 재설치하는 것이기 때문에 CUDA와 cudnn이 설치되어 있지 않다면, 먼저 CUDA와 cudnn을 설치해야 한다.
{: .notice--warning}