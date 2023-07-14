---
layout: single
title:  "쉐이더 이해하기!"
categories: ['programing', 'git']
---

쉐이더란, 픽셀의 렌더링을 결정하는 프로그램  


  
탄젠트 공간 노말 맵핑(Tangent-space Normal Mapping)  
  알아야 하는 정보 : 
    "TS" n(normal vector), > Fregment Shader가 TS에서 정의된 노말맵에서 가져옴.(문제없음)  
    "TS" r(reflection vector, > Fregment Shader가 즉석에서 계산 예정. "TS" n, l로 계산함. ("TS" l만 있으면 문제 없음)  
    "TS" l(light vector), > "WS"이기 때문에 "TS"로 변환해야함 (계산 필요!!!)  
    "TS" v(viewing vector) > n,l,r 등과 내적하여 렌더링하기 위해서는 "TS" 변환 필요 (계산 필요!!!)  

  "WS" to "TS"  방법 : 
    "TS"의 TBN을 "WS"로 transpose하고, 
    Mat3로 매트릭스로 만들고, 
    이를 l과 r에 곱해주면 각 공간 변환 완료.  


임포트 : 버텍스 어레이에 있는 정보
"WS" 포지션 벡터, UV좌표 
"OS" 노말, 탄젠트 벡터

@궁금한점
버택스 어레이의 정보에서 노말맵의 노말과 탄젠트의 공간은 "OS"인지 "TS"인지 궁금하다.
"OS"인 경우
"TS"인 경우
