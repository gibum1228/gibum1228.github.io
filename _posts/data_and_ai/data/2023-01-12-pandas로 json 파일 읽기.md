---
title: "[pandas] json 파일 읽어서 데이터 프레임으로 만들기"

categories:
  - data_and_ai
tags:
  - [인공지능, 데이터, pandas, json]
---

json 파일을 읽은 다음에 데이터 프레임을 만들려고 한다.<br>
사용하는 json은 MS-ASL 데이터에 대한 정보가 담긴 파일이다.

# Import
```python
import json
import pandas as pd
```

# Code
root 변수를 만들어 절대 경로를 지정해주고 '__main__'으로 메인 함수를 작성하자.

```python
root = {절대경로}

if __name__ == '__main__':
  with open(root + '/datasest/MS-ASL/MSASL_train.json') as f:
    json_data = json.load(f)

    # json 파일 출력하기
    print("train_shape => (", len(json_data), ",", len(json_data[0]),")") # (16054, 16)

    # 각 속성 담기
    url, start_time, end_time, label, signer_id, box, text, width, height, fps = [], [], [], [], [], [], [], [] ,[] ,[]
    
    for i in range(len(json_data)):
        url.append(json_data[i]["url"])
        start_time.append(json_data[i]["start_time"])
        end_time.append(json_data[i]["end_time"])
        label.append(json_data[i]["label"])
        signer_id.append(json_data[i]["signer_id"])
        box.append(json_data[i]["box"])
        text.append(json_data[i]["text"])
        width.append(json_data[i]["width"])
        height.append(json_data[i]["height"])
        fps.append(json_data[i]["fps"])

    # 데이터 프레임 만들기
    df = pd.DataFrame({
        "url": url,
        "start_time": start_time,
        "end_time": end_time,
        "label": label,
        "signer_id": signer_id,
        "box": box,
        "text": text,
        "width": width,
        "height": height,
        "fps": fps
    })
    
    print(df)
```

root를 만들어 준 이유는 Ubuntu 20.04 버전에서 디스크를 마운트해서 사용하고 있기 때문이다.<br>
그래서 기본 경로보다 길기 때문에 전부 다 작성하기 귀찮아 변수로 만들었다.<br>
open() 매개변수로 root + {상대 경로}를 넘겨줘 json 파일을 불러왔다.<br>
그리고 json_data에 json 정보를 담고 제대로 담겼는지 확인하려면 json.dumps에 "\t"를 주어 띄어쓰기가 이쁘게 된 상태로 json_data를 출력할 수 있다.<br>
각 속성명으로 리스트를 만들어 각 속성명에 해당하는 값들을 담고 pd.DataFrame()에 넘겨줘 df라는 데이터 프레임을 만들었다.<br>
마지막으로 데이터 프레임이 잘 만들어졌는데 print()로 실행시켜보면 끝이다.<br>