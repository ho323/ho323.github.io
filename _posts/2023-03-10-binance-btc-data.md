---
title: "바이낸스(Binance)에서 비트코인(BTC) ohlcv 5분봉 가져오기"
categories: Python
tags:
  - Python
  - Crawling
  - ML
  - Reinforcement Learning
search: true
---

나는 평소에 바이낸스 선물 거래를 한다.  
그러다 어떤 데이터를 넣고 실험해 보고 싶은 게 생겨서 강화 학습으로 만들어 보려고 한다.  

강화 학습 공부도 할 겸.  


## 5분봉으로 선택한 이유
나는 단타 모델로 만들 거라서 5분봉 데이터를 사용하기로 했다.  
그렇다고 1분봉으로 하기엔 거래가 너무 자주 일어나서 수수료 때문에 손해를 볼 것 같았다.  
~~바이낸스 시장가 수수료 양아치~~

문제는 바이낸스에선 최대 1000개의 데이터만 가져올 수 있다.  
그래서 지금부터라도 천천히 데이터를 쌓아가 될 것 같다.  


## 5분봉 가져오는 코드
```python
import time
import requests
import json
import csv
from datetime import datetime

url = "https://api.binance.com/api/v3/klines"
interval = "5m"
symbol = "BTCUSDT"
limit = 1  # 5분봉 데이터를 하나씩 가져옴

while True:
    params = {"symbol": symbol, "interval": interval, "limit": limit}
    response = requests.get(url, params=params)

    if response.status_code == 200:
        data = json.loads(response.content)
        with open("btc_usdt_ohlcv.csv", mode="a", newline="") as file:
            writer = csv.writer(file)
            for item in data:
                timestamp = item[0] / 1000  # 밀리초를 초 단위로 변환
                dt_object = datetime.fromtimestamp(timestamp)
                dt_str = dt_object.strftime("%Y-%m-%d %H:%M")
                open_price = float(item[1])
                high_price = float(item[2])
                low_price = float(item[3])
                close_price = float(item[4])
                volume = float(item[5])
                writer.writerow([dt_str, open_price, high_price, low_price, close_price, volume])
    else:
        print("Error:", response.status_code)

    time.sleep(300)  # 5분 대기
```
코드는 300초(5분)마다 반복 실행된다.  

"https://api.binance.com/api/v3/klines"에서 바이낸스 BTC/USDT 쌍의 5분봉 데이터를 가져온다.  
가져온 데이터의 ohlcv 데이터만 btc_usdt_ohlv.csv에 저장한다.  

newline 인자를 빈 문자열("")로 지정하여 csv 파일에 빈 줄을 삽입하지 않도록 한다.  

datetime 모듈을 사용하여 timestamp를 datetime 객체로 변환하고, strftime() 메서드를 사용하여 필요한 형식으로 포맷한다.  
이 코드에서는 "%Y-%m-%d %H:%M" 형식으로 포맷팅한다. 이 형식은 연도-월-일 시:분으로 표현한다.  


## 마치며
경제적 자유를 꿈꿔본다...  