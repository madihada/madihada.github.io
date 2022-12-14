---
layout: single
title:  "Web Server Deployment(Nginx, Soket, uWsigi) 1단계!"
categories: ['programing', 'backend', 'aws']
---

이번 블로그는 배포 2단계 과정 중 

첫번째 단계!

EC2접속 > 기본 패키지 설치 > 가상환경 실행 > github > 장고 > runserver 까지!

# 1) EC2 ssh로 접속
##### 먼저 윈도우의 경우 Open SSH 앱이 설치되어있는지 확인
##### 두번째로 AWS ssh로 접속하기 위해서 pem 파일 권한 설정
##### https://techsoda.net/7


# 2-1) 기본 셋팅
##### 
```
sudo apt-get update
sudo apt-get update

// 필수패키지, 파이썬설치, pip 설치
sudo apt-get install build-essential
sudo apt-get install python3
python3 --version
sudo apt-get install python3-pip
sudo pip3 install --upgrade pip
```
##### git clone 할 때 http 아닌 ssh로 통신할 경우 아래 등록
##### 키 복사 후 github.com > settings > Deploy key > Add deploy key
```
ssh-keygen -t rsa                 // 키 생성 & pass 암호 설정
cat /home/ubuntu/.ssh/id_rsa.pub  // 키 복사
```

##### 가상환경 설치 및 장고 패키지 설치
```
git clone <url or ssh>
sudo apt-get install virtualenv 
virtualenv -p python3 venv
source venv/bin/activate
```
```
pip install -r requirements.txt
python manage.py runserver 0.0.0.0:8000
```
##### requirements.txt 실행 안 될 때
##### Ubuntu mysqlclient 설치
```
sudo apt-get install python3-dev default-libmysqlclient-dev build-essential
pip install mysqlclient
```


배포 2단계는 링크 참조!
[배포 2단계 가기](https://madihada.github.io/deployment-webserver/)
