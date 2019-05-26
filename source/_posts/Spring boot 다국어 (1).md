---
title : Spring Boot 다국어 설정 (1)
tags: ["springboot"]
---

스프링부트는 다국어 설정을 지원한다. 이를 이용해서 API의 다국어 지원을 구현한다. 

다국어 설정에 대해 먼저 구현한다.

# 1. 다국어 설정 (I18nConfig.java)

## LocaleResolver
클라이언트의 언어&국거 정보를 인식할 클래스 
Bean 등록 필요 
아래 종류들 중 원하는 것을 등록
* AcceptHeaderLocaleResolver : http 통신 때 사용되는 Accept-Language 필드를 이용하여 언어&국가 정보를 인식
* CookieLocaleResolver : 쿠키를 이용해 언어&국가정보를 인식.
* SessionLocaleResolver : 세션을 이용해 언어&국가정보를 인식.
* FixedLocaleResolver : 모든 요청에 대해 특정한 언어&국가정보로 인식.

### `AcceptHeaderLocaleResolver` 선택
REST API 로써 프론트에서 Accept-Language 필드값을 넘겨주려고 했었기 때문에 `AcceptHeaderLocaleResolver`를 선택했다. 

### Accept-Language 필드 > 언어코드-국가코드 형태
* 한국어-한국 : ko-KR
* 영어-미국 : en-US

### Locale
언어&국가정보 표현하는 클래스 
Controller 에서 Locale을 매개변수로 받을 수 있다. 

## MessageSource
클라이언트의 언어&국가정보(Locale 객체)를 이용하여 여러 언어 리소스들을 관리하는 클래스
Bean 등록 필요
* ResourceBundleMessageSource 
	* 지정한 path로 여러 리소스파일들을 인식
	* Locale에 따라 각기 다른 리소스 파일 선택할수 있도록 해줌
* **ReloadableResourceBundleMessageSource**
	* ResourceBundleMessageSource 와 기능 같음
	* 지정한 시간마다 리소스 파일 리로드하여 운영환경에서 재배포 없이 없데이트된 리소스 파일을 사용할수 있게 해줌

### 다국어만 바꿔도 배포를 하지 않도록 `ReloadableResourceBundleMessageSource` 로 설정 

```java java
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
  messageSource.setDefaultEncoding("utf-8"); 
  messageSource.setCacheSeconds(180); // 리로딩 간격  
  Locale.setDefault(Locale.KOREA);  
  return messageSource;  
}
```
* basename 
	* 다국어 정보를 정의한 properties 파일들이 들어있는 장소 
	* 여러 properties가 들어있으면 여러개 정의 
	* 절대 경로 사용 가능 (프로젝트 폴더 외 내부에 두고 다국어 유지보수)
```java java
messageSource.setBasename("file:D:/message");
```

# 2. properties 파일 
basename 설정한 것에 언어코드, 언어코드-국가코드 를 붙혀서 properties 파일을 생성한다. 
## 예시 
* 파일명 : message_ko.properties & message_n.properties 
* Accept-Language : ko / en 
* 파일명 : message-ko-

### 사용 (Controller)
```java java
@Autowired  
MessageSource messageSource;

@GetMapping("/locale")  
public void localetest(Locale locale) {
  log.trace(messageSource.getMessage("key", null, locale));  
}
```

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTQ2NjU0MzkxMSwzNjEwOTM3NjAsLTE2MT
Y1NzU5MDQsMjgxNDAyNjIyLDE1MzM5NzcwNzAsNDgxMzgzMTk2
LC0zNDIxMzA2MTldfQ==
-->