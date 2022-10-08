## Django RestFramework

개념
```
rest란, 
첫째, http통신(단발성)
둘째, 자원관리(db 데이터를 permission & authorization을 통해 CRUD할 수 있음)
셋째, client로부터 완전한 독립
```
```
rest server란,
이 세가지 조건이 만족되는 서버를 restful server라고 하고,
```
```
rest api란,
이 세가지 조건이 만족되는 api를 restful api라고 한다.(api는 정보를 안전하게 교환하기 위해 사용하는 인터페이스)
```
장고의 restframework는,
```
rest api라는 설계도로 구축된 server를 만들기 위해서 여러 가지 기능이 있는 라이브러리로 만드는 것. 
그러므로 restframework없이 장고만으로도 rest server를 만들 수 있지만, 이 라이브러리가 제공해주는 기능을 통해서 쉽게 만들 수 있다.
```
그렇다면 어떤 기능이 있을까?
```python
from rest_framework import authentication, generics, mixins, permissions
from rest_framework.decorators import api_view
from rest_framework.response import Response
# from django.http import Http404
from django.shortcuts import get_object_or_404

from .models import Product
from .serializers import ProductSerializer
```
```
첫번재, serializer를 통해서 form과 같이 데이터를 정리해서 받거나 보내줌.
두번째, APIView를 통해서 UI로 데이터를 정리해서 볼 수 있음.
세번째, permission & authorization
```

자 그럼, restful server를 만들기 위해서 
