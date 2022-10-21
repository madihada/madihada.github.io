# 장고 rest api 로그인 구현

##### 유니티에서 로그인 구현을 위해서는,  
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
