---
title: "Value Collection을 JPA로 구현"
tags: ["jpa", "DDD"]
---

# 밸류 컬렉션 테이블 매핑 
밸류 컬렉션을 RDB와 매핑하는 방법은 세 가지가 있다. 

* 컬럼에 매핑
* 한 컬럼에 배열로 매핑 (,로 구분해서 한 컬럼에 들어감)
* 테이블에 매핑 (밸류 컬렉션)

테이블에 매핑하는 방법은 DDD 책에서 `@CollectionTable`을 사용하도록 가이드한다. 

## 밸류 타입 구현 - `@Embeddable`
밸류 타입을 `@Embeddable` 로 구현한후 `@CollectionTable` 로 매핑해서 사용할 수 있다.
그렇게 생성된 밸류 테이블은 엔터티가 아니며 `@Id` 매핑이 없기 때문에 pk가 없는 테이블이 만들어졌다. (FK만 있음)
데이터베이스 관점에서 PK가 없는 테이블은 문제가 될수 있다. 

### PK가 없는 밸류 컬렉션 문제점
* 데이터 삽입 중복이 발생할 확률이 높다. 
* UNIQUE KEY를 지정하더라도 PK가 없기 때문에 NULL이 들어갈 수 있다. 
* 조회 성능에 영향을 줄수 있다. 
MySQL에서는 인덱싱을 위해 PK를 사용한다. PK가 없으면 내부에서 Auto Increment INT 를 사용하거나, UNIQUE를 찾아내거나 사용해서 인덱싱을 한다. MySQL이 내부에서 해주는 인덱싱에 의존해서 PK를 안쓰는 행위는 위험할 수 있다. 
* PK가 없기 때문에 변경이 발생 시, 전체를 지우고 다시 입력하게된다. 
이를 해결하기 위해서 `@OrderColumn`을 추가하는 방법이 있긴하지만, `@OrderColumn` 자체가 order  값을 완벽히 보장하지 않기 때문에 이슈가 있다. 

### 해결책
JPA 구현에서 Entity처럼 db 스키마를 구성하지만, 구현하는 입장에서는 객체로 보이도록 구현한다. 
상속을 이용해서 **식별자 속성을 은닉**시킨다.

#### 상속받아서 사용할 식별자 클래스
```java 
@MappedSuperclass
@NoArgsConstructor(access = AccessLevel.PROTECTED)  
@AllArgsConstructor
public abstract class IdentifiedValueObject {  
  @Id  
  @GeneratedValue(strategy = GenerationType.IDENTITY)  
  @Getter(AccessLevel.PACKAGE)
  @Setter(AccessLevel.PACKAGE)
  private Long id;  
}
```

#### 밸류 
```java
@Entity  
@Table(name = "Child")  
@NoArgsConstructor(access = AccessLevel.PROTECTED)  
public class Child extends IdentifiedValueObject {
  ...  
}
```

### 어색한 점
* 식별자 컬럼명을 변경할 수 없다. 
* 유일 식별자 (Autoincrement) 로만 식별자 구현 가능하다. 
* 식별자로 복합키를 둘수 없다. 

이러한 어색한 점에도 불구하고, 테이블로 매핑된 밸류 컬렉션은 PK가 있어야하며, 애그리거트와 루트 엔터티는 개념적으로 존재하게 구현한다. 

즉, JPA 구현체와 DDD 부분이 일치하지 않을수 있다. 
DDD의 엔터티와 JPA 의 엔터티가 같지 않을 수 있다.

## 연관관계 구현 
개념적으로 밸류이지만, 엔티티임으로 부모 엔터티와 연관관계를 가져야한다. 

## 일대다 단방향 연관관계
일대일 단방향 연관관계를 사용하면, 구현체가 단순하다. 
또한, 자식 객체에서 부모 객체의 존재를 모른다. 

