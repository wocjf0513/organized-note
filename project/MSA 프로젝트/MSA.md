**: 작고, 독립적으로 배포 가능한 하나 이상의 서비스로 구성된 구조**
```
Microservice architectures are the ‘new normal’. Building small, self-contained, ready to run applications can bring great flexibility and added resilience to your code.
```

## 개요
간단한 서비스 두 개를 포함한 MSA구조의 프로젝트를 진행할 것이다.

## 연습 프로젝트 구조

![[Pasted image 20230929121824.png]]
* **Service Mesh** : 마이크로서비스 간 통신을 처리하기 위한 전용 인프라(기술적 환경) 레이어  
	a dedicated infrastructure layer that is responsible for handling communication between microservices in a distributed application.

* **Eureka Server** : MSA와 관련된 어플리케이션에서 서비스 등록, 검색(위치 및 엔드포인트 동적 검색) 및 로드밸런싱(트래픽 분산 기술)을 관리한다.

* **Gateway Server** : 네트워크 간 통신을 중개하거나 연걸하기 위해 사용되는 서버

### 프로젝트 설정
* Port 설정
	* Eureka Server : 8081
	* Gateway Server : 8082
	* User Service Server : 8091
	* Admin Service Server : 8092


## Eureka Server

#### 기능
* 서비스 검색 : 각각의 서비스 위치 파악
* 서비스 등록 : 각각의 서비스가 자신의 위치 정보를 특정 서버에 등록

#### 과정

![[Pasted image 20230929125706.png]]

1. Service Registry 기능을 할 Eureka Server 가 최초에 기동된다.
2. **Service Registry 서버인 Eureka Server** 에 등록될 서비스들이 기동된다. 여기서 **등록된 서비스는 Eureka Client** 라고 한다.
3. Eureka 서버는 자신에게 등록된 Eureka Client 에게 30초마다 Ping을 보내며 Health Checking을 수행한다.
4. 만약 30초마다 보내는 Heart Heat가 일정 횟수 이상으로 동작되지 않으면 Eureka Server는 해당 Client를 삭제한다.

## Gateway Server

![[Pasted image 20230929125849.png]]
#### 용어
* Route : URI와 Predicates, Filter를 이용 어디로 Routing할지 명시한다.
* Predicate : 조건 (예) predicated: -Path=/user/**
* Filter : 요청, 응답의 수정 및 헤더 조작이 가능하다.
* Gateway Hadler Mapping : Gateway를 Client의 요청으로부터 Mapping하는 작업을 한다.

#### 과정
![[Pasted image 20230929145246.png]]
1. Client 는 Spring Cloud Gateway 에 요청을 보낸다.
2. Gateway Handler Mapping 에서 해당 요청에 대한 Route와 Predicates가 일치한다고 판단하면 해당 요청은 Gateway Web handler로 보내진다.
3. handler 에서 Filter Chain 을 이용해서 **사전 필터** 혹은 **사후 필터**로 나누어 동작한다.
4. 필터링이 된 후 실제 마이크로서비스에게 전달된다.




## Gradle 명령어
* gradlew build --refresh-dependencies : 새로 추가한 의존성을 설치해준다.

## 하면서 배운 점
* actuator를 http로 이용하고 싶으면, spring-starter-web 의존성을 추가해야 된다.
* Spring Cloud Gateway는 Spring Boot 프로젝트로 개발된 것이며, 내부적으로 Spring Boot의 기능을 활용합니다. **Spring Cloud Gateway는 HTTP 요청을 라우팅하고 필터링하는 데 사용되며, 이러한 작업을 위해 Spring Boot의 웹 기능을 활용**합니다. 따라서 Spring Boot Starter Webflux를 추가해야 합니다.

## 에러
```
Network level connection to peer localhost; retrying after delay
```
➡️ eureka.client.service-url.defaultZone=http://localhost:${server.port}/eureka/
설정하거나 기본포트(8761)를 사용한다.

	Eureka 기본 포트는 8761이다. Eureka Server가 등록되어 있는 인스턴스에 ping하면서 자기자신을 호출하게 되는데,  기본 포트로 호출하게 되기 때문에 에러가 생긴다.

## 해결해야 될 문제
* actuator를 사용하고 싶은데, 의존성이 설치가 안된다. (gradle을 통해)
➡️ intellij cash를 삭제해줬다. ( shift x2=> invalidate cashes )

### 추가로 배워야 할 점
* webflux 와 web의 차이

### 참고 문헌

1. [MSA 프로젝트 구조 참고](https://wonit.tistory.com/506)
2. [Service Mesh 서버 참고](https://wonit.tistory.com/495?category=854728)
