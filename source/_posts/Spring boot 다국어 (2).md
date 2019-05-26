---
title : Spring Boot 다국어 설정 (2)
tags: ["springboot"]
---

다국어 메시지 설정을 마쳤고, 몇가지 필요한게 더 있다. 

## MessageUtils 구현
MessageSourceAccessor로 MessageUtils 를 구현해서 다국어를 더 편리하게 사용한다. 

### MessageUtils 구현 
*  static 으로 구현
```java java
public class MessageUtils {  
  static MessageSourceAccessor messageSourceAccessor;  
  
  public static void setMessageSourceAccessor(MessageSourceAccessor messageSourceAccessor) {  
  MessageUtils.messageSourceAccessor = messageSourceAccessor;  
  }  
  
  public static MessageSourceAccessor getMessageSourceAccessor() {  
  return messageSourceAccessor;  
  }  
  
  public static String getMessage(String key) {  
  return messageSourceAccessor.getMessage(key);  
  }  
  
  public static String getMessage(String key, Object[] objs) {  
  return messageSourceAccessor.getMessage(key, objs, Locale.getDefault());  
  }  
}
```

### Bean 등록
* `MessageSourceAccessor ` 사용 
* Header 의 Accept-Language 알아서 읽음 
*  MessageUtils Bean 등록

```java java
@Configuration  
public class I18nConfig {  
  
  @Bean  
  public LocaleResolver localeResolver() {  
  AcceptHeaderLocaleResolver localeResolver = new AcceptHeaderLocaleResolver();  
  localeResolver.setDefaultLocale(Locale.KOREA); // 디폴트 한국  
  return localeResolver;  
  }  
  
  @Bean  
  public ReloadableResourceBundleMessageSource messageSource() {  
  ReloadableResourceBundleMessageSource messageSource = new ReloadableResourceBundleMessageSource();  
  messageSource.setBasename("classpath:/messages/message");  
//        messageSource.setBasename("file:D:/message"); // 절대경로  
  messageSource.setDefaultEncoding("utf-8");  
  messageSource.setCacheSeconds(180); // 리로딩 간격  
  Locale.setDefault(Locale.KOREA);  
  return messageSource;  
  }  
  
  @Bean  
  public MessageSourceAccessor messageSourceAccessor() {  
  return new MessageSourceAccessor(this.messageSource());  
  }  
  
  @Bean  
  public void messageUtils() {  
  MessageUtils.setMessageSourceAccessor(this.messageSourceAccessor());  
  }  
}
```

### 사용
```java java
MessageUtils.getMessage("key");
```


## 빈 검증 메시지 다국어 처리 
형식 검증을 위해 @Valid 를 사용한다. 검증이 걸리면, 메시지를 반환하는데, 이 부분도 다국어가 되어야 한다. 
컨트롤러에서 Locale 를 매개변수로 받고, Accept-Language 필드를 전송하면, 자동으로 다국어 처리가 된 메세지를 반환한다. 

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEwNzg3Njk3MjMsMjIxMzEzNjQxXX0=
-->