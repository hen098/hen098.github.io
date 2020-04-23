---
title: "p6spy 사용 - 콘솔에서 쿼리 값 확인"
tags: ["p6spy"]
---
console로 로그를 확인하면 parameter값에 '?' 로 찍힌다.
p6spy를 사용하면 ? 대신 실제 파라미터 값을 확인할 수 있다. 

개발 모드에서만 사용하고, 디버깅 및 개발에 용이하게 사용할 수 있다. 

## build.gradle
```
implementation 'com.github.gavlyukovskiy:p6spy-spring-boot-starter:1.6.1'
```

## application.yml 
```yaml
spring:
  jpa:
	url: jdbc:p6spy:mysql:'url~~~'	# (1)
    driverClassName: com.p6spy.engine.spy.P6SpyDriver	#(2)
```

## spy.properties
기본으로 사용해도 되지만, 콘솔에 찍히는 내용 커스텀 가능 
```properties 
driverlist=com.mysql.cj.jdbc.Driver
dateformat=yyyy-MM-dd a hh:mm:ss  
  
# logMessageFormat=com.p6spy.engine.spy.appender.MultiLineFormat # 기본 설정에서 sql이 multi line으로  
logMessageFormat=com.p6spy.engine.spy.appender.CustomLineFormat  
  
# 최대 format#customLogMessageFormat=%(currentTime)|took %(executionTime)ms|%(category)|connectionid %(connectionId)\n%(effectiveSqlSingleLine)\n%(sqlSingleLine);\n  
# effective sql만 제외한 formatcustomLogMessageFormat=%(currentTime)|took %(executionTime)ms|%(category)|connectionid %(connectionId)\n%(sqlSingleLine);\n
```



<!--stackedit_data:
eyJoaXN0b3J5IjpbMTk5NDU5MzQxMF19
-->