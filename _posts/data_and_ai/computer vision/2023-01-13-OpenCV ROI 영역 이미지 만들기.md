---
title: "[OpenCV] ROI 영역 그리고 마스크 이미지 만들기"

categories:
  - data_and_ai
tags:
  - [인공지능, 데이터, OpenCV, 컴퓨터 비전, ROI 영역 그리기, 마스크 이미지 만들기]
---

관심 영역(ROI; Region Of Interest)을 추출해내기 위해서는 두 개의 데이터가 필요한데 그것은 원본 이미지 데이터와 마스크 이미지 데이터이다.<br>
<br>
알고리즘을 간단하게 이야기하자면, 원본 이미지에 마스크 이미지를 올려 마스크 이미지에서 일반적으로 검은색이 아닌 흰색으로 된 부분에 해당하는 원본 이미지 픽셀값을 추출하는데, 이게 ROI 영역만 활성화 한 것처럼 보이는 것이다.<br>
그렇다면 마스크 이미지는 어떻게 얻는가?<br>
질문의 대답은 사람이 하나하나 ROI 영역을 지정해야한다. ROI 영역을 그리는 방법이 여러가지 있겠지만, 나는 OpenCV를 통해 이미지를 윈도우에 띄우고 마우스 클릭 이벤트를 사용해 ROI 영역을 지정한 걸 저장하여 마스크 이미지를 만든다.<br>

# Import
```python
import cv2 as cv
```
openv 라이브러리는 python 코드 내에서 cv2로 호출하지만 패키지명은 opencv-python이다. 따라서 `pip install opencv-python`으로 설치 후 import 해야 한다.<br>

# Code

## 마우스 클릭 이벤트
OpenCV에 다양한 이벤트가 있지만, 나는 마우스를 사용할 것이기 때문에 4가지 마우스 클릭 이벤트를 소개하겠다.<br>

|이벤트|행동|
|:--:|:--:|
|EVENT_LBUTTONDOWN|마우스 왼쪽 버튼 클릭|
|EVENT_RBUTTONDOWN|마우스 오른쪽 버튼 클릭|
|EVENT_LBUTTONDBLCLK|마우스 왼쪽 버튼 더블 클릭|
|EVENT_MBUTTONDOWN|마우스 휠 클릭|

## 창과 이벤트 리스너 연결
우선 메소드명을 자율적으로 지정해 이벤트 리스너 메소드를 만들어야 한다. 나는 `draw_mask_eventListener`라고 지었다. 그런 다음 창과 이벤트 리스너를 연결해줘야 하는데 그 과정은 다음과 같다.<br>

```python
img = cv.imread({경로}, flag={색 정보})
cv.startWindowThread()
cv.namedWindow('image')  # 새로운 윈도우 창 이름 설정
cv.setMouseCallback('image', draw_mask_eventListener)  # 마우스 이벤트가 발생했을 때 전달할 함수
```

## 클릭 이벤트 구현
이벤트 리스너에서는 (event, x, y, flag, param)을 매개변수로 받는데 event는 어떤 이벤트로 호출됐는지 알려주고, x와 y는 이벤트 발생 당시 좌표를 의미한다.<br>
지금 글에서 왼쪽 버튼 클릭시 현재 좌표를 추가하고 더블 클릭시 좌표를 연결하여 선을 그린 후에 내부를 색칠한다(채운다).<br>
오른쪽 버튼 클릭시 최신 좌표를 지울 수 있고 휠 버튼을 누르면 지금까지 지정한 ROI 영역을 저장해 마스크 이미지로 만들고 프로그램을 종료한다.<br>

```python
# 마스크 추출을 위한 마우스 클릭 이벤트 리스너
    def draw_mask_eventListener(event, x, y, flags, param):
        global pts
        img2 = img.copy()

        if event == cv.EVENT_LBUTTONDOWN:  # 마우스 왼쪽 버튼 클릭 시 pts에 (x,y)좌표를 추가
            pts.append((x, y))

        if event == cv.EVENT_RBUTTONDOWN:  # 마우스 오른쪽 버튼 클릭 시 클릭 했던 포인트를 삭제
            pts.pop()

        if event == cv.EVENT_LBUTTONDBLCLK:  # 마우스 왼쪽 버튼 더블 클릭 시 좌표들을 리스트에 추가
            # 초기화
            mask_list.append(pts)
            pts = []

        if event == cv.EVENT_MBUTTONDOWN:  # 마우스 중앙(휠)버튼 클릭 시 ROI 선택 종료
            result_roi = np.zeros(img.shape, np.uint8)  # 최종 마스크 이미지

            for point in mask_list:
                if not point: continue
                mask = np.zeros(img.shape, np.uint8)
                points = np.array(point, np.int32)
                points = points.reshape((-1, 1, 2))  # pts 2차원을 이미지와 동일하게 3차원으로 재배열
                mask = cv.polylines(mask, [points], True, (255, 255, 255), 2)  # 포인트를 연결하는 라인을 설정 후 마스크 생성
                mask2 = cv.fillPoly(mask.copy(), [points], (255, 255, 255))  # 채워진 다각형 마스크 생성

                ROI = cv.bitwise_and(mask2, img)  # img와 mask2에 중첩된 부분을 추출

                result_roi = cv.add(result_roi, ROI)  # 마스크 이미지끼리 더하기

            result_roi = np.where(result_roi == 0, result_roi, 255)  # 첫번째 매개변수 조건에 따라 참이면 유지, 거짓이면 255으로 변경
            cv.imwrite('result_roi.png', result_roi) # 저장
            cv.destroyAllWindows()  # 열린 창 닫기
            cv.waitKey(0)

        try:
            if len(pts) > 0:  # 마우스 포인트 원으로 지정
                cv.circle(img2, pts[-1], 3, (0, 0, 255), -1)
        except:
            pts = []

        if len(pts) > 1:  # 마우스 포인트 연결 라인 생성
            for i in range(len(pts) - 1):
                cv.circle(img2, pts[i], 5, (0, 0, 255), -1)
                cv.line(img=img2, pt1=pts[i], pt2=pts[i + 1], color=(255, 0, 0), thickness=2)

        if len(mask_list) > 0:  # 마스크 여러 개일때 포인트 연결 라인 생성
            for m in mask_list:
                for i in range(len(m) - 1):
                    cv.circle(img2, m[i], 5, (0, 0, 255), -1)
                    cv.line(img=img2, pt1=m[i], pt2=m[i + 1], color=(255, 0, 0), thickness=2)

        cv.imshow('image', img2)  # 이미지 화면 출력
```

