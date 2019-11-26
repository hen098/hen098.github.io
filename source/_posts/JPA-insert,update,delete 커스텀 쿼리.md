-
JPA에서 insert, update, delete custom 쿼리를 작성할 때, 아래처럼 수행한다.

```java
public interface UserRepository extends JpaRepository<User, Long> {
@Transactional  
@Modifying  
@Query(value = "update User u set u.locked = true " +  
  "where u.userId =:userId and u.failCnt > :failMaxCnt")  
int updateLock(@Param("userId")String userId, @Param("failMaxCnt")Integer failMaxCnt);
}
```
### `@Query`&`@Modifying`
parameter를 받는 insert, update, delete 커스텀 쿼리는 `@Query` 어노테이션을 통해서 쿼리를 작성한다.
`@Query`는 `@Modifying`을 함께 써주어야 한다.

### `@Transactional`
`JpaRepository`는 기본이 `@Transactional(readOnly=true)`이다. 
insert, update, delete의 경우 `@Transactional` 항상 필요하다.

참고 [https://codar.club/blogs/5cd7f06bec80a.html](https://codar.club/blogs/5cd7f06bec80a.html)
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEwMjE5NzMyMjEsMTA1ODIwODA4OF19
-->