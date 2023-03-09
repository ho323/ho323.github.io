---
title: "GitHub Commit 비밀번호를 토큰 방식으로 바꾸는 방법"
categories: Git
tags:
  - Git
search: true
---

![github](https://user-images.githubusercontent.com/86637300/133374997-aa710b87-1b29-4fdf-bbdb-8fd8af5d4082.png)
## 갑자기 GitHub Commit 오류?
평소와 똑같이 깃허브 커밋하는데 아래와 같은 오류가 발생했다.  
```
remote: Password authentication is temporarily disabled as part of a brownout. Please use a personal access token instead.  
remote: Please see https://github.blog/2020-07-30-token-authentication-requirements-for-api-and-git-operations/ for more information.  
The requested URL returned error: 403  
```

그래서 저기 써있는 링크에 들어가 봤더니 2021년 8월 13일 이후부터는 모든 Git 작업에 토큰 혹은 SSH키 인증이 필요하다고 한다.  
[https://github.blog/2020-07-30-token-authentication-requirements-for-api-and-git-operations/](https://github.blog/2020-07-30-token-authentication-requirements-for-api-and-git-operations/)  

그래서 어떻게 토큰을 발급받는지 포스팅하려고 한다.  


## Token 발급받는 방법
### 1번째
Github 로그인하고 Settings에 들어간다.  
![스크린샷 2021-09-15 오후 2 08 45](https://user-images.githubusercontent.com/86637300/133375007-303ccf84-94d4-48ea-9aae-79846b47fffd.png)  

### 2번째
Developer settings 탭을 클릭한다.  
![스크린샷 2021-09-15 오후 2 09 45](https://user-images.githubusercontent.com/86637300/133375013-533e8762-1a79-434f-86ba-84709f1af34e.png)  

### 3번째
Personal access tokens 탭을 클릭하고, Generate new token 버튼을 클릭한다.  
![스크린샷 2021-09-15 오후 2 10 29](https://user-images.githubusercontent.com/86637300/133375018-440ef686-2cc9-4a5f-8524-f23a060bebb7.png)  

### 4번째
Note에 토큰 이름, Expiration에 만료기간 설정하고, Select scopes에서 권한을 설정해주면 된다. 
![스크린샷 2021-09-15 오후 2 12 38](https://user-images.githubusercontent.com/86637300/133375022-5d1bb082-860e-43bc-97a2-4855336187d9.png)  

### 5번째
Generate toekn 버튼을 클릭하면 토큰이 발급되는데, 그 코드를 안전한 곳에 복사해서 보관해줘야 한다.  
![스크린샷 2021-09-15 오후 2 17 00](https://user-images.githubusercontent.com/86637300/133375026-b600e24a-814d-4fbb-9fd3-050b7b2ac140.png)  

### Mac
맥북의 경우 Spotlight 검색에서 Keychain access를 검색해서 github를 검색하고 github.com 인터넷 암호를 제거해준다.  


## 다시 GitHub Commit 시도
이제 다시 git push를 하면  
<img width="324" alt="스크린샷 2021-09-15 오후 2 20 55" src="https://user-images.githubusercontent.com/86637300/133375225-864a692a-2881-4e16-a6f1-355d9e1ae9b1.png">  
username과 password를 묻는다.  
username에는 내 깃허브 username을 똑같이 입력하고,  
password에는 내 깃허브 비밀번호가 아니라 아까 발급받은 ghp어쩌구로 시작하는 이상한 토큰을 입력하면 된다.  
