---
title: "SQL 숫자, 문자 섞인 정렬"
tags: ["mysql", "order by"]
---

데이터베이스에 문자 타입에 1,2,3,4.. 가 있으면 1,11,12.. 순으로 정렬이 된다. 

이럴때 해결방법

### 1번
```sql 
ORDER BY LENGTH(COL_NAME), COL_NAME
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE4MTc5NTcyNjBdfQ==
-->


### 2번
```sql
ORDER BY COL_NAME*1
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbOTk3Njc4MDQ5XX0=
-->