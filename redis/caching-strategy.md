# Caching Strategy

\[읽기 전략]

1.  \*\*Look Aside(== Cache Aside)\*\*패턴: 캐시가 없을 때 DB에서 조회함.

    * 캐시가 있는지 먼저 체크하고, 없는 경우(miss)에만 DB에 접근함
    * DB가 (정답) 제대로 된 데이터, 캐시는 보조수단, 가장 많이 사용됨

    → Best for: Systems with a high read-to-write ratio, like e-commerce sites.

    *   code

        * 스프링은 기본적으로 Look-Aside 패턴을 사용함.

        ```java
        @Cacheable(value = "products", key = "#id")
        public Product getProduct(String id) {
            return db.findById(id);
        }
        ```
2.  **Read Through**:

    * Cache Miss ←→ Cache System(ex: Redis) ←→ DB
      * 캐시에 데이터 없으면 캐시가 DB 조회, 결과를 캐시에 저장 및 클라이언트에 반환
    * 즉, 데이터 조회 책임을 캐시 계층에 위임하는 방식
    * 애플리케이션은 캐시만 바라보며, cache miss나 DB 접근은 신경쓰지 않음.

    → Read-heavy apps like CDNs, social media feeds, and user profiles.

\[쓰기 전략]

1.  **Write Through(동기)**: 캐시와 DB 모두에 저장함.

    * 데이터 쓰기 시 캐시와 DB 모두 동시에 기록
    * 항상 캐시와 DB가 동기화된 상태 유지
    * (순서) 쓰기 요청 → 캐시 + DB 저장
    * 장점: 캐시 miss 걱정이 없고(언제나 최신 데이터 존재) 언제나 최신 데이터 일관성 유지에 용이함
    * 단점: 쓰기 성능이 DB 성능에 종속됨(DB가 느려지면 전체 지연)

    → Best for: Consistency-critical systems, such as financial apps.
2. **Write Back(Write Behind)(비동기)**: 데이터를 캐시에 먼저 저장, 이후 DB에 비동기로 씀. 쓰기 레이턴시(대기시간) 최소화.
   * 우선 캐시 저장, 나중에 비동기적으로 DB에 저장
   * 쓰기 속도가 매우 빠름
   * (순서)
     * 쓰기 요청 → 캐시에만 저장
     * 일정 시간 후(배치 혹은 트리거) → DB에 저장
   * 장점: 쓰기 처리 속도 빠름(캐시에만 저장), 트래픽 급증시 DB 부하 줄이기 가능
   * 단점: 캐시 손실 시 데이터 유실 가능성 있음, 구현 복잡도가 높음(지연 저장, 실패 재시도 등 고려 필요함)
3. **Cache invalidation(삭제)**
   * 캐시에 있는 값을 삭제하거나 무효화하는 동작
   * 데이터의 정합성을 유지하기 위해 캐시를 지움
4.  **Write Around**: 캐시를 거치지 않고 DB에 바로 저장. 읽을 때 캐시를 업데이트함

    * DB에만 우선 저장을 하고 캐시 저장은 건너뜀. 데이터를 조회하는 요청이 오면 그때 가져온 데이터를 캐시에 저장하는 방식.
      * 이후 조회시에 캐시에 적재됨(Look Aside와 비슷함)
    * (순서)
      * 쓰기 요청 → DB 저장
      * 이후 읽기 → 캐시 miss → DB 조회 → 캐시 저장
    * 장점: 불필요한 캐시 쓰기 줄여서 효율 높아짐
    * 단점: 첫 조회 시 캐시 miss 발생 가능성이 높음

    → Write-heavy systems where data isn’t immediately needed, like logging systems

→ Best for: High-write throughput systems, such as social media feeds.

<figure><img src="../.gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>

> \[참고] [https://www.linkedin.com/posts/ashishps1\_top-5-caching-strategies-explained-1-𝐑𝐞𝐚𝐝-activity-7309431214296178688-S6Av/?utm\_source=share\&utm\_medium=member\_android\&rcm=ACoAAC3MX\_QBs8sraw\_w5APeiR7f9aopDa2GZhc](https://www.linkedin.com/posts/ashishps1_top-5-caching-strategies-explained-1-%F0%9D%90%91%F0%9D%90%9E%F0%9D%90%9A%F0%9D%90%9D-activity-7309431214296178688-S6Av/?utm_source=share\&utm_medium=member_android\&rcm=ACoAAC3MX_QBs8sraw_w5APeiR7f9aopDa2GZhc) [https://inpa.tistory.com/entry/REDIS-📚-캐시Cache-설계-전략-지침-총정리#look\_aside\_패턴](https://inpa.tistory.com/entry/REDIS-%F0%9F%93%9A-%EC%BA%90%EC%8B%9CCache-%EC%84%A4%EA%B3%84-%EC%A0%84%EB%9E%B5-%EC%A7%80%EC%B9%A8-%EC%B4%9D%EC%A0%95%EB%A6%AC#look_aside_%ED%8C%A8%ED%84%B4)
