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
형식 검증을 위해 @Valid 를 사용한다. 
검증 에러 메시지도 다국어 처리가 필요하다. 
> 아래 url 참고해서 구현

[https://books.google.co.kr/books?id=RzLVBQAAQBAJ&pg=PA328&lpg=PA328&dq=reloadableresourcebundlemessagesource+web-inf&source=bl&ots=EN1jLxXT-R&sig=ACfU3U3XJFJKIpN-YTm75ugpso63IDFLoA&hl=ko&sa=X&ved=2ahUKEwi4osKrtp_iAhU4yYsBHXHDAJUQ6AEwCHoECAgQAQ#v=onepage&q=reloadableresourcebundlemessagesource%20web-inf&f=false](https://books.google.co.kr/books?id=RzLVBQAAQBAJ&pg=PA328&lpg=PA328&dq=reloadableresourcebundlemessagesource+web-inf&source=bl&ots=EN1jLxXT-R&sig=ACfU3U3XJFJKIpN-YTm75ugpso63IDFLoA&hl=ko&sa=X&ved=2ahUKEwi4osKrtp_iAhU4yYsBHXHDAJUQ6AEwCHoECAgQAQ#v=onepage&q=reloadableresourcebundlemessagesource%20web-inf&f=false)
>> 빈 검증 국제화
지금 검증 valid 메시지 쓰고있음. 
이것도 국제화 필요 
<!--stackedit_data:
eyJoaXN0b3J5IjpbMjIxMzEzNjQxXX0=
-->