```java
@Entity  
@Getter 
@NoArgsConstructor(access = AccessLevel.PROTECTED)    
public class Parent extends AbstractBaseEntity {  
	@Id  
	@GeneratedValue(strategy = GenerationType.IDENTITY)  
	@Column(name = "parent_no")  
	private Long id;

	@OneToMany(cascade = CascadeType.ALL, orphanRemoval = true, fetch = FetchType.LAZY)
	@JoinColumn(name = "parent_no")
	private Set<Child> childs = new HashSet<>();
}
```
* joincolumn을 매핑해주지 않으면 parent는 child을 전혀 몰라서 관계 테이블이 생긴다. 

```java
@Entity
@NoArgsConstructor(access = AccessLevel.PROTECTED)   
public class Child extends IdentifiedValueObject {
	private String childname;
}
```
* child는 parent를 가지고 있지 않다.

### 단점

* 성능 문제
	* 레코드 삭제 요청시에 parent_id에 null을 넣는 update문, child delete 문이 발생한다. 
이런 경우, JPA에서 remove를 후  delete를 수행하기 때문이다. 
update 문을 날려서 remove를 수행하는 것은 안전하지 않아보이며, delete문만 수행하는게 간단해보인다.

* 관리 문제
	* 테이블과 매핑된 테이블이 아닌 다른 엔티티에서 외래키를 관리한다.   

이런 단점때문에 일대다 단방향 연관관계보다 다대일 양방향 연관관계를 사용하도록 권장한다.

## 다대일 양방향 연관관계

```java
@Entity  
@Getter 
@NoArgsConstructor(access = AccessLevel.PROTECTED)    
public class Parent extends AbstractBaseEntity {  
	@Id  
	@GeneratedValue(strategy = GenerationType.IDENTITY)  
	@Column(name = "parent_no")  
	private Long id;

	@OneToMany(cascade = CascadeType.ALL, orphanRemoval = true, fetch = FetchType.LAZY)
	@JoinColumn(name = "parent_no")
	private Set<Child> childs = new HashSet<>();
}
```
* 보통 `@OneToMany`부분에 mappedBy로 주인을 설정해준다. 주인을 설정해주게되면, 주인이 아닌쪽은 select 만 가능하다. 부모에서 insert, update, delete 모두 수행할 것임으로 주인을 설정하지 않는다.

```java
@Entity
@NoArgsConstructor(access = AccessLevel.PROTECTED)   
public class Child extends IdentifiedValueObject {
	@ManyToOne(fetch = FetchType.LAZY)
	@JoinColumn(name = "parent_no", insertable = false, updatable = false)
	private Parent parent;

	@Getter
	private String childname;
}
```
* parent에 getter를 만들지 않는다. 
* `insertable = false, updatable = false` insert와 update를 child에서 안되도록 한다.


### 단점 
child 에게 parent 존재를 알려주게 된다.

### 해결책
child 에서 parent의 getter를 private하게 만들거나, 아예 parent의 getter를 만들지 않는다.
parent에 접근하지 못하도록 한다. 


### 참고 
[http://redutan.github.io/2018/05/29/ddd-values-on-jpa](http://redutan.github.io/2018/05/29/ddd-values-on-jpa)
[https://homoefficio.github.io/2019/04/28/JPA-%EC%9D%BC%EB%8C%80%EB%8B%A4-%EB%8B%A8%EB%B0%A9%ED%96%A5-%EB%A7%A4%ED%95%91-%EC%9E%98%EB%AA%BB-%EC%82%AC%EC%9A%A9%ED%95%98%EB%A9%B4-%EB%B2%8C%EC%96%B4%EC%A7%80%EB%8A%94-%EC%9D%BC/](https://homoefficio.github.io/2019/04/28/JPA-%EC%9D%BC%EB%8C%80%EB%8B%A4-%EB%8B%A8%EB%B0%A9%ED%96%A5-%EB%A7%A4%ED%95%91-%EC%9E%98%EB%AA%BB-%EC%82%AC%EC%9A%A9%ED%95%98%EB%A9%B4-%EB%B2%8C%EC%96%B4%EC%A7%80%EB%8A%94-%EC%9D%BC/)

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTIwNDY5OTg3NTJdfQ==
-->
