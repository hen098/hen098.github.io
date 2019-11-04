---
title: "의존성 주입(DI)"
tags: ["spring"]
---

## Bean 
스프링 컨테이너 상에서 생성된 객체 
컨테이너는 스프링에서 Ioc 원칙을 통한 객체와 의존성을 관리하기 위해 만든 요소이다. 안에 bean들이 들어가있다.

## Dependency Injection (의존성 주입)
프레임워크에 의해 객체의 의존성이 주입되는 설계 패턴

## IoC (제어의 역전)
프로그램 제어권을 프레임워크가 가져가는 것

- 개발자가 제어 하지만, 코드 전체에 대한 제어는 프레임워크가 한다.
- 개발자가 설정하면 (xml, annotation) 컨테이너가 알아서 처리한다.
- 즉, 프레임워크 속에서 프로그래밍 한다.

### 의존성 주입 방법
1. 생성자
2. setter method
3. `@Autowired` 어노테이션 통한 주입

## `@Autowired`
각 상황에 맞는 IoC 컨테이너 안에 존재하는 Bean을 자동으로 주입받는다.


<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE2NDU0NjMsLTExNjI1NTY3MjhdfQ==
-->
