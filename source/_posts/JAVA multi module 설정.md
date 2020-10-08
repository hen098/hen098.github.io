---
title: "JAVA multi module 설정"
tags: ["Java"]
--- 
JAVA multi module로 프로젝트를 구성하게하면 필요한 설정이 있다. 

## XXApplication.java
```java java
@SpringBootApplication(scanBasePackages =  
  {"kr.co.moduleA", "kr.co.moduleB"})
public class MultimoduleApiApplication {  
  
  public static void main(String[] args) {  
  SpringApplication.run(MultimoduleApiApplication.class, args);  
  }  
}
```

## bundle.gradle
```java 
..

dependencies {
    compile project(':moduleA')
    compile project(':moduleB')
}
```

<!--stackedit_data:
eyJoaXN0b3J5IjpbODAyMzEwODc4XX0=
-->