---
title: "[Flutter] FutureBuilder와 StreamBuiler"
categories: Flutter
tags:
  - Flutter
  - Application
search: true
---


## 공통점
* FutureBuilder와 StreamBuiler 둘 다 동일한 동작을 하는 비동기 빌더다.  
* 각각의 개체에 대한 변경 사항을 수신하고 새로운 값을 받으면 새로운 빌드를 한다.  
* Future 혹은 Stream이 완료되었는지, 에러가 있었는지 여부를 얻을 수 있다.  

## 차이점
### FutureBuilder
* 오직 Future 하나의 응답만 받는다.  
* Future 변수 변경을 들을 수 없다.   
* 이미지 가져오기, HTTP 요청 만들기 등과 같은 일회성 응답에 사용된다.  

### StreamBuilder
* 시간이 지남에 따라 변할 수 있는 값으로 바뀔 수 있다.  
* Stream 각각의 새 값을 얻을 수 있다.  
* 위치 업데이트 듣기, 음악 재생, 스톱워치 등과 같은 데이터를 2번 이상 가져오는 경우 사용된다.  
