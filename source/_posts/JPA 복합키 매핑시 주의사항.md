---
title : "JPA 복합키 조인시 주의사항"
tags: ["jpa"]
---
JPA 에서 복합키에 대해 JOIN COLUMN을 아래와 같이 설정해줬었다. 
```java java
@ManyToOne(fetch = FetchType.EAGER)  
@JoinColumns({  
  @JoinColumn(name = "PARENT_NO"),  
  @JoinColumn(name = "COL_ONE"),  
  @JoinColumn(name = "COL_TWO")
})  
private Parent parent;
```
### 문제 
DB에  COL_TWO와 COL_ONE 컬럼에 데이터가 바껴서 삽입되는 현상이 발생했다. 
JPA를 통해서 삽입도 조회도 제대로 되지만, 테이블에 데이터만 바껴서 삽입되어있는 이상한 현상.

### 해결 
복합키 매핑시에 어떤 컬럼을 참조하는지 명시적으로 써주지 않으면 발생할수 있는 문제라고 한다. 

```java java
@ManyToOne(fetch = FetchType.EAGER)  
@JoinColumns({  
  @JoinColumn(name = "PARENT_NO", referencedColumnName = "PARENT_NO"),  
  @JoinColumn(name = "COL_ONE", referencedColumnName = "COL_ONE"),  
  @JoinColumn(name = "COL_TWO", referencedColumnName = "COL_TWO")
})  
private Parent parent;
```


<!--stackedit_data:
eyJoaXN0b3J5IjpbMjIwMDU5NjQyLC0xMTU0MTI1Mzg3XX0=
-->