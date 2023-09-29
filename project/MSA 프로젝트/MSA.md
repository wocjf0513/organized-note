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

### Eureka Server

#### 기능
* 서비스 검색 : 각각의 서비스 위치 파악
* 서비스 등록 : 각각의 서비스가 자신의 위치 정보를 특정 서버에 등록

#### 과정

![[Pasted image 20230929125706.png]]

1. Service Registry 기능을 할 Eureka Server 가 최초에 기동된다.
2. **Service Registry 서버인 Eureka Server** 에 등록될 서비스들이 기동된다. 여기서 **등록된 서비스는 Eureka Client** 라고 한다.
3. Eureka 서버는 자신에게 등록된 Eureka Client 에게 30초마다 Ping을 보내며 Health Checking을 수행한다.
4. 만약 30초마다 보내는 Heart Heat가 일정 횟수 이상으로 동작되지 않으면 Eureka Server는 해당 Client를 삭제한다.


















### Gateway Server

![[Pasted image 20230929125849.png]]

### Gradle 명령어
* gradlew build --refresh-dependencies : 새로 추가한 의존성을 설치해준다.

### 참고 문헌

1. [MSA 프로젝트 구조 참고](https://wonit.tistory.com/506)
2. [Service Mesh 서버 참고](https://wonit.tistory.com/495?category=854728)
