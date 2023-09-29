**: 작고, 독립적으로 배포 가능한 하나 이상의 서비스로 구성된 구조**
```
Microservice architectures are the ‘new normal’. Building small, self-contained, ready to run applications can bring great flexibility and added resilience to your code.
```

### 개요
간단한 서비스 두 개를 포함한 MSA구조의 프로젝트를 진행할 것이다.

### 프로젝트 구조

![[Pasted image 20230929121824.png]]
* **Service Mesh** : 마이크로서비스 간 통신을 처리하기 위한 전용 인프라(기술적 환경) 레이어  
	a dedicated infrastructure layer that is responsible for handling communication between microservices in a distributed application.

* **Eureka Server** : MSA와 관련된 어플리케이션에서 서비스 등록, 검색(위치 및 엔드포인트 동적 검색) 및 로드밸런싱을 관리한다.
	* 로드밸런싱 : 트래픽 분산시키는 기술

* **Gateway Server** : 네트워크 간 통신을 중개하거나 연걸하기 위해 사용되는 서버





### 참고 문헌

1. [MSA 참고 블로그](https://wonit.tistory.com/506)
