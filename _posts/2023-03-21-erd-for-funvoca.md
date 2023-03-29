>목차
- [1. Mutlivalued Attribute는 어떻게 테이블을 작성해?](#1-mutlivalued-attribute는-어떻게-테이블을-작성해)
- [2. mysql에서 optionality의 'required', 'optional'은 테이블에 코드로 어떻게 표현돼?](#2-mysql에서-optionality의-required-optional은-테이블에-코드로-어떻게-표현돼)
- [3. 'required', 'optional'의 차이는 뭐야? 즉, FK에 null이 있고 없고 차이가 뭐야?](#3-required-optional의-차이는-뭐야-즉-fk에-null이-있고-없고-차이가-뭐야)
- [4. 그러면 optionality의 'required', 'optional'과 CASCADE는 어떤 차이야?](#4-그러면-optionality의-required-optional과-cascade는-어떤-차이야)
- [5. one to one, one to many, many to many 테이블 작성 방법은?.](#5-one-to-one-one-to-many-many-to-many-테이블-작성-방법은)
- [6. N:M은 1:N으로 변경은 어떻게해?.](#6-nm은-1n으로-변경은-어떻게해)
- [7. 1:1 관계에서 Primary Key로 Foreign Key로 쓰면 auto ingrement없이 좋은 성능으로 사용 가능!](#7-11-관계에서-primary-key로-foreign-key로-쓰면-auto-ingrement없이-좋은-성능으로-사용-가능)
- [8. 제1 정규화 : 복수의 데이터 제거](#8-제1-정규화--복수의-데이터-제거)
  - [8.1. 복수의 데이터가 있는 attribute를 entity로 만들어줌.](#81-복수의-데이터가-있는-attribute를-entity로-만들어줌)
  - [8.2. 이 때, N:M인지 확인 -\> 맞다면 -\> 중간 테이블 생성.](#82-이-때-nm인지-확인---맞다면---중간-테이블-생성)
- [9. 제2 정규화 : 테이블 안 attribute의 데이터의 중복 제거](#9-제2-정규화--테이블-안-attribute의-데이터의-중복-제거)
- [10. 제3 정규화 :  테이블 안 attribute 중복 제거](#10-제3-정규화---테이블-안-attribute-중복-제거)
- [11. 개념적 다이어그램 작성](#11-개념적-다이어그램-작성)
- [12. 제1,2,3 정규화를 통해 Join Table 유 무 판단](#12-제123-정규화를-통해-join-table-유-무-판단)
  - [12.1. Join Table 생성 조건이 아니라면](#121-join-table-생성-조건이-아니라면)
  - [12.2. Join Table 생성 조건이라면](#122-join-table-생성-조건이라면)
- [13. Cardinality Optionality 결정은 위에서 끝났고, 참조 무결성 원칙을 위해 CASCADE 결정](#13-cardinality-optionality-결정은-위에서-끝났고-참조-무결성-원칙을-위해-cascade-결정)




># 1. ERD 질문
## 1. Mutlivalued Attribute는 어떻게 테이블을 작성해?
결론 : 한 사람이 여러개의 전화번호를 가질 수 있기때문에 attribute를 entity로 변경하여 테이블을 추가한다.    
  
첫 번째, 테이블 1개 추가 : attribute가 1개 일 때  
두 번째, 테이블 2개 추가(조인테이블) : attribute가 2개 이상일 때  
```sql
-- 사람 테이블 생성
CREATE TABLE person (
  person_id INT PRIMARY KEY,
  name VARCHAR(50)
);

-- 전화번호 테이블 생성
CREATE TABLE phone_number (
  phone_number_id INT PRIMARY KEY,
  phone_number VARCHAR(20),
  person_id INT,
  FOREIGN KEY (person_id) REFERENCES person(person_id)
);
```
<br>
 
## 2. mysql에서 optionality의 'required', 'optional'은 테이블에 코드로 어떻게 표현돼?  
결론 : 1:N에서 N이 되는 테이블에 FK에 null 혹은 not null로 표현됨.   
optionality는 테이블 간의 관계를 정의할 때 사용되는 외래 키(Foreign Key) 제약 조건을 통해 나타낸다.  
```sql
// 예를 들어, 다음과 같은 두 개의 테이블이 있다고 가정해보겠습니다.

CREATE TABLE orders (
  order_id INT PRIMARY KEY,
  customer_id INT,
  FOREIGN KEY (customer_id) REFERENCES customers(customer_id)
);

CREATE TABLE customers (
  customer_id INT PRIMARY KEY,
  customer_name VARCHAR(50)
);

// FK가 NULLABLE하게되면 optionality가 'optional' 그렇지 않으면 'required'
CREATE TABLE orders (
  order_id INT PRIMARY KEY,
  customer_id INT NULL,
  FOREIGN KEY (customer_id) REFERENCES customers(customer_id)
);
```
<br>

## 3. 'required', 'optional'의 차이는 뭐야? 즉, FK에 null이 있고 없고 차이가 뭐야?  
결론 : 참조 무결성을 유지하기 위해서, 테이블 관계가 의존인지 독립인지 나타냄.
```sql
먼저 일반 attribute에 null과 not null의 차이는 값을 넣어도 되느냐 마느냐차이
FK attribute에 null과 not null의 차이는 같을텐데...
만약 넣지 않아도 된다면 없어도 된다는 것, 테이블관계가 독립이냐 의존이냐 차이.

```
<br>

  
## 4. 그러면 optionality의 'required', 'optional'과 CASCADE는 어떤 차이야?
결론 : 참조 무결성을 유지하기 위해서, 테이블들 안의 Column값이 똑같이 업데이트 되는지를 나타냄.

MySQL에서 optionality와 CASCADE는 외래 키(Foreign Key) 제약 조건을 함께 사용하여 해당 열(Column)의 NULL 값을 허용 여부와 외래 키 참조 무결성(Integrity) 유지 여부를 결정합니다.  
```sql
// CASCADE란?
관계형 데이터베이스에서 부모 엔티티의 레코드가 삭제되거나 업데이트될 때 자식 엔티티의 레코드를 자동으로 업데이트 또는 삭제하는 기능을 제공하는 데 사용됩니다. 이는 외래 키(Foreign Key) 관계에서 많이 사용됨.

// CASCADE는 코드(FK에 붙는다)
CREATE TABLE orders (
  order_id INT PRIMARY KEY,
  customer_id INT,
  FOREIGN KEY (customer_id) REFERENCES customers(customer_id) ON DELETE CASCADE
);

CREATE TABLE customers (
  customer_id INT PRIMARY KEY,
  customer_name VARCHAR(50)
);

```
<br>


## 5. one to one, one to many, many to many 테이블 작성 방법은?.
  1:1은 FK없음  
  1:N은 FK있음  
  N:M은 1:N으로 변경  
  <br>

## 6. N:M은 1:N으로 변경은 어떻게해?.

중간테이블 작성 : FK로 두 테이블의 PK 가져옴  
Cardinality : 개념적 다이어그램 그대로  
Optionality : 개념적 다이어그램 그대로  


## 7. 1:1 관계에서 Primary Key로 Foreign Key로 쓰면 auto ingrement없이 좋은 성능으로 사용 가능!
  <br>

## 8. 제1 정규화 : 복수의 데이터 제거
### 8.1. 복수의 데이터가 있는 attribute를 entity로 만들어줌.
### 8.2. 이 때, N:M인지 확인 -> 맞다면 -> 중간 테이블 생성.

그런데 왜 중간 테이블을 만들지? 없으면 테이블 1개 추가로 끝나 효율적일텐데
가능성1. null 값ㅇ


## 9. 제2 정규화 : 테이블 안 attribute의 데이터의 중복 제거
  <br>

## 10. 제3 정규화 :  테이블 안 attribute 중복 제거
  <br>

># 2. ERD 사용법
  <br>
  
## 11. 개념적 다이어그램 작성
  <br>
  
## 12. 제1,2,3 정규화를 통해 Join Table 유 무 판단
  <br>

###  12.1. Join Table 생성 조건이 아니라면
  - 관계 없음 : 각 테이블에 PK 있음, FK 없음.  
  - 1:1 관계 : 의존 테이블에 PK가 FK로 옴. FK 1개  (예시, '단어'&'의미'테이블)
  - 1:N 관계 & attribute 1개 : Join 테이블에 PK가 FK로 옴. FK 1개 
  <br>

### 12.2. Join Table 생성 조건이라면 
  - 1:N 관계 & attribute 복수 : 의존 테이블에 PK있음, FK로 각 테이블의 PK가 옴. FK 2개  
  - N:M 관계는 1:N으로 변경: 의존 테이블에 PK있음, FK로 각 테이블의 PK가 옴. FK 2개  
    - Join table 생성 시 3개의 entity를 연결해주는 4개의 관계가 있음(2개에서 4개가 됨)
    - (1) table -> join table : 관계가 그대로 옴.
    - (2) join table -> table : 무조건 1:1관계에 required이다. 왜냐하면 "join테이블의 역할이 N:M을 1:N으로 변경"하고 "join table이 있다면 table이 있어야만 함"
![image](https://user-images.githubusercontent.com/44697751/228404480-53d45bd1-d5d6-435a-8536-5e518ad4075d.png)
  <br>
  
## 13. Cardinality Optionality 결정은 위에서 끝났고, 참조 무결성 원칙을 위해 CASCADE 결정