## 전체 코드
좌표 정보와 ROI 영역을 유지하고자 마지막 두 개의 if문이 작동되어야 한다.<br>
그래서 마우스 클릭 이벤트를 사용해 ROI 영역을 그리는 전체 코드는 다음과 같다.<br>

```python
import cv2 as cv

# ROI 마스크 PNG 파일 추출
def draw_mask():
    print("in draw_mask")
    pts = []  # 마우스로 클릭한 포인트 저장
    mask_list = []  # 마스크 리스트 저장

    # 마스크 추출을 위한 마우스 클릭 이벤트 리스너
    def draw_mask_eventListener(event, x, y, flags, param):
        global pts
        img2 = img.copy()

        if event == cv.EVENT_LBUTTONDOWN:  # 마우스 왼쪽 버튼 클릭 시 pts에 (x,y)좌표를 추가
            pts.append((x, y))

        if event == cv.EVENT_RBUTTONDOWN:  # 마우스 오른쪽 버튼 클릭 시 클릭 했던 포인트를 삭제
            pts.pop()

        if event == cv.EVENT_LBUTTONDBLCLK:  # 마우스 왼쪽 버튼 더블 클릭 시 좌표들을 리스트에 추가
            # 초기화
            mask_list.append(pts)
            pts = []

        if event == cv.EVENT_MBUTTONDOWN:  # 마우스 중앙(휠)버튼 클릭 시 ROI 선택 종료
            result_roi = np.zeros(img.shape, np.uint8)  # 최종 마스크 이미지

            for point in mask_list:
                if not point: continue
                mask = np.zeros(img.shape, np.uint8)
                points = np.array(point, np.int32)
                points = points.reshape((-1, 1, 2))  # pts 2차원을 이미지와 동일하게 3차원으로 재배열
                mask = cv.polylines(mask, [points], True, (255, 255, 255), 2)  # 포인트를 연결하는 라인을 설정 후 마스크 생성
                mask2 = cv.fillPoly(mask.copy(), [points], (255, 255, 255))  # 채워진 다각형 마스크 생성

                ROI = cv.bitwise_and(mask2, img)  # img와 mask2에 중첩된 부분을 추출

                result_roi = cv.add(result_roi, ROI)  # 마스크 이미지끼리 더하기

            result_roi = np.where(result_roi == 0, result_roi, 255)  # 첫번째 매개변수 조건에 따라 참이면 유지, 거짓이면 255으로 변경
            cv.imwrite('result_roi.png', result_roi) # 저장
            cv.destroyAllWindows()  # 열린 창 닫기
            cv.waitKey(0)

        try:
            if len(pts) > 0:  # 마우스 포인트 원으로 지정
                cv.circle(img2, pts[-1], 3, (0, 0, 255), -1)
        except:
            pts = []

        if len(pts) > 1:  # 마우스 포인트 연결 라인 생성
            for i in range(len(pts) - 1):
                cv.circle(img2, pts[i], 5, (0, 0, 255), -1)
                cv.line(img=img2, pt1=pts[i], pt2=pts[i + 1], color=(255, 0, 0), thickness=2)

        if len(mask_list) > 0:  # 마스크 여러 개일때 포인트 연결 라인 생성
            for m in mask_list:
                for i in range(len(m) - 1):
                    cv.circle(img2, m[i], 5, (0, 0, 255), -1)
                    cv.line(img=img2, pt1=m[i], pt2=m[i + 1], color=(255, 0, 0), thickness=2)

        cv.imshow('image', img2)  # 이미지 화면 출력

	img = cv.imread({경로}, flag={색깔})
    img = cv.resize(img, (600, 400))
    cv.startWindowThread()
    cv.namedWindow('image')  # 새로운 윈도우 창 이름 설정
    cv.setMouseCallback('image', draw_mask_eventListener)  # 마우스 이벤트가 발생했을 때 전달할 함수

    while True:
        key = cv.waitKey(1) & 0xFF  # SOH
        if key == 27:  # ESC
            break
    cv.destroyAllWindows()  # 열린 창 닫기
```