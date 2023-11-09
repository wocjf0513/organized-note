 powerful and highly customizable authentication and access-control framework.
 
 Spring Security has an architecture that is designed to separate authentication from authorization



1. **authentication (who are you?)** 
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


authentication 이후 과정

* **authorization (what are you allowed to do?).**

	AccessDecisionManager - AccessDecisionVoter
	
	**# Access Control 결정을 내리는 인터페이스로, 구현체 3가지를 기본으로 제공한다.**
	
	- **AffirmativeBased** : 여러 Voter 중에 한명이라도 허용하면 허용, 기본전략
	
	- ConsensusBased : 다수결
	
	- UnanimousBased : 만장일치

```java
public AccessDecisionManager accessDecisionManager() {
        RoleHierarchyImpl roleHierarchy = new RoleHierarchyImpl();
        roleHierarchy.setHierarchy("ROLE_ADMIN > ROLE_USER");

        DefaultWebSecurityExpressionHandler handler = new DefaultWebSecurityExpressionHandler();
        // handler 도 같은 걸 쓰고 있는데, roleHierarchy 설정을 추가해준 것 뿐임
        handler.setRoleHierarchy(roleHierarchy);

        WebExpressionVoter webExpressionVoter = new WebExpressionVoter();
        webExpressionVoter.setExpressionHandler(handler);

        List<AccessDecisionVoter<? extends Object>> voters = Arrays.asList(webExpressionVoter);
        return new AffirmativeBased(voters);
    }

    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http.authorizeRequests()
                .mvcMatchers("/", "/info", "/account/**").permitAll()
                .mvcMatchers("/admin").hasRole("ADMIN")
                .mvcMatchers("/user").hasRole("USER")
                .anyRequest().authenticated()
                .accessDecisionManager(accessDecisionManager());

            http.formLogin();
            http.httpBasic();
    }
```

```java
@SpringBootApplication
@EnableGlobalMethodSecurity(securedEnabled = true)
public class SampleSecureApplication {
}
```

```java
@Service
public class MyService {

  @Secured("ROLE_USER")
  public String secure() {
    return "Hello Security";
  }

}
```

현재 인증된 사용자 정보를 가져올 때

```java
@RequestMapping("/foo")
public String foo(@AuthenticationPrincipal User user) {
  ... // do stuff with user
}
```

Authentication. get Principal() 함수가 작용한다.


![[Pasted image 20231105183849.png]]
필터는 다음과 같은 체인 구조를 가지며, 순서에 맞게 처리된다.


SpringSecurity는 thread- bounded 되기 때문에 

```java

@Configuration
@EnableAsync
public class AsyncConfig {

    @Bean
    public Executor customExecutor() {
        ThreadPoolTaskExecutor executor = new ThreadPoolTaskExecutor();
        executor.setCorePoolSize(5);
        executor.setMaxPoolSize(5);
        executor.setThreadNamePrefix("5bepoz");
        executor.initialize();
        return executor;
    }

    @Bean
    public Executor customExecutor2() {
        ThreadPoolTaskExecutor executor = new ThreadPoolTaskExecutor();
        executor.setCorePoolSize(10);
        executor.setMaxPoolSize(10);
        executor.setThreadNamePrefix("10bepoz");
        executor.initialize();
        return executor;
    }
}

@Service
@Slf4j
public class AsyncService {

    @Async("customExecutor")
    public void call() {
        log.info("async Test " + Thread.currentThread());
    }
}


```



## 출처
1. [Spring Security공식문서](https://spring.io/projects/spring-security)

jwt + spring security 
1. login과 회원가입 페이지를 security를 이용해서 모두가 들어갈 수 있게 해줌.
2. 사용자가 login페이지에 아이디, 비번을 넣어서 로그인하거나
3. 회원가입하면 저희가 controller랑 service를 이용해서 jwt token을 발급해줄거에요
5. 그런다음에 사용자 login, 회원가입 페이지 이외에 페이지를 들어가고 싶으면
6. 발급한 jwt token을 브라우저의 쿠키에 담아서 페이지 요청을 하게 되고
7. 이떄, security에서 저희가 작성한 custom filter가 돌면서 맞는 token인지 확인하고 security context holder에 저장한다음, 인증 절차를 끝내요.



### JWT

![[Pasted image 20231109211723.png]]


![[Pasted image 20231109211731.png]]



