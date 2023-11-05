 powerful and highly customizable authentication and access-control framework.

* authentication (who are you?) 
```java 
public interface AuthenticationManager { 
	
	Authentication authenticate(Authentication authentication) throws AuthenticationException; 
}
```

* authorization (what are you allowed to do?).

Spring Security has an architecture that is designed to separate authentication from authorization





## 출처
1. [Spring Security공식문서](https://spring.io/projects/spring-security)
2. 