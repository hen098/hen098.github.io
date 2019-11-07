---
title : "jpa-getOne vs findOne"
tags : ["jpa", "프록시"]
---

## getOne()
* lazy-loading을 통해 객체를 전달
	* `getReference()`를 통해서 조회 
		* 실제 db를 조회하지 않고 프록시 객체를 반환
* DB의 참조만을 반환
	* 프록시 객체만 반환한다. 
	* 존재하면 not exception을 발생하지 않음 
* 기본적으로 트랜잭션 외부에 존재 (서비스에 선언한 `@Transactional`은 고려되지 않는다.)
* 단지 연관관계만 맺는다면, db에서 모든 데이터를 조회해올 필요 없음 > 이런 경우에 프록시 객체를 사용.

### Lazy-loading
사용할 시에 조회한다.
```java
post.getTitle();
```
이런식으로 사용하는 부분에 쿼리를 한번더 날려서 조회한다.

## 프록시 객체
* 실제 클래스와 겉모양이 같음
	* 프록시인지 실제 클래스인지 구분하지 않고 사용하면 된다.
* 프록시의 객체를 호출하면 프록시 객체가 실제 객체의 메소드를 호출
	* 실제 사용하는 시점에 db에 조회 

## findOne()
* 즉시 조회하여 객체를 전달
	* `find()` 통해서 조회

참고
[https://joosjuliet.github.io/getOne-findOne/](https://joosjuliet.github.io/getOne-findOne/)
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTUzMjA2MjQzOV19
-->