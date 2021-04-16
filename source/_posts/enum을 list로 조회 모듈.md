---
title : "enum을 리스트로 조회 모듈"
tags: ["enum"]
---
select 등과 같은 목록으로 보여줘야하는 enum들은 application에 저장하고 db를 조회하거나 enum을 리스트로 각각 변환하는 작업을 하지 않을 수 있다. 

### Enum Model 생성
```java
public interface EnumModel {  
    String getValue();  
  String getName();  
}
```

### Enum value 생성
```java
@Getter  
public class EnumValue {  
    private String value;  
 private String korName;  
 private String engName;  
  
 public EnumValue(EnumModel enumModel) {  
        this.value = enumModel.getValue();  
 this.korName = enumModel.getKorName();  
 this.engName = enumModel.getEngName();  
  }  
}
```

### Enum model 을 구현하는 enum 생성

```java
public enum Status implements EnumModel {
..

@Override  
public String getValue() {  
    return name();  
}
}
```

### Enum mapper 생성 
enum을 리스트로 바꿔서 map에 저장 
```java 
public class EnumMapper {  
    private Map<String, List<EnumValue>> factory = new HashMap<>();  
  
 private List<EnumValue> toEnumValues(Class<? extends EnumModel> e) {  
        return Arrays.stream(e.getEnumConstants())  
                .map(EnumValue::new)  
                .collect(Collectors.toList());  
  }  
  
    public void put(String key, Class<? extends EnumModel> e) {  
        factory.put(key, toEnumValues(e));  
  }  
  
    public Map<String, List<EnumValue>> getAll() {  
        return factory;  
  }  
  
    public Map<String, List<EnumValue>> get(String keys) {  
        return Arrays.stream(keys.split(","))  
                .collect(Collectors.toMap(Function.identity(), key -> factory.get(key)));  
  }  
}
```

### Bean 등록
```java 
@Configuration  
public class AppConfig {  
  
    @Bean  
  public EnumMapper enumMapper() {  
        EnumMapper enumMapper = new EnumMapper();  
		enumMapper.put("Currency", PayEnums.Currency.class);  
		return enumMapper;  
  }  
}
```

### 사용 - controller
```java
@NonNull  
private EnumMapper enumMapper;

@GetMapping("/enums/{names}")  
public ResponseEntity<Map<String, List<EnumValue>>> findByEnums(@PathVariable String names) {  
    return ResponseEntity.ok(enumMapper.get(names));  
}
```
