---
title: SQL 나 혼자 퀴즈내고 나 혼자 맞추기
description: DDL, DML, DCL 기본 명령어 재정비
author: subeenjeon
date: 2024-04-14
categories: [CS, Database]
tags: [ Database ]
pin: true
math: true
mermaid: true
#image:
#  path: /commons/devices-mockup.png
#  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
#  alt: Responsive rendering of Chirpy theme on multiple devices.
---

## Overview

프로그래머스로 풀고는 있으나 약간 불안 불안해서 기초 한 번 더 짚고 넘어가기

<!-- TOC -->
* [Overview](#overview)
* [2024년 4월 14일 SQL DCL, DDL, DML 명령어 정리](#2024년-4월-14일-sql-dcl-ddl-dml-명령어-정리)
* [내가 틀린 것](#내가-틀린-것)
* [뷰](#뷰)
* [인덱스](#인덱스)
* [DDL(Data Definition Language) 명령어 예시](#ddldata-definition-language-명령어-예시)
* [DCL(Data Control Language) 명령어 예시](#dcldata-control-language-명령어-예시)
* [DML(Data Manipulation Language)](#dmldata-manipulation-language)
* [LIKE 연산자](#like-연산자)

<!-- TOC -->

---

### 이름이 "John"으로 시작하는 고객들의 정보를 모두 조회하는 SQL 쿼리를 작성해보세요.

```sql
SELECT 이름, 나이, 번호
FROM 고객
WHERE 이름 LIKE 'John%';
// 작은 따옴표 사용
```

### 주문 테이블에서 주문 일자가 2024년 4월 1일부터 4월 14일까지인 주문들의 정보를 조회하는 SQL 쿼리를 작성해보세요.

```sql
SELECT 주문번호, 주문일자, 주문상품
FROM 주문
WHERE 주문일자 >= '2024-04-01'
  AND 주문일자 <= '2024-04-14';

//WHERE 주문일자 BETwEEN DATE(2024,04,01) AND DATE(2024,04,14);
```

### 새로운 고객의 정보를 추가하는 SQL 쿼리를 작성해보세요. (고객 테이블에는 고객의 이름과 이메일 주소가 저장되어 있습니다.)

```sql
INSERT INTO 고객(이메일, 이름)
VALUES ('abc@123', '회원1);
```

### 주문 테이블에서 상품 ID가 1001인 상품의 가격을 5000원으로 수정하는 SQL 쿼리를 작성해보세요.

```sql
UPDATE 주문
SET 상품가격 = 5000 // I made a mistake by putting ';' here.
WHERE 상품ID=1001;
```

### 주문 테이블에서 주문 일자가 2024년 4월 10일인 주문들을 모두 삭제하는 SQL 쿼리를 작성해보세요.

```sql
DELETE
FROM 주문
WHERE 주문일자 = '2024-04-10';

//WHERE 주문일자 >= '2024-04-01' AND 주문일자 <= '2024-04-10'; 아까
틀렸던 것을 여기에 복기
```

### 주문 테이블에서 각 고객이 최근에 한 번 이상 주문한 상품들의 평균 가격을 조회하는 SQL 쿼리를 작성해보세요.

1. 주문 수량이 **1 이상인** 상품에 대한 평균 가격을 계산한다.
2. WHERE 절에서 주문 수량이 **1 이상인 것만을 선택해야** 한다.

```sql
// 상품 기준 GROUP BY 조건 최근 주문일자 NOT NULL? , 주문 수량 >=1?
//
SELECT AVG(가격) AS 평균가격 //
FROM 주문
  //
GROUP BY 상품
  //
WHERE 구매수량>=1;

// GROUP BY, WHERE 순서 틀렸다. WHERE 다음에 GROUP BY가 와야함
SELECT AVG(가격) AS 평균가격
FROM 주문
WHERE 구매수량 >= 1
GROUP BY 상품
           //
ORDER BY 
```

### 고객 테이블에서 각 고객이 가장 많이 주문한 상품의 카테고리를 조회하는 SQL 쿼리를 작성해보세요.

Let's try using a subquery.

```java
// 상품 카테고리별로 GROUP BY하고 , 상품 수량에 따라 DESC, 하나만 조회한다면 LIMIT 1
SELECT 카테고리
FROM 고객
GROUP BY
상품 카테고리
ORDER BY
수량 DESC
LIMIT 1;
```

```sql
SELECT 고객ID, 카테고리
FROM (SELECT 고객ID, 카테고리, 주문수량 -- 주문 테이블 기준 조회할 열
      FROM 주문
      WHERE (고객ID, 주문수량) IN (SELECT 고객ID, MAX(주문수량)
                             FROM 주문
                             GROUP BY 고객ID -- 고객 ID별로 그룹핑 후, 각 고객별로 주문 수량이 가장 큰 것 
      ))
       AS 최다주문
```

### 학생 중에서 성적이 80점 이상인 학생의 수를 찾는다.

```sql
SELECT COUNT(*)
FROM 학생
WHERE 성적 >= 80ㅣ
```

### 고객 테이블에서 가장 최근에 가입한 고객의 정보를 조회하는 SQL 쿼리를 작성해보세요.

```sql
SELECT 고객이름, 이메일, 가입일자
FROM 고객
ORDER BY 가입일자 ASC LIMIT 1; -- 오름차순 정렬 후, 첫 번째 행만을 반환
```

### 주문 테이블에서 주문 금액이 가장 높은 상위 10개의 주문 정보를 조회하는 SQL 쿼리를 작성해보세요.

```sql
SELECT 주문id, 주문금액
FROM 주문
ORDER BY 주문금액 DESC LIMIT 10;
```

### 주문 테이블에서 상품별 판매량이 가장 높은 상위 3개의 상품과 그에 해당하는 총 판매량을 조회하는 SQL 쿼리를 작성해보세요.

```sql
SELECT 상품이름, SUM(판매량) AS 총판매량
FROM 주문
GROUP BY 상품이름
ORDER BY) 총판매량 DESC
  LIMIT 3;
```

### 새로운 학생 정보를 저장하는 "students" 테이블을 생성해보세요. 테이블은 다음과 같은 컬럼을 포함해야 합니다:

- 학생ID (정수, 기본키)
- 이름 (문자열)
- 나이 (정수)
- 성별 (문자열)

```sql
// 실수 - 순서를 자바 필드하듯이 선언하듯이 ...
CREATE TABLE students (
		학생ID INT PRIMARY KEY,
//	INT 학생ID PRIMARY KEY,
	이름 VARCHAR(50),
	나이 INT,
	성별 VARCHAR(10)
);
```

### "students" 테이블에 전화번호를 저장하는 "phone_number" 컬럼을 추가하는 쿼리문을 작성해보세요.

```sql
ALTER TABLE students
  ADD phone_number VARCHAR(40);

// 컬럼 타입 변경하려면
ALTER TABLE students
ALTER
COLUMN phone_number INT;
```

### "students" 테이블에서 성별 정보를 저장하는 "gender" 컬럼을 삭제하는 쿼리문을 작성해보세요.

```sql
ALTER TABLE students
DROP
COLUMN gender;
```

### "students" 테이블에서 나이 정보를 저장하는 "age" 컬럼의 데이터 타입을 문자열로 변경하는 쿼리문을 작성해보세요.

```sql
//
ALTER TABLE students
  //
ALTER
COLUMN age VARCHAR(10);

ALTER TABLE students
  ADD COLUMN new_age VARCHAR(10);

UPDATE students
SET new_age = CAST(age AS VARCHAR(10));

ALTER TABLE students
DROP
COLUMN age;

ALTER TABLE students
  RENAME COLUMN new_age TO age;
```

---

# 2024년 4월 14일 SQL DCL, DDL, DML 명령어 정리

# 내가 틀린 것

- [ ]  인덱스 생성
- [ ]  뷰

---

## 뷰

### CREATE VIEW Statement

```sql
CREATE VIEW view_name AS
SELECT column1, column2,
  ...
  FROM table_name
  WHERE condition;
```

### 뷰 조회

```sql
SELECT *
FROM view_name;
```

### 뷰 수정

```sql
CREATE
OR REPLACE VIEW view_name AS
SELECT column1,
       column2, ...
  FROM table_name
WHERE condition;
```

### 뷰 삭제

```sql
DROP VIEW view_name;
```

---

## 인덱스

### **인덱스 생성**

```sql
CREATE INDEX index_name
  ON table_name (column1, column2, . . .);
```

### **인덱스 조회**

```sql
SHOW
INDEX FROM table_name;
```

### **인덱스 수정**

인덱스의 수정은 지원하지 않으므로, 삭제 후 다시 생성해야 한다.

### **인덱스 삭제**

```sql
DROP INDEX index_name ON table_name;
```

---

## DDL(Data Definition Language) 명령어 예시

DDL은 DB 스키마 및 설명에 사용되는 SQL 명령어

종류로는 **CREATE, ALTER, DROP**이 있다.

### CREATE:

새로운 데이터베이스, 테이블, 인덱스, 또는 뷰를 생성한다.

```sql
CREATE
DATABASE database_name;  -- 데이터베이스 생성

CREATE TABLE table_name
( -- 테이블 생성
  column1 datatype,
  column2 datatype,
  column3 datatype, .
  .
  .
  .
);

CREATE INDEX index_name -- 인덱스 생성
  ON table_name (column_name);
```

### ALTER:

데이터베이스, 테이블, 또는 인덱스를 변경한다.

```sql
ALTER TABLE table_name -- 테이블 변경
  ADD column_name datatype;

ALTER TABLE table_name -- 컬럼 타입 변경
ALTER
COLUMN column_name datatype;
```

### DROP:

데이터베이스, 테이블, 또는 인덱스를 삭제합니다.

```sql
DROP
DATABASE database_name;    -- 데이터베이스 삭제

DROP TABLE table_name; -- 테이블 삭제

DROP INDEX index_name; -- 인덱스 삭제
```

---

## DCL(Data Control Language) 명령어 예시

DCL은 데이터 베이스에 대한 접근을 제어하는 명령어로, **GRANT, REVOKE** 등이 있다.

### GRANT:

사용자에게 특정 작업의 수행을 허용하도록 권한을 부여

```sql
GRANT
privilege_name -- 권한 부여
ON object_name
TO {user_name | PUBLIC | role_name}
[WITH GRANT OPTION];
```

### REVOKE:

사용자로부터 특정 작업 수행 권한을 제거

```sql
REVOKE privilege_name -- 권한 제거
  ON object_name
  FROM {user_name | PUBLIC | role_name};
```

---

## DML(Data Manipulation Language)

DML은 데이터베이스 내의 데이터를 조작하는 SQL 명령어로 **SELECT, INSERT, UPDATE, DELETE** 등이 있다.

### SELECT:

데이터베이스 내의 하나 이상의 테이블에서 데이터를 조회

```sql
SELECT column1,
       column2, ... -- 데이터 조회
  FROM table_name
WHERE condition;
```

### INSERT:

테이블에 새로운 데이터를 추가

```sql
INSERT INTO table_name -- 데이터 추가
  (column1, column2, column3, ...)
VALUES (value1, value2, value3, ...);
```

### UPDATE:

테이블의 데이터를 수정

```sql
UPDATE table_name -- 데이터 수정
SET column1 = value1,
    column2 = value2, ...
  WHERE condition;
```

### DELETE:

테이블의 데이터를 삭제

```sql
DELETE
FROM table_name -- 데이터 삭제
WHERE condition;
```

---

## LIKE 연산자

LIKE operator은 특정한 조건에 따라 검색한다.

### Starts with ‘a’;

```sql
SELECT *
FROM Customers
WHERE CustomerName LIKE 'a%';
```

### Ends with ‘b’;

```sql
SELECT *
FROM Customers
WHERE CustomerName LIKE '%a';
```

### Contains ‘or’;

```sql
SELECT *
FROM Customers
WHERE CustomerName LIKE '%or%';
```

### Starts with ‘a’ and ends with ‘o’;

```sql
SELECT *
FROM Customers
WHERE CustomerName LIKE 'a%o';
```
