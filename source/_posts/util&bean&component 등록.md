---
title : "bean/component등록 &util"
tags: ["spring", "bean"]
---

## Util
#### 관행 
- 상태가 없음
- 생성자 없음

## Bean 등록
개발자가 컨트롤이 불가능한 외부 라이브러리를 사용한 경우
반환하는 객체를 Bean으로 만듬 


## Component 등록
개발자가 직접 구현한 경우
선언된 클래스를 Bean으로 만듬


### 직접구현한 것을 Bean 으로 등록해도 괜찮을까 ? No
`@Bean`과 `@Component` 는 각각 선언할 수 있는 타입이 정해져있어 용도외에 선언은 컴파일 에러를 발생시킨다.
* @Component : class에만 선언 가능
* @Bean : 메소드에 선언 가능

## ModelMapper를 사용할 때 - 예 
### 원하는 것 
1. ModelMapper 사용
2. 인스턴스를 1개만 생성 
3. modelmapper 메소드를 구현부마다 작성하지않음 (wrapping)

### 구현
1. Modelmapper @Bean 등록
2. CustomModelMapper 구현 
ModelMapper를 주입받고 감싸서 구현 
3. 사용 


<!--stackedit_data:
eyJoaXN0b3J5IjpbMTk4MjE3NDIxMl19
-->