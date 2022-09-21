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
```
  git pull
```  


