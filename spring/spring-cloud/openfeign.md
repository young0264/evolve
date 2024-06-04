# OpenFeign

### 유래

Feign은 netflix에서 만들어지고 배포된 프로젝트다. 오늘날 이건 오픈소스 프로젝트로 남아있다.

Spring Cloud Netflix Feign은 Netflix OSS와 통합한 spring cloud system이다. 하지만 Feign은 Feign에만 access할 수 있도록 별도의 spring cloud starter 환경이 주어진다.

OpenFeign은 netflix가 Feign을 내부적으로 운영하는 것을 중단하겠다란 결정을 내린 후 Feign을 새 프로젝트로 이전한 것을 OpenFeign이라 부름.

***



### 개요

마이크로 서비스 간 동기 통신. 기능, 관심사, 도메인 등등 특정 역할에 의해 분리해 마이크로서비스  간 협력이나 트랜잭션을 수행해야 할 때 가장 쉽게 사용하는 것이 REST API를 활용한 동기 통신.

이때 서버측에서는 다른 서버에 통신을 보내는 동기 통신 클라이언트가 있어야 하는데, 대표적으로 RestTemplaate과 Spring Cloud OpenFeign이 있음.



### Spring Cloud OpenFeign

Spring Cloud에 포함된 동기 통신 클라이언트 프로젝트. 선언적 REST 클라이언트로 웹 서비스 클라이언트 작성을 보다 쉽게 할 수 있음. 인터페이스 선언만으로 자동으로 구현체가 생성됨



### RestTemplate과 Spring Cloud OpenFeign의 차이점

RestTemplate과 Spring Cloud OpenFeign의 주요 차이점

*   명령형 API vs 선언적 API

    * RestTemplate은 명령형 API, API로 HTTP 요청을 생성하고 수행함
    * Spring Cloud OpenFeign은 선언적 API로 인터페이스를 정의, 어노테이션을 사용하여 해당 인터페이스를 통해 HTTP 클라이언트를 생성 -> 명시적으로 HTTP 요청을 작성하지 않고 원격 서비스 호출이 가능


*   자동 구성

    * RestTemplate은 RestTemplate 빈 등록 및 호출, 메서드를 통한 명령을 직접 처리해야 함.
    * Spring Cloud OpenFeign은 인터페이스 선언으로 클라이언트 구현체가 자동으로 구성됨


* URL 템플릿
  * RestTemplate은 URL 템플릿을 사용해 동적인 요청 URL을 생성할 수 있음
  *   Spring Cloud OpenFeign은 어노테이션을 사용하여 동적인 URL을 정의할 수 있음







***

참고

* feign 적용기(우아한) : [https://techblog.woowahan.com/2630](https://techblog.woowahan.com/2630/)
* baeldung : [https://www.baeldung.com/netflix-feign-vs-openfeign](https://www.baeldung.com/netflix-feign-vs-openfeign)
* spring docs : [https://docs.spring.io/spring-cloud-openfeign/docs/current/reference/html/](https://docs.spring.io/spring-cloud-openfeign/docs/current/reference/html/)
* netflix Feign과 OpenFeign의 차이 : [https://www.baeldung.com/netflix-feign-vs-openfeign](https://www.baeldung.com/netflix-feign-vs-openfeign)
