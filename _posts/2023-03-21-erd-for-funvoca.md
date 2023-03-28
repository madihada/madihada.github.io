>목차
- [1. ERD 질문](#1-erd-질문)
  - [1.1. Mutlivalued Attribute는 어떻게 테이블을 작성해?](#11-mutlivalued-attribute는-어떻게-테이블을-작성해)
  - [1.2. mysql에서 optionality의 'required', 'optional'은 테이블에 코드로 어떻게 표현돼?](#12-mysql에서-optionality의-required-optional은-테이블에-코드로-어떻게-표현돼)
  - [1.3. 'required', 'optional'의 차이는 뭐야? 즉, FK에 null이 있고 없고 차이가 뭐야?](#13-required-optional의-차이는-뭐야-즉-fk에-null이-있고-없고-차이가-뭐야)
  - [1.4. 그러면 optionality의 'required', 'optional'과 CASCADE는 어떤 차이야?](#14-그러면-optionality의-required-optional과-cascade는-어떤-차이야)
  - [1.5. one to one, one to many, many to many 테이블 작성 방법은?.](#15-one-to-one-one-to-many-many-to-many-테이블-작성-방법은)
  - [1.6. N:M은 1:N으로 변경은 어떻게해?.](#16-nm은-1n으로-변경은-어떻게해)


# 1. ERD 질문
## 1.1. Mutlivalued Attribute는 어떻게 테이블을 작성해?
결론 : 한 사람이 여러개의 전화번호를 가질 수 있기때문에 attribute를 entity로 변경하여 테이블을 추가한다.  
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
 
## 1.2. mysql에서 optionality의 'required', 'optional'은 테이블에 코드로 어떻게 표현돼?  
결론 :  optionality는 테이블 간의 관계를 정의할 때 사용되는 외래 키(Foreign Key) 제약 조건을 통해 나타낸다.
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

## 1.3. 'required', 'optional'의 차이는 뭐야? 즉, FK에 null이 있고 없고 차이가 뭐야?  
결론 : 참조 무결성을 유지하기 위해서, 테이블 관계가 의존인지 독립인지 나타냄.
```sql
먼저 일반 attribute에 null과 not null의 차이는 값을 넣어도 되느냐 마느냐차이
FK attribute에 null과 not null의 차이는 같을텐데...
만약 넣지 않아도 된다면 없어도 된다는 것, 테이블관계가 독립이냐 의존이냐 차이.

```
<br>

  
## 1.4. 그러면 optionality의 'required', 'optional'과 CASCADE는 어떤 차이야?
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


## 1.5. one to one, one to many, many to many 테이블 작성 방법은?.
  1:1은 FK없음  
  1:N은 FK있음  
  N:M은 1:N으로 변경  
  <br>

## 1.6. N:M은 1:N으로 변경은 어떻게해?.

중간테이블 작성 : FK로 두 테이블의 PK 가져옴  
Cardinality : 개념적 다이어그램 그대로  
Optionality : 개념적 다이어그램 그대로  
