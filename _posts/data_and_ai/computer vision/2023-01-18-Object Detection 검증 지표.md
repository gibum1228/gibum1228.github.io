---
title: "Object Detection에서 검증 지표 (Precision, Recall, Average Precision, mAP)"

categories:
  - data_and_ai
tags:
  - [인공지능, 데이터, Object Detection, Precision, Recall, AP, Average Precision, 검증 지표]
---

Object Detection 문제에서 모델 성능을 평가할 때 단일 레이블이먄 AP(Average Precision)을 사용하고 다중 레이블이면 mAP(mean Average Precision)을 사용한다.<br>
mAP는 AP로 계산할 수 있고 AP는 PR 곡선(Precision-Recall Curve)으로 구하며, PR 곡선은 Precision과 Recall으로 구하기 때문에 Object Detection 문제를 해결하는 모델의 성능을 검증하기 위해서 Precision과 Recall을 먼저 알아야 한다.<br>
<br>
Precision과 Recall을 설명하기 이전에 Confusion Matrix라고 하는 아래 표를 보고 용어를 이해해야 한다.<br>

> ![confusion matrix](/assets/images/post/AP/confusion%20matrix.png)

# Precision
-----

Precision은 정확도를 의미하며 검출 결과 중 올바르게 검출한 비율을 의미한다.<br>

$$Precision = \frac{TP}{TP + FP} = \frac{TP}{All\,Detections}$$

예를 들어 객체가 있는 이미지에서 4개의 객체를 검출해야 하지만 10개의 객체를 검출했다면, 이 상황에서 Precision은 $\frac{4}{10}$으로 0.4이다.<br>

# Recall
-----

Recall은 재현율이라고 하며 실제 올바르게 검출된 결과에서 올게 예측한 것의 비율을 의미한다.<br>

$$Recall = \frac{TP}{TP + FN} = \frac{TP}{All\,Ground\,truths}$$

이미지에 검출해야 하는 객체는 10개인데 검출된 것 중 4개만 올바르게 예측했다면 Recall은 $\frac{4}{10}$이므로 0.4가 된다.<br>

# Precision-Recall 곡선
-----

일반적으로 Precision과 Recall은 서로 반비례 관계를 가진다. 그래서 Precision과 Recall의 성능 변화를 확인해야 하며 이를 확인하기 위해 PR(Precision-Recall) 곡선을 이용해야 한다.<br>
PR 곡선은 물체를 검출하는 알고리즘의 성능을 평가하는 방법 중 하나로 threshold에 따라 Precision과 Recall이 달라진다. threshold가 0.4라면 0.4보다 작으면 검출되지 않은거고 0.4 이상이면 검출된다고 판단한다.<br>
사람 한 명씩 있는 15개의 이미지에서 사람을 검출했을 때 10개의 이미지에서 검출 결과가 있지만, 7개만 사람을 검출했고 3개는 다른 객체를 검출해 잘못 검출했다고 가정하자. 그렇다면 Precision은 $\frac{올바르게\,검출된\,이미지}{검출된\,이미지} = \frac{7}{10}$이라 0.7이고 Recall은 $\frac{올바르게\,검출된\,이미지}{총\, 이미지} = \frac{7}{15}$이기 때문에 0.47을 갖는다.<br>
이렇게 구해진 값들로 x축은 Recall, y축은 Precision으로 한다면 다음과 같은 그래프(PR 곡선)이 그려진다.<br>

> ![pr 곡선](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FqsCQt%2FbtqufbT4BWO%2FRgG7ha2HvpWv52sp4k4sEk%2Fimg.bmp)

# Average Precision
-----

PR 곡선은 알고리즘의 성능을 전반적으로 파악하기에는 좋으나 서로 다른 두 알고리즘의 성능을 정량적으로 비교하기에는 불편한 점이 있어 이를 해결하고자 Average Precision 개념이 나왔다.<br>
Average Precision(AP)은 인식 알고리즘의 성능을 하나의 값으로 표현한 것으로 PR 곡선에서 선 아래 쪽의 면적으로 계산한다. AP가 높으면 높을수록 그 알고리즘의 성능이 전체적으로 우수하다는 의미이며 컴퓨터 비전에서 Object Detection 알고리즘의 성능은 대부분 AP로 평가한다.<br>

> ![AP](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fbg73Gf%2FbtquhjDtzFh%2FETRORigF8P4dT02zB4kox1%2Fimg.png)

계산 전에 PR 곡선을 계단 형식 그래프로 변경한다. 그런 다음에 그래프 선 아래의 넓이를 계산해 AP를 구한다. 여기서 AP는 왼쪽 큰 사각형의 넓이 + 오른쪽 자은 사각형의 넓이이다.<br>
<br>
mAP(mean Average Precision)은 이런 AP 연산 공식으로 클래스별 AP 값을 구해 클래스 수만큼 평균을 냈을 때 가지는 값이다.