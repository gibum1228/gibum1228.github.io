---
title: "Object Detection에서 검증 지표 (Precision, Recall, Average Precision, mAP)"

categories:
  - data_and_ai
tags:
  - [인공지능, 데이터, Object Detection, Precision, Recall, AP, Average Precision, 검증 지표]
---

Object Detection 문제에서 모델 성능을 평가할 때 단일 레이블이먄 AP(Average Precision)을 사용하고 다중 레이블이면 mAP(mean Average Precision)을 사용한다.<br>
mAP는 AP로 계산할 수 있고 AP는 PR 곡선(Precision-Recall Curve)으로 구하며, PR 곡선은 Precision과 Recall으로 구하기 때문에 Object Detection 문제를 해결하는 모델의 성능을 검증하기 위해서 Precision과 Recall을 먼저 알아야 한다.<br>

# Precision
-----

Precision은 정확도를 의미하며 검출 결과 중 올바르게 검출한 비율을 의미한다.<br>

$$Precision = \frac{TP}{TP + FP} = \frac{TP}{All Detections}$$

예를 들어 객체가 있는 이미지에서 4개의 객체를 검출해야 하지만 10개의 객체를 검출했다면, 이 상황에서 Precision은 $\frac{4}{10}$으로 0.4이다.<br>

# Recall
-----

Recall은 재현율이라고 하며 실제 올바르게 검출된 결과에서 올게 예측한 것의 비율을 의미한다.<br>

$$Recall = \frac{TP}{TP + FN} = \frac{TP}{All Ground truths}$$

이미지에 검출해야 하는 객체는 10개인데 검출된 것 중 4개만 올바르게 예측했다면 Recall은 $\frac{4}{10}$이므로 0.4가 된다.<br>

# Precision-Recall 곡선
-----

일반적으로 Precision과 Recall은 서로 반비례 관계를 가진다. 그래서 Precision과 Recall의 성능 변화를 확인해야 하며 이를 확인하기 위해 Precision-Recall 곡선을 이용해야 한다.<br>


# Average Precision
-----



# mean Average Precision
-----

