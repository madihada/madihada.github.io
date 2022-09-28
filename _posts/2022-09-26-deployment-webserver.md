---
layout: single
title:  "Web Server Deployment(Nginx, Soket, uWsigi) 2단계!"
---

이번 블로그는 배포 2단계 과정 중

두번째 단계!

## 1. Nginx 설치 > Nginx를 socket과 연결하기 > 
## 2. uWSGI 설치 > uWSGI를 socket과 연결하기 > uWSGI를 Django와 연결하기 > 
## 3. 모두 연결된 uWSGI를 root에서 실행하기 (uwsgi.ini파일을 통해서) > 
4. emp
<br>

## 0. 개념
```
uWSGI는 
- 장고 runserver처럼 cmd에 실행을 해줘야 함.
- socket과 Django에 중간에서 양쪽으로 연결
- 첫째로, socket과 연결 : /home/ubuntu/<장고프로젝트폴더>/.sock에 연결함
- 둘째로, Django와 연결 : /home/ubuntu/<장고프로젝트폴더>/<장고프로젝트이름>/.wsgi에 연결함
- 코딩이 필요없음
Nginx
- 설치하면 자동으로 root에서 실행됨.
- 포트(client)와 socket에 중간에서 연결
- /etc/nginx/sites-available/.sock에 연결하여 socket과 연결
```
<br>

## 1. Nginx 설치 > Nginx를 socket과 연결하기 >
(vi uwsgi.ini 으로 파일생성)

#### 참고자료
##### [nginx 설정 파일 추가 거의 없도록, static파일 위치 정확히 설정된 방법](https://www.youtube.com/watch?v=oGQ1HteFYnQ&t=1217s&ab_channel=%EB%A9%8B%EC%9F%81%EC%9D%B4%EC%82%AC%EC%9E%90%EC%B2%98%EB%9F%BC%EB%8F%99%EA%B5%AD%EB%8C%80%ED%95%99%EA%B5%90)
##### [nginx 설정 파일 추가가 더 많지만 그 이유와 서버 죽었을 때 reboot되는 설정 추가된 방법](https://www.youtube.com/watch?v=ZpR1W-NWnp4&t=1579s&ab_channel=TonyTeachesTech)
##### [위 방법의 text자료](https://tonyteaches.tech/django-nginx-uwsgi-tutorial/)
<br>

#### (1) 설치
```
sudo apt-get install nginx
```
#### (2) .conf 파일 생성 : /etc/nginx/sites-available/funvocaback.conf
```
# the upstream component nginx needs to connect to
upstream django {
            server unix:///home/ubuntu/funvocaback/funvocaback.sock;
}
# configuration of the server
server {
        listen      80;
        server_name funvoca.duckdns.org www.funvoca.duckdns.org;
        charset     utf-8;
        # max upload size
        client_max_body_size 75M;
        # Django media and static files
        location /media  {
                alias /home/ubuntu/funvocaback/media;
        }
        location /static {
                alias /home/ubuntu/funvocaback/static;
        }
        # Send all non-media requests to the Django server.
        location / {
                uwsgi_pass  django;
                include     /home/ubuntu/funvocaback/uwsgi_params;
        }
}
```

#### (3) uwsgi_params 파일 생성 : /home/ubuntu/funvocaback/uwsgi_params (manage.py파일이 있는 위치)

```
uwsgi_param  QUERY_STRING       $query_string;
uwsgi_param  REQUEST_METHOD     $request_method;
uwsgi_param  CONTENT_TYPE       $content_type;
uwsgi_param  CONTENT_LENGTH     $content_length;
uwsgi_param  REQUEST_URI        $request_uri;
uwsgi_param  PATH_INFO          $document_uri;
uwsgi_param  DOCUMENT_ROOT      $document_root;
uwsgi_param  SERVER_PROTOCOL    $server_protocol;
uwsgi_param  REQUEST_SCHEME     $scheme;
uwsgi_param  HTTPS              $https if_not_empty;
uwsgi_param  REMOTE_ADDR        $remote_addr;
uwsgi_param  REMOTE_PORT        $remote_port;
uwsgi_param  SERVER_PORT        $server_port;
uwsgi_param  SERVER_NAME        $server_name;
```

#### (4) symbolic link 생성
```
sudo ln -s /etc/nginx/sites-available/funvocaback.conf /etc/nginx/sites-enabled/
```
#### (5) settings.py static 경로 추가

##### settings.py파일 수정
```
import os
STATIC_URL = '/static/'
STATIC_ROOT = os.path.join(BASE_DIR, "static/")
```
##### static, media 폴더 생성 
```
python manage.py collectstatic
mkdir media
wget https://upload.wikimedia.org/wikipedia/commons/b/b9/First-google-logo.gif -O media/media.gif
sudo /etc/init.d/nginx restart
```
<br>

