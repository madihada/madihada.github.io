# Unity <-> Django 로그인 만들기!

## 1.Django에서 로그인 구현
## 2.회원가입 (POST)
## 3.로그인 (Authentication -> GET) 


---

## 1.Django에서 로그인 구현
- 장고의 내장 User 모델을 AbstractUser 사용하여 상속
```python
# settings.py

DB변경(sqlight -> mysql)

AUTH_USER_MODEL = 'appname.User'

// 이 설정을 하고 첫번째 migrate해주야합니다 꼭!
```

- 모델 생성
```python
# models.py

from django.contrib.auth.models import AbstractUser

class User(AbstractUser):
    name = models.CharField(max_length=255)
    email = models.CharField(max_length=255, unique=True)
    password = models.CharField(max_length=255)
    username = models.CharField(max_length=255, null=True)
    
    def __str__(self):
        return self.email
```

- url연결
```python
# project urls.py

urlpatterns = [
    path('admin/', admin.site.urls),
    path('accounts/', include('allauth.urls')),
    path('webserver/', include('webserver.urls')),
]

# app urls.py

from django.contrib import admin
from django.urls import path, include
from . import views
from .views import RegisterView, LoginView

urlpatterns = [
    path('', views.apiOverview, name="api-overview"),

    path('register', RegisterView.as_view(), name="register"),
    path('login', LoginView.as_view(), name="login"),
]

```
- views 연결
```python
# views.py

from rest_framework.views import APIView
from .serializers import CustomerSerializer, WordSerializer, UserSerializer
from rest_framework.exceptions import AuthenticationFailed
import jwt, datetime

class RegisterView(APIView):
    def post(self, request):
        serializer = UserSerializer(data=request.data)
        serializer.is_valid(raise_exception=True)
        serializer.save()
        return Response(serializer.data)
        
class LoginView(APIView):
    def post(self, request):
        email = request.data['email']
        password = request.data['password']

        user = User.objects.filter(email=email).first()

        if user is None:
            raise AuthenticationFailed('User not found!')

        if not user.check_password(password):
            raise AuthenticationFailed('Incorrect password!')

        payload = {
            'id': user.id,
            'exp': datetime.datetime.utcnow() + datetime.timedelta(minutes=60),
            'iat': datetime.datetime.utcnow()
        }

        token = jwt.encode(payload, 'secret', algorithm='HS256').decode('utf-8')

        response = Response()

        response.set_cookie(key='jwt', value=token, httponly=True)
        response.data = {
            'jwt': token
        }
        return response
        
        //중요! pip install PyJwt==1.7.1  높은 버전은 에러밣생
```
- serializer 연결
```python 
# serializers.py

from dataclasses import field
from rest_framework import serializers
from .models import Customer, Word, User

class UserSerializer(serializers.ModelSerializer):
    class Meta:
        model = User
        fields = ['id', 'name', 'email', 'password']
        extra_kwargs = {
            'password': {'write_only': True}
        }

    def create(self, validated_data):
        password = validated_data.pop('password', None)
        instance = self.Meta.model(**validated_data)
        if password is not None:
            instance.set_password(password)
        instance.save()
        return instance
```
- 쿠키로 토큰 저장
```
pip install django-cors-headers
```
- settings 설정
```python
# settings.py

INSTALLED_APPS = [
    'corsheaders',
]
MIDDLEWARE = [
    'corsheaders.middleware.CorsMiddleware',
]
CORS_ORIGIN_ALLOW_ALL = True
CORS_ALLOW_CREDENTIALS = True
```




## 유니티에서 로그인 구현을 위해서는,  
---

첫번째, api signup 설정
##### - C#에서 HTTP통신으로 url에 json파일 전송 >  
##### - url에 연결된 views 함수 호출 > 
##### - views에서 Userserializer연결 데이터 저장(만약 양식이 맞다면) 
##### - < serializer에서 양식을 정해주고 이 양식은 model로 연결하여 가져옴 
##### - < model은 abstractuser을 사용해서 장고 자체에서 제공하는 user에 접근

[api signup 설정](https://www.youtube.com/watch?v=PUzgZrS_piQ&t=331s&ab_channel=ScalableScripts)

---

두번째, api signup과 allauth signup 연결  
##### ~~~
[api signup과 allauth signup 연결방법](https://learndjango.com/tutorials/django-log-in-email-not-username)

---
