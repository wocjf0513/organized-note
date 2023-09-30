: API 명세를 쉽게 해주는 프레임워크다.
```markdown
원하는 형태의 파일로 변환이 가능하며, **테스트 코드 기반으로 API명세가 만들어진다**는 강점이 있다.

이는 스웨거와는 다른 점이다. 
아무래도 테스트 코드에 명세와 관련된 코드를 적기 때문에 
비즈니스 로직과 관련된 코드를 작성할 때, 코드가 많이 작성되는 걸 
막을 수 있다.
```

## 프로젝트 의존성
```
implementation 'org.springframework.boot:spring-boot-starter-web'  
compileOnly 'org.projectlombok:lombok'  
developmentOnly 'org.springframework.boot:spring-boot-devtools'  
annotationProcessor 'org.projectlombok:lombok'  
testImplementation 'org.springframework.boot:spring-boot-starter-test'  
testImplementation 'org.springframework.restdocs:spring-restdocs-mockmvc'
```

## MockMvc



### 궁금한 거 해결
```
@Test  
void contextLoads() {  
  
}
```
 to verify if the application is able to load Spring context successfully or not.
 실제로 구현이 없는 contextLoad는 Spring context가 완벽하게 로드되는지 테스트한다.

