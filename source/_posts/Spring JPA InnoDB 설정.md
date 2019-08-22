---
title: "Spring Boot JPA InnoDB 설정"
tags: ["jpa","mysql"]
---
# MySQL의 Storage Engine
## 가장 많이 사용하는 엔진
###  MyISAM
- 전체적인 속도가 InnoDB 보다 빠르다. 
- 특히 Select 작업 속도가 빨라 읽기 작업에 적합하다.
- 데이터 무결성이 보장되지 않는다. 
	- 개발자나 DBA가 직접 무결성을 보장해야한다.
	- 외래키를 걸 수 없다.

### InnoDB
- 외래키 설정 가능하다.
- 대부분 사용한다.

## Spring boot JPA 설정
JPA를 기본으로 사용하고 ddl-auto를 했더니, MyISAM로 테이블이 만들어졌다. 
InnoDB를 사용하기 위해서 설정 수정 
application.yml
```yml
jpa:
  properties.hibernate:
    dialect: org.hibernate.dialect.MySQL5InnoDBDialect
```

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTIxMjI1NDc4NzYsMjE5MzcwMjg4XX0=
-->
