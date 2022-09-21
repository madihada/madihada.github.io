---
layout: single
title:  "Github 사용방법!"
---

먼저, 깃은 3개의 공간과, 브랜치라는 차원으로 나뉘어지고 특정 공간과 특정 차원의 원활한 통신방법이 핵심이다.

첫번째, 로컬에서의 작업공간, 로컬 repository, 리모트 repository라는 3개의 공간이 있다. (commit push pull 등으로 상호 통신함)
두번째, 브랜치라는 차원들이 있다.

원활한 통신을 위해선, 첫째로 브랜치라는 차원을 통일시켜줘야한다. 그렇지 않으면 너무 복잡해지기때문에 통신이 어려워짐.
로컬과 원격에서 똑같은 이름의 브랜치를 같이 만들고, sync해주고, 삭제를 같이한다.

이들을 이해하면 3가지의 다른 공간과 공간안에서 공간 간 서로 통신을 가능할 것이다. 

## 0. 기본설정
윈도우의 경우 cmder을 설치하여 리눅스/맥과 같은 명령어를 사용할 수 있도록한다.
sourceTree설치하여 GUI도 볼 수 있도록한다.
vscode 터미널을 git bash로 설정
 

```cs
  git config --global user.name "madihada"
  git config --global user.email "tlarbgus89@gmail.com" (깃헙로그인메일주소)
```
```
  git config --global init.defaultBranch main
```
```
  git config --global core.autocrlf true(윈도우사용자) (맥 사용자는 true 대신 input)
```
```
 git config --global alias.co checkout
 git config --global alias.br branch
 git config --global alias.ci commit
 git config --global alias.st status
 git config --global alias.l "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr)%C(bold blue)<%an>%Creset' --abbrev-commit"
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
  git branch -r  (remote branch 목록 보기)
  git switch <브랜치 이름>  (사용 할 branch 체크아웃)
  git switch -c <브랜치 이름>  (branch 생성 이동)
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

## 4. local repository와 local repository 합치기

#### Merge
```
  git switch <두꺼워질 브랜치명>
  git merge <없어질 브랜치명>
  git branch -d <없어질 브랜치명>
```  
#### Rebase
```git
  git switch <없어질 브랜치명>
  git rebase <두꺼워질 브랜치명>
  요기서 두꺼워질 브랜치가 커밋 behind되기때문에 맨 위로 올려줘야한다 그래서 머지함.
  git merge <없어질 브랜치명>
  git branch -d <없어질 브랜치명>
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
  git pull origin <브랜치 이름> 
```  



## 7. remote repository와 remote repository 합치기(브랜치와 브랜치 합치기)
```
  remote repository에서 Branches로 New pull request > Create pull request > Comment > Merge pull request 의 순서로 진행
```  

## 8. 시간을 과거로 되돌리기 (Reset and Revert)

#### Reset (전으로 모든걸 다 돌리는것)
```
  git log
  돌아가려는 커밋 주소 복사
  git reset --hard <커밋주소>
  git reset --hard (이렇게하면 직전으로 돌아감)
``` 
#### Revert (전 커밋으로 되돌아가면서 커밋 생성하는것(지우는것없음))
```
  git log
  돌아가려는 커밋 주소 복사
  git revert <커밋주소>
  git rm <REVERTING되고있는 파일명들>
  git revert --continue
```

## 8. 시간을 과거로 되돌리기 (Reset and Revert)
