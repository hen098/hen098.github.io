---
title: Spring Boot 설정 추가 
tags: ["springboot"]
---
자동구성된 스프링에서 추가적인 조작만 하려면 다음과 같은 행태로 구성한다.

```java java
@Configuration
public class WebConfig implements WebMvcConfigurer, WebMvcRegistions {
}
```
* `WebMvcConfigurer` 
	* 자동구성된 스프링 MVC 구성에 `Formatter`, `MessageConverter`등을 추가 등록할 때 사용
	* 스프링 부트 2.0 부터 `WebMvcConfigurer` 메서드에서 구현하는 클래스 모든 메서드를 구현해야하는 강제력이 사라졌다. 

* `WebMvcRegistions ` 
	* `RequestMappingHandlerMapping`, `RequestMappingHandlerAdapter`, `ExceptionHandlerExceptionResolver` 재정의할 때 사용한다.

## 스프링 MVC 구성 제어 
### EnableWebMvc
`@EnableWebMvc` 선언하면 `WebMvcConfigurationSupport` 구성을 불러온다.
```java java
@Configuration
@EnableWebMvc
public class WebConfig {
}
```

## `@Configuration` & `@Component`
일반적으로 외부의 것을 추가하는 것은 `@Confituration`을, 개발자가 직접 제어 가능한 것은 `@Component`를 사용한다. 
`@Configuration`은 `@Bean`과 함께 짝꿍을 이루어서 사용해야 한다. 

`@Component`는 클래스 상단에 선언하며, 클래스 자체를 bean으로 등록해준다. 
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTkxNzI5NTExN119
-->