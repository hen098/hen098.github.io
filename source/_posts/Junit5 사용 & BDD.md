---
title : "Junit5 사용 & BDD"
tags: ["junit5", "BDD"]
---


## Describe - Context - It 패턴

given-when-then 패턴은 주석으로 처리하기 때문에 강제성을 줄수 없다.

```java
Describe : 
	Context : 
    It : 

```

위 모습처럼 계층구조로 만들어서 구현할 수 있다.

### 작성법

-   Describe
    -   테스트 대상을 명사로 작성
-   Context
    -   ~인 경우, ~할 때, 만약 ~하다면 과 같은 상황 또는 조건
-   It
    -   테스트 대상의 행동을 작성
    -   ~이다, ~한다, ~를 갖는다 가 적절
    -   ~된다 같은 표현은 지양

## `@Nested`
-   중첩 구조 가능
-   junit4 에서 중첩구조를 지원하지 않아서 BDD를 실행할 때

```java
@Test
public void test() {
    // given

    // when

    // then 
}

```

위와 같이 작성하라고 가이드한다. junit5 에서는 중첩구조가 가능하기 때문에 각각 구분을 메소드나 클래스로 두어서 만들수 있다.

## 그 외 
### @DataJpaTest
> junit5 만의 기능은 아니나, 정확히 모르고 있었던 부분이라 기억하기 위해 함께 작성한다.

jpa 관련 테스트 설정만 로드한다.
-   DataSource 설정이 정상적인지
-   JPA 사용하여 데이터를 제대로 생성, 수정, 삭제하는지
-   내장형 데이터베이스를 사용하여 실제 데이터베이스를 사용하지 않고 테스트 데이터베이스로 테스트함
-   `@Transactional` 어노테이션을 포함함으로 테스트가 완료되면 자동으로 롤백된다.
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTU2MTk0MjA4Ml19
-->