# 데이터 베이스 구축

## 목차 
- ERM


## ERM을 ERMaster로 만들기

1정규형 Atomic (1개에 키에는 1개 데이터만)
2정규형 No partial dependencies (중복키) (부분 종속성 제거)
3정규형 No transitive dependencies (No 이행적 종속성)

정리하면, 1정규형 = N:M관계에서 1키에 여러 값을 넣어야하는 경우가 생기는데, 이를 수용 할 수 없음
2정규형 = 중복되는 행 이 없도록 테이블을 쪼개 필요한 데이터만 보일 수 있도록
3정규형 = 관계있는 열끼리 따로 테이블을 쪼개 필요한 데이터만 보일 수 있도록


성능향상
find slow query

index
cach 
