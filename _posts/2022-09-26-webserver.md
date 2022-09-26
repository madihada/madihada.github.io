---
layout: single
title:  "Web Server (Nginx, Soket, uWsigi)!"
---
```C#

```

# Nginx 사용법

## 1) 기본 사용법

### NGINX Configuration Language Support vscode에 설치
### -부모 자식 프로세스의 상관관계 (부모자식)
```
ps aux --forest
ps aux --forest | grep nginx
```

### -nginx 다시시작
```
systemctl restart nginx
```

### -nginx 다시시작
```
systemctl restart nginx
```

### -nginx host설정
#### 우리의 도메인이 호스트로 적성되어있지 않았기 때문에 hosts파일을 만들어줌
```
vi etc/hosts 
```
#### 아래 내용을 작성
```
# nginx server_name
127.0.0.1 helloworld.com
```

### cmd에 문법오류검사
```
nginx -t
```








## 2) 설정파일 (etc/nginx/nginx.conf 에 위치)

### nginx.conf 안에 모든 설정을 하지 않고 
```
include /etc/nginx/conf.d/*.conf;
```
### 위의 conf.d 디렉토리에 1,2,3 .. 등을 설정하여 사용

3) 서버블록

