## 2. uWSGI 설치 > uWSGI를 socket과 연결하기 > uWSGI를 Django와 연결하기 >
#### (1) uWSGI 설치
```
sudo apt-get install python3.8-dev
sudo apt-get install gcc
pip install uwsgi
```
#### (2) uWSGI 간단사용방법
이처럼 uWSGI는 runserver 처럼 실행을 시켜줘야만 runtime으로 실행 됨. 종료도 동일하게 ctrl+c 누르면 종료.
```
uwsgi --http :8000 --module funvocaback.wsgi
uwsgi --http :8000 --module funvocaback.wsgi
// /home/ubuntu/funvocaback(프로젝트이름)/funvocaback.sock과
// /home/ubuntu/funvocaback(프로젝트이름/mywebserver(앱이름)/mywebserver.wsgi
uwsgi --socket funvocaback.sock --module mywebserver.wsgi --chmod-socket=666
```

## 3. 모두 연결된 uWSGI를 root에서 실행하기 (uwsgi.ini파일을 통해서) > 
vim funvocaback_uwsgi.ini
```
[uwsgi]
# full path to Django project's root directory
chdir            = /home/ubuntu/<funvocaback>/
# Django's wsgi file
module           = <mywebserver>.wsgi
# full path to python virtual env
home             = /home/ubuntu/<funvocaback>/venv
# enable uwsgi master process
master          = true
# maximum number of worker processes
processes       = 10
# the socket (use the full path to be safe
socket          = /home/ubuntu/<funvocaback>/<funvocaback>.sock
# socket permissions
chmod-socket    = 666
# clear environment on exit
vacuum          = true
# daemonize uwsgi and write messages into given log
daemonize       = /home/ubuntu/uwsgi-emperor.log
```
#### uWSGI root에서 실행
```
uwsgi --ini microdomains_uwsgi.ini
```
uWSGI emperor mode로 실행
```
cd /home/ubuntu/venv/
mkdir vassals
sudo ln -s /home/ubuntu/funvocaback/funvocaback_uwsgi.ini /home/udoms/env/md/vassals/
```




## 세번째, uWSGI를 통해 Socket과 장고 연결 
#### manage.py파일 위치
```
uwsgi --socket <funvocaback.sock> --module <mywebserver>.wsgi --chmod-socket=666
```

## 네번째, Nginx와 uWSGI 연결을 설정파일 만들어 root에서 실행(runserver 없이도 runserver!)
#### manage.py파일 위치
#### home/ubuntu/funvocaback/microdomains_uwsgi.ini
```
[uwsgi]
# full path to Django project's root directory
chdir            = /home/ubuntu/<funvocaback>/
# Django's wsgi file
module           = <mywebserver>.wsgi
# full path to python virtual env
home             = /home/ubuntu/<funvocaback>/venv
# enable uwsgi master process
master          = true
# maximum number of worker processes
processes       = 10
# the socket (use the full path to be safe
socket          = /home/ubuntu/<funvocaback>/<funvocaback>.sock
# socket permissions
chmod-socket    = 666
# clear environment on exit
vacuum          = true
# daemonize uwsgi and write messages into given log
daemonize       = /home/ubuntu/uwsgi-emperor.log
```
#### 파일 실행 & 취소
```
uwsgi --ini microdomains_uwsgi.ini
```

## 다섯번째, uWSGI를 통해 Socket과 장고 연결 

#### let’s run uWSGI in emperor mode. 
#### This will monitor the uWSGI config file directory for changes and will spawn vassals (i.e. instances) for each one it finds.
```
cd /home/ubuntu/funvocaback/venv/
mkdir vassals
sudo ln -s /home/ubuntu/funvocaback/funvocaback_uwsgi.ini /home/ubuntu/venv/vassals/
```


## 여섯번째, Start up uWSGI when the system boots!
#### Finally, we want to start up uWSGI when the system boots. 
#### Create a systemd service file at '/etc/systemd/system/emperor.uwsgi.service' with the following content:
```
[Unit]
Description=uwsgi emperor for funvocaback website
After=network.target
[Service]
User=ubuntu
Restart=always
ExecStart=/home/ubuntu/venv/bin/uwsgi --emperor /home/ubuntu/venv/vassals --uid www-data --gid www-data
[Install]
WantedBy=multi-user.target
```

#### Enable the service to allow it to execute on system boot and start it so you can test it without a reboot.
```
systemctl enable emperor.uwsgi.service
systemctl start emperor.uwsgi.service
```
#### Visit http://funvocaback.duckdns.org/webserver a browser and you will see the default Django landing page if everything works correctly.

#### Check the status of the service and stop it as follows:
```
systemctl status emperor.uwsgi.service
systemctl stop emperor.uwsgi.service
```
#### For good measure, reboot your system to make sure that your website is accessible at startup.
















