---
layout: single
title:  "Web Server (Nginx, Soket, uWsigi)!"
---
# 배포방법
 
## 개념 :   

## 첫번째, uWSGI를 설치하여 Socket으로 Django와 통신한다. 
#### (0) 코딩 없음, uWSGI를 wsgi.py 파일(settings.py가 있는 폴더에 위치함)과 연결하여 Django와 통신.
#### (1) uWSGI 설치
```
sudo apt-get install python3.8-dev
sudo apt-get install gcc
pip install uwsgi
```
#### (2) uWSGI 사용방법
이처럼 uWSGI는 runserver 처럼 실행을 시켜줘야만 runtime으로 실행 됨. 종료도 동일하게 ctrl+c 누르면 종료.
```
uwsgi --http :8000 --module funvocaback.wsgi
```

## 두번째, Nginx 설치 
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

#### (3) uwsgi_params 파일 생성 : /home/ubuntu/funvocaback/uwsgi_params
#### manage.py파일 위치

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

settings.py파일 수정
```
import os
STATIC_URL = '/static/'
STATIC_ROOT = os.path.join(BASE_DIR, "static/")
```
cmd에 명령어 입력
```
python manage.py collectstatic
sudo /etc/init.d/nginx restart
mkdir media
wget https://upload.wikimedia.org/wikipedia/commons/b/b9/First-google-logo.gif -O media/media.gif
```

## 세번째, Nginx와 uWSGI 연결 
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
chdir            = /home/udoms/microdomains/
# Django's wsgi file
module           = microdomains.wsgi
# full path to python virtual env
home             = /home/udoms/env/md
# enable uwsgi master process
master          = true
# maximum number of worker processes
processes       = 10
# the socket (use the full path to be safe
socket          = /home/udoms/microdomains/microdomains.sock
# socket permissions
chmod-socket    = 666
# clear environment on exit
vacuum          = true
# daemonize uwsgi and write messages into given log
daemonize       = /home/udoms/uwsgi-emperor.log
```








### -nginx 다시시작
```
systemctl restart nginx
```
### cmd에 문법오류검사
```
nginx -t
```















