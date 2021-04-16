---
title: "modelmapper custom 매핑"
tags: ["modelmapper"]
---

### ` xxMap` 작성 
```java
public class UserMap extends PropertyMap<User, UserDto> {  
    protected void configure() {  
        map().setCode(source.getUserNo());  
  map().setValue(source.getUserName());  
  }  
}
```

### `addMappings()`
```java
@Configuration  
@EnableWebMvc  
public class WebConfig implements WebMvcConfigurer {  
    @Bean  
  public ModelMapper modelMapper() {  
        ModelMapper modelMapper = new ModelMapper();  
  modelMapper.getConfiguration()  
                .setFieldAccessLevel(org.modelmapper.config.Configuration.AccessLevel.PRIVATE)  
                .setFieldMatchingEnabled(true);  
  
  modelMapper.addMappings(new AdmsMap());  
  
 return modelMapper;  
  }  
  
    @Bean  
  public ApexModelMapper apexModelMapper(ModelMapper modelMapper) {  
        return new ApexModelMapper(modelMapper);  
  }  
  
}
```

### 사용 
```java
modelmapper.map(user, UserDto.class);
```
