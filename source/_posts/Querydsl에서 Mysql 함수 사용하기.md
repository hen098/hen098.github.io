

--- 
title: "Querydsl에서 Mysql 함수 사용하기"
tags: ["Querydsl"]
--- 

### Mysql 함수 설정 
```java
import org.hibernate.dialect.MySQL57Dialect;  
import org.hibernate.dialect.function.StandardSQLFunction;  
import org.hibernate.type.StandardBasicTypes;  
  
public class MysqlCustomDialect extends MySQL57Dialect {  
    public MysqlCustomDialect() {  
        super();  
  registerFunction("Mysql 함수명", new StandardSQLFunction("호출명", StandardBasicTypes.STRING));  
}
```
- `registerFunction("Mysql등록된 함수명", Querydsl에서 사용할 이름과 리턴 타입)`

### application.yml 설정
```yml 
jpa: 
  database: mysql  
  generate-ddl: false  
  open-in-view: false  
  hibernate: 
    ddl-auto: none # 항상 조심!! 
  properties: 
    hibernate: 
  database-platform: com.example.querydsl.configuration.MysqlCustomDialect # 이 부분에 생성
```

### Querydsl 사용 방법
```java
select(
Expressions.stringTemplate("호출명('KR',{0},{1})", schoolCode, courseCode),
..
```

