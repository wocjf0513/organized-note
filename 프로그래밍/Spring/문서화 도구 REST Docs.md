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

## MockMvc (테스트 프레임워크)
	MockMVC를 사용하면 실제 웹 서버를 실행하지 않고도 컨트롤러의 동작을 시뮬레이트하고 테스트할 수 있습니다.

```java
@SpringBootTest 
@AutoConfigureMockMvc 
public class MyControllerTest { 
	@Autowired private MockMvc mockMvc; 
	@Test 
	public void testController() throws Exception{ mockMvc.perform(get("/my-endpoint")) 
	.andExpect(status().isOk()) 
	.andExpect(content().string("Hello, World!")); 
	} 
}
```

### 주의
* Json 형식 사용시
![[Pasted image 20231001033903.png]]
### 궁금한 거 해결
```
@Test  
void contextLoads() {  
  
}
```
 to verify if the application is able to load Spring context successfully or not.
 실제로 구현이 없는 contextLoad는 Spring context가 완벽하게 로드되는지 테스트한다.




## 의존성
* testImplementation 'org.springframework.restdocs:spring-restdocs-mockmvc'
### build.gradle

* plugin 추가
``` java
plugins {
    id 'org.springframework.boot' version '2.5.2'
    id 'io.spring.dependency-management' version '1.0.11.RELEASE'
    id 'java'
    id "org.asciidoctor.jvm.convert" version "3.3.2"
}

ext {  
    set('snippetsDir', file("build/generated-snippets"))  
}

tasks.named('test') {  
    outputs.dir snippetsDir  
    useJUnitPlatform()  
}  
  
tasks.named('asciidoctor') {  
    inputs.dir snippetsDir  
    dependsOn test  
}  
  
tasks.named('bootJar') {  
    from ("${asciidoctor.outputDir}/html5") {  
       into 'static/docs'  
    }  
    dependsOn asciidoctor  
}

task copyDocument(type: Copy) {
    dependsOn asciidoctor
    from file("build/docs/asciidoc")
    into file("src/main/resources/static/docs")
}

build {  
    dependsOn copyDocument  
}
```

## Test Code
```java
@Test  
public void testInfo() throws Exception {  
    this.mockMvc.perform(RestDocumentationRequestBuilders.get("/info")  
                .accept(MediaType.APPLICATION_JSON))  
          .andExpect(status().isOk())  
          .andDo(document("getInfo"));  
}
```
## index.adoc
```java
ifndef::snippets[]  
:snippets: ./build/generated-snippets  
endif::[]  
= Test Service  
  
== Test API  
=== getInfo()  
include::{snippets}/getInfo/httpie-request.adoc[]  
include::{snippets}/getInfo/http-response.adoc[]
```

## 자동 생성 Snippets
### Snippets이란?
: Snippet은 원본 자료의 일부를 선택적으로 가져와서 무엇인가를 설명하거나 보여주는 데 사용



* curl-request.adoc
* http-request.adoc
* http-response.adoc
* httpie-request.adoc
: HTTP 요청 및 응답을 인간 친화적인 형식으로 표시하는 기능을 제공

* request-body.adoc
* response-body.adoc


## 에러 해결
```
Unresolved directive in index.adoc
```
다음과 같은 에러시 ,
```
ifndef::snippets[]  
:snippets: ./build/generated-snippets  
endif::[]  
= Test Service  
  
== Test API  
=== getInfo()  
include::{snippets}/getInfo/httpie-request.adoc[]  
include::{snippets}/getInfo/http-response.adoc[]
```
경로 설정을 다음과 같이 한다. 