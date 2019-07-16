---
title: "ModelMapper를 사용해서 도메인을 DTO로 매핑"
tags: ["jpa","modelmapper", "DTO"]
---
# DTO와 Entity
도메인(Entity)은 비즈니스 업무를 포함한다. 
그럼으로, 반환값으로 도메인(Entity)을 사용하는 것은 비즈니스가 노출될 위험이 있다. 

ModelMapper와 Java 8 문법을 사용하면, 간략하게 DTO를 사용할 수 있다. 

## DTO 
요청과 응답 dto가 다를수 있다. 
XXRequest와 XXResponse로 따로 만들어서 사용할 수 있다. 

Request는 표현영역 값 검증 어노테이션을 포함한다. 

# 구현방법

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
  // modelmapper는 디폴트로 setter를 통해서 aceess한다.
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

### XXXDto.java 
* 도메인과 같은 타입, 변수명으로 선언해야 ModelMapper로 자동매핑할 수 있다.
* DB에서 불러온 값을 반환하는 DTO는 습관적인 Setter 구현을 지양한다.

### 사용 - Controller
DTO를 Entity로 반환하거나, Entity를 DTO로 반환할 때 사용한다. 

```java
// 객체를 객체로 반환 
MapperUtils.convert(postDto, Post.class);
MapperUtils.convert(post, PostDto.class);

// 리스트를 리스트로 반환
MapperUtils.convert(posts, PostResponse.class);	

// 페이징된 객체를 페이징된 객체로 반환
MapperUtils.convert(posts, PostResponse.class, pageable)
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTU4MDk0Mjc2OSwtNzY3ODA0NDQ2LC0xMz
E5OTY3MDc3LC00MjU2NTgyMzRdfQ==
-->