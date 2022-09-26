
### 먼저 윈도우의 경우 
Open SSH 앱이 설치되어있는지 확인
### 두번째로 AWS ssh로 접속하기 위해서 pem 파일 권한 설정
https://techsoda.net/7

//깃 설치
sudo apt-get update
git init
git clone <주소>

//가상환경설치
pip install virtualenv
python3 -m venv /home/ubuntu/venv/md
source venv/md/bin/activate

//장고설치
pip install django==2.2

//mysql설치
sudo apt-get install mysql-server
sudo apt-get install python3-pip
sudo apt-get install --upgrade pip
//
ssh_keygen -t rsa
cat /hime/ubuntu/.ssh/id_rsa.pub
키 복사에서 git > settings > Deploy key > Add deploy key

git clone

sudo apt-get install python3.10-dev
sudo apt-get install gcc
pip install uwsgi

sudo apt-get update
sudo apt-get install build-essential


### Ubuntu mysqlclient 설치
sudo apt-get install python3-dev default-libmysqlclient-dev build-essential
pip install mysqlclient
