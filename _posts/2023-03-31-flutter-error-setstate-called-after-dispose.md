---
title: "[Flutter] 에러 해결 - setState() called after dispose(): (lifecycle state: defunct, not mounted)"
categories: Flutter
tags:
  - Flutter
  - Error
  - App
  - Lifecycle
search: true
---


## 에러
```
setState() called after dispose(): (lifecycle state: defunct, not mounted
```  
위젯이 마운트 되지 않은 상태에서 setState 함수를 호출하면 나오는 에러라고 한다.  
내가 async 함수에서 setState를 쓰려고 했는데 await 이후에는 더 이상 마운트 되지 않는다고 한다.  

## 솔루션
아래와 같이 해결할 수 있다.  
```dart
if(mounted){
  setState(() {});
}
```

## Mounted
마운트란?  
State를 생성하면, 마운트라는 boolean 속성을 true로 설정해서, State를 BuildContext와 연결한다.  
이 속성은 이 State가 현재 위젯 트리에 있는지, 없는 지에 대한 정보를 제공한다.  
