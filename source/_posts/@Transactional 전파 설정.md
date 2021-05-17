---
title: "트랜잭션의 전파 설정"
tags: ["transactional"]
---

## 트랜잭션의 전파 설정
하나의 트랜잭션 안에 다른 트랜잭션을 불렀을 때 처리하는 방법을 설정 

스프링 컨테이너는 영속성의 생명을 트랜잭션 범위와 일치하는 것을 기본으로 한다. 즉, 영속성 컨텍스트의 생존범위와 트랜잭션의 범위가 같다.

### REQUIRED (기본값)
부모 트랜잭션이 있다면 부모 트랜잭션으로 합류한다. 
없다면 새로운 트랜잭션을 생성한다. 

### REQUIRES_NEW
무조건 새 트랜잭션을 생성한다. 
각각의 트랜잭션이 롤백되더라도 서로 영향을 주지 않는다. 

### MANDATORY 
부모 트랜잭션에 합류한다. 
없다면 예외를 발생한다.

그 외 NESTED, NEVER, SUPPORTS, NOT_SUPPORTED 옵션이 존재한다. 

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTExMzc3MzcxNDZdfQ==
-->