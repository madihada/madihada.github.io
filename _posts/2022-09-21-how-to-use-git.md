---
layout: single
title:  "Github 사용방법!"
---

깃을 사용하기 위해서는 먼저 작업공간, 저장공간의 구분을 인식해야한다. 그리고 이 공간들은 브랜치에 따라 나뉘어진다.(3개의 건물과 건물 안 층별의 개념이라고해도될까요?)
첫번째, 로컬에서의 작업공간, 로컬 repository, 리모트 repository의 구분
두번째, 로컬에서의 branch들과 원격에서의 branch들의 다름.(그러므로 연동이되어야만 헷갈리지 않을 수 있음)

첫번째 개념을 이해하면 1, 2번을 이해하고
두번째 개념을 이해하면 3번을 이해하고

이들을 이해하면 3가지 다른 공간에서 서로 통신, 그리고 브랜치에서 

## 0. 기본설정
```
  
```



## 1. 로컬 repository 생성
  #### 먼저, repository 생성
```
  git init(작업될 폴더에)
```
  #### 그리고 원격과 통신하도록 원격 url을 origin에 넣어놓기
```
  git remote add origin https://github.com/madihada/funvoca.git
```



## 2. 로컬 branch 생성
```
  git branch <브랜치 이름>  (local branch 생성)
  git checkout <브랜치 이름>  (사용 할 branch 체크아웃)
  git branch -r  (remote branch 목록 보기)
```



## 3. 원격 branch와 연동

  #### 첫째, local branch를 remote branch에 추가
```
  git push origin <브랜치 이름> 
```
  #### 둘째, local branch, remote branch 연동
```
  git branch --set-upstream-to origin/<브랜치 이름> 
```  



## 4. local(컴퓨터)과 local repository 합치기
```
  git status
  git add .
  git commit -m "Title"
```  



## 5. local repository와 remote repository 합치기
```
  git push origin <브랜치 이름> 
```  



## 6. remote repository와 local repository 합치기
다른 개발자가 작업한 remote repository를 pull하기 위해서는 먼저 나의 로컬 repository를 commit해줘야한다!
```
  git status
  git add .
  git commit -m "Title"
  git pull
```  



## 7. remote repository와 remote repository 합치기(브랜치와 브랜치 합치기)
```
  remote repository에서 Branches로 New pull request > Create pull request > Comment > Merge pull request 의 순서로 진행
```  


