---
title: "ModelMapper를 사용해서 도메인을 DTO로 매핑"
tags: ["jpa"]
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

### Service.java
```java java
import org.modelmapper.ModelMapper;

public class UserService {
	@NonNull  
	private ModelMapper modelMapper;

	@NonNull  
	private UserRepository userRepository;

	public Page<UserDto> getAllUsers(String type, String search, Pageable pageable) {  
	  Page<User> user = this.userRepository.findAllByEmailContaining(search, pageable);  
	  
	  List<UserDto> list = user.getContent()  
	  .stream()  
	  .map(i -> modelMapper.map(i, UserResponse.class)).collect(Collectors.toList());  
	  
	  return new PageImpl<>(list, pageable, user.getTotalElements());  
	}
}
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTQyNTY1ODIzNF19
-->
