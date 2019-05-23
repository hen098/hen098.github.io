---
title: "Spring boot2에 Swagger2 사용하기"
tags: ["java","springboot"]
---
API 단을 만들어 문서관리하는 것이 꼭 필요하다. 
잦은 변화에 맞추어 수작업으로 문서를 변경하는 것은 번거로운 일이다.

API 문서화를 자동화 해주는 도구가 있다. 

## Swagger
간단한 설정으로 프로젝트와 연결되어 API에 대한 문서화를 HTML 화면으로 확인할 수 있다.

## Spring Boot 에서 사용방법

### Gradle 의존성 추가
```
compile group: 'io.springfox', name: 'springfox-swagger2', version: '2.8.0'  
compile group: 'io.springfox', name: 'springfox-swagger-ui', version: '2.8.0'
```
Spring boot 2 이상부터는 2.8.0이상을 사용하라고 한다.

### SwaggerConfig.java
Spring Boot2 대 버젼은 Swagger2가 작동하지 않는 부분이 있다.
(Spring Boot 방법으로 구현하면, 404 에러가 발생한다.)
그래서 Spring Boot 설정방법이 아닌 부트를 사용하지 않는 방법으로 구현해야 한다.
```java java
@Component 
@EnableSwagger2  
public class SwaggerConfig extends WebMvcConfigurerAdapter {  
  @Bean  
  public Docket productApi() {  
  return new Docket(DocumentationType.SWAGGER_2)  
  .select()  
  .apis(RequestHandlerSelectors.basePackage("kr.co.apexsoft.jpaboot"))  
  .paths(PathSelectors.ant("/**"))  
  .build();  
  
  }  
  
  protected void addResourceHandlers(ResourceHandlerRegistry registry) {  
  registry.addResourceHandler("swagger-ui.html")  
  .addResourceLocations("classpath:/META-INF/resources/");  
  
  registry.addResourceHandler("/webjars/**")  
  .addResourceLocations("classpath:/META-INF/resources/webjars/");  
  }
}
```
### Security url 열어주기
`SwaggerConfig`에서 설정한 url의 보안을 열어주어야 한다.
```java java
.antMatchers(
		"/v2/api-docs",  
		"/swagger-resources/**",  
		"/swagger-ui.html",  
		"/webjars/**"
).permitAll();
```
### html에서 확인
localhost:8080/swagger-ui.html 접속
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTUyNDIyMDMyNiwxNzkxMzUwNjg2XX0=
-->
