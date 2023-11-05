 powerful and highly customizable authentication and access-control framework.

1. authentication (who are you?) 
```java 
public interface AuthenticationManager { 
	
	Authentication authenticate(Authentication authentication) throws AuthenticationException; 
}
```

	* authenticationManager 의 구현체는 providerManager
	* authenticationProvider 객체들의 체인을 위임하고 있다.
	* 

```java 
public interface AuthenticationProvider { 
	
	Authentication authenticate(Authentication authentication) throws AuthenticationException; 
	
	boolean supports(Class<?> authentication); 
}
```


**1-1. authenticate 의 역할**
> 1. 입력 값이 신뢰있는 사용자를 나타내면 Authentication 를 return
> 2. 아니면, AuthenticationException (RunTime Exception)
> 3. 결정을 못하겠으면, null return



* authorization (what are you allowed to do?).

```

```

Spring Security has an architecture that is designed to separate authentication from authorization





## 출처
1. [Spring Security공식문서](https://spring.io/projects/spring-security)
2. 