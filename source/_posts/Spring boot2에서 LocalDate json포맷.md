---
title: Spring Boot 2버젼에서 LocalDate JSON 포매팅 방법
tags: ["java","springboot"]
---
Spring Boot 2 버젼부턴 Java 8인 `LocalDate`, `LocalDatetime` JSON포맷팅 문제를 어노테이션으로 해결할수 있다.
#### 예제
```java java
@JsonFormat(pattern="dd-MM-yyyy")
protected LocalDate localDate;

@JsonFormat(pattern="yyyy-MM-dd HH:mm")
protected LocalDateTime localDateTime1;

@JsonFormat(pattern="yyyy-MM-dd")
protected LocalDateTime localDateTime2;

@JsonFormat(pattern="HH:mm")
protected LocalTime localTime;
```
객체타입이 아닌 설정한 포맷팅으로 json으로 넘어간다.

#### 결과
```json json
{
	"localDate": "01-03-2019",
	"localDateTime1": "2019-03-01 17:39",
	"localDateTime2": "2019-03-01",
	"localTime": "19:35"
}
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTk5NTgyODcyOCwxNzM2ODU4MzYwLDE4Mj
c3MjQyNzFdfQ==
-->
