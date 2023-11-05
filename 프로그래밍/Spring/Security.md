 powerful and highly customizable authentication and access-control framework.

1. authentication (who are you?) 
```java 
public interface AuthenticationManager { 
	
	Authentication authenticate(Authentication authentication) throws AuthenticationException; 
}
```

	* authenticationManager 의 구현체는 providerManager
	* authenticationProvider 객체들의 체인을 위임하고 있다.

```java 
public interface AuthenticationProvider { 
	
	Authentication authenticate(Authentication authentication) throws AuthenticationException; 
	
	boolean supports(Class<?> authentication); 
}
```

WebSecurityConfigurerAdapter 함수 상속 시 ,configure 함수를 반드시 작성해야 하는데, 
함수 위에 1) @Override 2) @Autowired 에 따라 Global AuthenticationManagerBuilder를 사용하거나, 로컬 AuthenticationManagerBuilder를 사용하는 것으로 나눠진다.

**1-1. authenticate 의 역할**
> 1. 입력 값이 신뢰있는 사용자를 나타내면 Authentication 를 return
> 2. 아니면, AuthenticationException (RunTime Exception)
> 3. 결정을 못하겠으면, null return



![[Pasted image 20231105163921.png]]



* authorization (what are you allowed to do?).

```

```

Spring Security has an architecture that is designed to separate authentication from authorization





## 출처
1. [Spring Security공식문서](https://spring.io/projects/spring-security)
2. 