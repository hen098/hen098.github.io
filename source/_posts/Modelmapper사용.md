---
title: "ModelMapper를 사용해서 도메인을 DTO로 매핑"
tags: ["jpa","modelmapper"]
---

JPA를 사용하면 도메인을 반환하는경우도 많다. 
하지만, 도메인에 불필요한 정보가 많으면 DTO를 사용하여 필요한 정보만 반환한다. 

ModelMapper와 Java 8 문법을 사용하면, 간략하게 DTO를 사용할 수 있다. 

## 방법
간단한 User정보를 반환하는 DTO 

### build.gradle
```
compile('org.modelmapper:modelmapper:0.7.8')
```

### WebConfig
Bean 등록 
```java
@Configuration
public class WebConfig implements WebMvcConfigurer {
  @Bean  
  public ModelMapper modelMapper() {  
  return new ModelMapper();  
  }  
}
```

### MapperUtils
Util로 만들어서 생성하지 않고 사용하도록 만든다. 
```java
@Component  
public class MapperUtils {  
  static ModelMapper modelMapper;  
  
  public MapperUtils(ModelMapper modelMapper) {  
  this.modelMapper = modelMapper;  
  // AccessLevel을 필드로 해서 setter가 없어도 매핑되도록 한다. 
  // modelmap
  this.modelMapper.getConfiguration()  
  .setFieldAccessLevel(Configuration.AccessLevel.PRIVATE)  
  .setFieldMatchingEnabled(true);  
  }  
  
  public static <D> D convert(Object source, Class<D> classLiteral) {  
  return modelMapper.map(source, classLiteral);  
  }  
  
  public static <D, E> List<D> convert(Collection<E> sourceList, Class<D> classLiteral) {  
  return sourceList.stream()  
  .map(source -> modelMapper.map(source, classLiteral))  
  .collect(Collectors.toList());  
  }  
  
  public static <D, E> Page<D> convert(Page<E> sourceList, Class<D> classLiteral, Pageable pageable) {  
  List<D> list = sourceList.getContent().stream()  
  .map(source -> modelMapper.map(source, classLiteral))  
  .collect(Collectors.toList());  
  return new PageImpl<>(list, pageable, sourceList.getTotalElements());  
  }  
}
```

### User.java
도메인
```java java
public class User {
	private String userId;
	private String password;
	private String email;
	private Set<Role> roles;
	private String profilePath;
	private String phoneNum;
	...
}
```

### UserDto.java 
User객체를 받는 UserDto
**도메인과 같은 타입, 변수명으로 선언해야 ModelMapper로 자동매핑할 수 있다.**
```java java
public class UserDto {
	private String userId;
	private Set<Role> roles;
}
```



<!--stackedit_data:
eyJoaXN0b3J5IjpbLTIxMzQxMzc2NTMsLTc2NzgwNDQ0NiwtMT
MxOTk2NzA3NywtNDI1NjU4MjM0XX0=
-->