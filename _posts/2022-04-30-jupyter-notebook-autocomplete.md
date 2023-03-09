---
title: "Jupyter Notebook에서 자동완성 기능 사용하기"
categories: Python
tags:
  - Python
  - Jupyter
search: true
---

주피터 노트북에서도 vscode처럼 자동완성 기능을 사용할 수 있다.  
이런 기능을 사용할 수 있다는 것을 오늘 처음 알게 되었는데 너무 좋아서 감격스러움에 바로 글을 작성하고 있다.  

방법은 너무나도 간단하다.  
우선 2가지의 방법이 있다.  

## 방법1
터미널에 아래와 같이 jedi를 삭제해준다.  
`pip uninstall jedi`

## 방법2
jedi를 삭제하지 않으려면 주피터 노트북에서 아래와 같은 magic command를 입력해준다.  
`%config Completer.use_jedi = False`

## 사용법1 
코드 도중 Tab키로 자동완성할 수 있다.   
<img width="433" alt="post" src="https://user-images.githubusercontent.com/86637300/135465424-621c935c-db26-4d69-b82b-4c6a5c5e36f0.png">

## 사용법2
Shift+Tab을 누르면 해당 Docstring을 확인할 수 있다.  
<img width="754" alt="post2" src="https://user-images.githubusercontent.com/86637300/135466289-b07addec-90a1-433a-9f4d-2d6297b843d8.png">
