---
title : "JPA - 기본키 매핑 전략"
tags: ["jpa"]
---

## 1. 직접 할당
직접 @Id 필드에 set

## 2. 자동 생성
대리키 사용 

### IDENTITY 
기본키 생성을 데이터 베이스에 위임 (mysql - auto increment)
데이터베이스에 저장한 후에 식별자를 조회해서 엔터티 식별자에 할당한다.

#### 단점 
데이터베이스에 값을 저장하고 나서야 기본키값을 구할수 있다. 

### SEQUENCE
데이터베이스 시퀀스를 사용하여 기본키 할당 (Oracle - sequence)
> mysql 은 sequence가 없음

## 위 두가지는 데이터베이스 벤더에 의존적이다.

### TABLE
키 생성 테이블을 사용
> 시퀀스용 테이블을 생성해서 테이블의 기본키를 저장하고 관리한다.


<!--stackedit_data:
eyJoaXN0b3J5IjpbNjQyMDQyNTk5XX0=
-->
