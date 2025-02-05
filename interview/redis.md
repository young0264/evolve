# Redis가 싱글 스레드로 만들어진 이유

*   **\[250205\_수] Redis가 싱글 스레드로 만들어진 이유**

    **Redis**가 **단일 스레드(single-threaded)**&#xB85C; 설계된 이유는 주로 **성능 최적화, 복잡성 감소, 데이터 일관성 유지**에 있음.

    **단일 스레드 모델**은 멀티스레드 모델에 비해 설계와 구현이 상대적으로 간단함. **멀티스레드 환경**에서는 동시성문제(race condition, dead-lock등)를 처리하기 위해 복잡한 동기화 메커니즘이 필요하지만, 단일 스레드 환경에서는 이런 문제를 자연스럽게 회피할 수 있음

    동시에 여러 스레드가 동일한 데이터를 수정하려고 할 때 발생할 수 있는 데이터 불일치 문제를 방지함. 모든 명령어가 순차적으로 처리되기 때문에, 복잡한 락(lock) 메커니즘없이도 데이터의 일관성을 자연스럽게 유지할 수 있음.

    **Redis**는

    * 주로 메모리 내에서 빠르게 수행되는 I/O 작업을 처리하는 인메모리 데이터베이스로 설계되어, 매우 빠른 응답 시간을 제공
    * **단일 스레드 이벤트 루프(event loop)를 사용**함으로써 컨텍스트 스위칭(Context Switching) 에 소요되는 오버헤드 최소화
    * 이벤트 기반(event-driven) 아키텍처를 채택하여 요청을 효율적으로 처리함. 단일 스레드 이벤트 루프는 비동기적으로 여러 클라이언트의 요청을 처리할 수 있어, 높은 동시성 구현 가능
    * 멀티스레드 모델에서는 이러한 비동기 처리의 이점을 충분히 활용하기 어려울 수 있음
    * Redis 6.0 부터 클라이언트로부터 전송된 **네트워크를 읽는 부분과 전송하는 I/O 부분은 멀티 스레드를 지원**함. 하지만 실행하는 부분은 싱글 스레드로 동작하기 때문에 기존과 같이 Atomic을 보장함.

    > \[참고]
    >
    > * 6.0 내용: [Redis - Diving Into Redis 6.0](https://redis.io/blog/diving-into-redis-6/)
    > * [Why the heck Single-Threaded Redis is Lightning fast? Beyond In-Memory Database Label](https://www.linkedin.com/pulse/why-heck-single-threaded-redis-lightning-fast-beyond-in-memory-kapur/)
    > * [\[입 개발\] Redis 6.0 – ThreadedIO를 알아보자.](https://charsyam.wordpress.com/2020/05/05/%EC%9E%85-%EA%B0%9C%EB%B0%9C-redis-6-0-threadedio%EB%A5%BC-%EC%95%8C%EC%95%84%EB%B3%B4%EC%9E%90/)

ㄴ **인메모리 데이터베이스(In-Memory Database, IMDB)?**

**인메모리 데이터베이스(In-Memory Database, IMDB)**&#xB294; 디스크(HDD, SSD)가 아닌 **RAM(메모리)**&#xC5D0; 저장하고 처리하는 데이터베이스.

이 방식 덕분에 훨씬 빠른 데이터 접근 속도 제공, 실시간 분석, 캐싱, 트랜잭션처리 등 중요한 애플리케이션에서 많이 사용됨.
