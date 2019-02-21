---
title: JAVA에서 Enum 객체 사용
tags: ["java"]
---
## Enum 이란
* JDK 1.5 버젼부터 추가
* 열거형(enumerated type) 이라고 부름
* **서로 연관된 상수들의 집합**이라고 할 수 있다.
* 이전에는 class나 interface를 사용해서 상수들의 집합을 정의함
* if문을 줄여줄 수 있음

## 특징 
### 1. 변수를 가질 수 있음
### 2. 생성자 가능 
class와 다르게 생성자는 private 로 지정해야한다.
#### 이유
상수는 수정이 불가능해야한다. 외부에서 접근해서 동적으로 값을 할당할 수 없어야한다. 
### 3. 싱글톤 (singleton)
### 4. 메소드를 가질수 있음 

## 사용방법
* 하나의 java 파일로
* 클래스 안에서 선언
* 클래스 밖에서 선언
* `values()`: 모든 상수를 배열에 담아 순서대로 반환
* `oridinal()`: 순서대로 정수 값으로 반환
* `valueOf(string)`: string과 이름이 일치하는  enum 객체를 반환 
* 상수에 값 설정 
```java java
public enum Type {
	IMAGE("이미지"),
	VIDEO("비디오");
	
	final private String typeName;
	
	private Type(String typeName) {
		this.typeName = typeName;
	}
}
```
## 도메인 설계에 이용
### 공통코드 대신 사용
`XXXType`이나 `XXXState`와 같이 공통코드 성격에 Enum을 이용할 수 있다. 
도메인 클래스에 선언하고, jpa 사용시에 저장 매핑까지 할 수 있다. 
Enum을 JSON으로 리턴하면 상수 STRING만 출력된다. 
```java java
@Enumerated(EnumType.STRING)
private Type type;
```
* `@Enumerated(EnumType.STRING)`: String으로 값을 저장
* `@Enumerated(EnumType.ORDINAL)`: 정수값으로 저장

### 정보 그룹 관리
대그룹, 소그룹 관계(포함)를 가지는 데이터를 관리할 수 있다. 
(이부분은 직접 구현해볼만한 예제가 있을 때 적용해보아야겠다. 잘 와닿지 않음)

### 장점
* db에서나 코드에서 가독성이 높아진다. (코드명만 보아도 알수 있음)
* 공통코드 값을 확인하기 위해 db를 조회하는 쿼리가 줄어든다.
* 공통코드가 가지는 로직은 해당 Enum에 구현한다. (애매한 위치에 메소드를 넣을 필요가 없다.) 

## 결론 
Enum 객체를 사용하여 깔끔하고, 가독성 높은 코드를 구현할 수 있다. 
하지만, 모두 Enum으로 구현할 필요는 없다. 성격에 따라서 Enum과 공통코드를 적절하게 사용하면 된다. 

### 내 기준
* XXType, XXState > Enum
* 화면에 표출되는 데이터 (select 박스 등) : 공통코드 테이블
<!--stackedit_data:
eyJoaXN0b3J5IjpbMzg5MTQwNjQ5XX0=
-->