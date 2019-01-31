--- 
title : DDD 도메인 모델 도출
tags : ["DDD"]
---

## 개념모델
데이터베이스, 트랜잭션 처리, 성능, 구현 기술과 같은 것들 고려하지않고, 업무 관점에서 작성한다. 

개념 모델을 구현 가능한 형태로 **전환**하는 과정을 거쳐야 한다. 

처음부터 완벽구현은 불가능하다. 분류하고 계획한 후, 수정, 보완의 과정을 거쳐서 점진적으로 구현모델로 발전시켜나가야 한다. 

# 도메인 모델 도출
* 도메인을 이해한다. (기획서, 유스케이스, 사용자스토리 등 활용) 
	* 유스케이스
		> 유스케이서 명세서 : 사용자와 기능 사이에 일어나는 과정들
		   유스케이스 장점 : 요구사항을 추상화 (도식화)
	* 사용자스토리 
		> 장점 : flow를 볼수 있다.

* 핵심 구성요소, 규칙, 기능 찾기 > 요구사항에서 출발 
* 도메인 모델 초안 작성한다.
* 특정 툴(수단-화이트보다, 종이와 연필, 모델링 툴)에 종속되지 않는다. 

## 도메인 초안 작성
책에선 java로 작성했다. 
> 도메인전문가가 java를 안다는 전제하에 작성했다. 

### 밸류 = DTO
값을 담는 그릇. 밸류 타입으로 완전한 하나의 개념으로 만들 수 있다. 
프레젠테이션 계층과 도메인 계층이 데이터를 주고받을 때 사용하는 구조체이다.
* map 방식
* VO 방식

p.24
#### `int price` 라고 `int`형을 쓸수도 있지만, `Money price` 라고 선언한 이유?
`Money`클래스안에 `int value` 하나만 선언되었지만, `int price`라고 쓰는 것보다 `Money price` 라고 선언했다. 
의미있는 추상화 활동이다. 그로인해 가독성이 높아졌다. 

#### 불변 타입으로 구현 - 데이터 변경 불가능

밸류 객체 데이터 변경할 때 기존 데이터를 변경하기보다는(set method) 변경한 데이터를 갖는 새로운 밸류 객체를 생성하는 방식을 선호한다. 
```java java
public class Money {
	private int value;

	public Money add(Money money) {
		return new Money(this.value + money.value);
	}
}
```
set 메소드로 값을 잘못 변경하게되는 상황을 방지한다. 

### 엔터티
레코드를 구분하는 식별자를 갖는다.
#### 식별자
* 특정 규칙에 따라 생성 - 보통 String 타입. 체계 필요. 채번테이블이 필요. 중간에 구멍이 있을수도 있음
* UUID - random 값
* 값을 직접 입력 - id, email 등. 중복/인증 필요
* 일련번호 사용 - Double, Int 타입. 쉽고 빠르지만 체계를 가질수 없음

### 도메인 모델에 set 메소드 넣지 않기
도메인 모델에 get/set 메서드를 무조건 추가하는 것은 좋지 않은 버릇이다. 

어떤 전용으로 값을 변경하는 메소드로 만든다. 
```java java
public class Order {
	public void setShippingInfo(ShippingInfo newShipping) { .. }
	public void setOrderState(OrderState){ .. }
}
```
set 메소드가 없는 도메인에 값을 세팅하는 방법은 **생성자**이다.

#### 생성자를 구현해서, 생성시점에 필요한 데이터 전송 
생성할 때 최초로 값을 세팅해준다. 불변 타입이 된다. public한 일반적인 set메소드가 아닌, 전용 목적 메소드로만 값을 변경한다. 

set메소드를 가지는 경우도 있다. 

#### set 메소드
```java java
private void setOrderer(Orderer order) {
	..
}
```
`private`으로 내부에서 데이터를 변경할 목적으로 사용된다. 품질을 위해서 아무나 접근하여 변경가능하지 않도록 구현한다.

### 도메인 용어
가독성을 위해서 바로 이해할 수 있는 용어를 적용한다.
```java java
public enum OrderState {
	PAYMENT_WAITING, PREPARING, SHIPPED, DELIVERING, DELIVERY_COMPLETED;
}
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTQyMDMyNzEzMl19
-->
