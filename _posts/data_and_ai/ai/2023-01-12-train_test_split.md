---
title: "[AI] train_test_split으로 데이터를 학습 데이터와 검증 데이터로 분류하기"

categories:
  - data_and_ai
tags:
  - [인공지능, 데이터, 데이터 전처리]
---

모델을 학습하고 test 데이터셋로 검증해 모델을 수정한다면, 그 모델은 좋은 모델이라고 할 수 없다.<br>
test 데이터셋은 딱 한 번만 사용되어야 하기 때문에 검증 데이터(validation data)를 만들어 학습 과정에서 제대로 학습이 되는지 확인한다.<br>
없는게 없는 sklearn 라이브러리에서 train_test_split()으로 raw 데이터에서 학습 데이터와 검증 데이터를 분리할 수 있다.<br>

# Import
```python
from sklearn.model_selection import train_test_split
```

# Code
```python
images = {이미지 데이터}
labels = {레이블 데이터}

train_images, val_images, train_labels, val_labels = train_test_split(images, labels, test_size=0.2, stratify=train_labels, random_seed=seed)
```

위 코드를 작동시키면 원본 데이터 image가 `test_size` 크기(0.2는 20%)만큼 검증 데이터셋을 만들고 나머지 80%은 학습 데이터가 된다.<br>
만약에 테스트 데이터가 필요하다면, val_*가 아닌 test_*으로 해주면 된다.<br>
매개변수 `stratify`은 labels를 넘겨 받아 분리되는 과정에서 label 비율에 맞게 분리해주기 때문에 좋다.<br>
또한, `random_seed`는 매번 무작위로 나오지 않도록 seed를 지정해주면 실험하는데 용이하다.<br>