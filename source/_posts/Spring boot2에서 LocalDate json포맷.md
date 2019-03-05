---
title: Spring Boot 2버젼에서 LocalDate JSON 포매팅 방법
tags: ["java"]
---
Spring Boot 2 버젼부턴 Java 8인 `LocalDate`, `LocalDatetime` JSON포맷팅 문제를 어노테이션으로 해결할수 있다.

```java java
@JsonFormat(pattern="dd-MM-yyyy")

@JsonFormat(pattern="yyyy-MM-dd HH:mm")

@JsonFormat(pattern="yyyy-MM-dd")

@JsonFormat(pattern="HH:mm")
```
객체타입이 아닌 설정한 포맷팅으로 json으로 넘어간다.
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTgyNzcyNDI3MV19
-->