이번 포스팅에서는 Authorization and Permission 에 대해서 자세히 알아보도록 하겠습니다
  

## Authorization and Permission(인증 및 권한)
<br/>

```python
# views.py

from rest_framework import generics, mixins, permissions

class ProductListCreateAPIView(generics.ListCreateAPIView):
  permission_classes = [permissions.IsAuthenticatedOrReadOnly]
  
  //AllowAny, IsAuthenticated, IsAdminUser, IsAuthenticatedOrReadOnly, DjangoModelPermissions, DjangoObjectPermissions, CustomPermission 등의 권한 요구 할 수 있음
```
<br/>

먼저 permission_classes를 설정하면, IsAuthenticated되었는지 인증 요구함. 그래서 authentication_classes 넣어준다.
```python
# views.py

from rest_framework import generics, mixins, permissions, authentication

class ProductListCreateAPIView(generics.ListCreateAPIView):
  authentication_classes = [authentication.SessionAuthentication]
  permission_classes = [permissions.IsAuthenticatedOrReadOnly]
```
<br/>

SessionAuthentication이란?
SessionAuthentication으로 어떻게 인증 받을 수 있을까?
첫번째, 슈퍼유저가 권한을 줘야함
두번째, 유저가 client에서 로그인을 해야함. 그런데 http 통신은 단발성 통신으로 request마다 로그인 필요함. 그래서 유효기간이 정해져있는 토큰을 사용함.
세번째, 토큰
<br/>

### CustomPermission을 설정하기
1. Cumtomer Permission 설정 또는 이를 Groups로 설정
- admin > Users > User permissions > 추가
- admin > Groups > 추가
