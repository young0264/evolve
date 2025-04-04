# CQS vs CQRS

#### ✅ CQS (Command Query Separation)

* **정의**: ‘함수는 \*\*추상 부수효과(Abstract Side Effect)\*\*를 발생시키만 안 된다.’ by Meyer
* **원칙 (Principle)**:
  *   "메서드는 **명령(Command)** 또는 **질의(Query)** 중 하나만 해야 한다.

      **둘 다 동시에 하면 안 된다.**"
* **의미**:
  *   **Command**: 무언가를 변경하지만 결과를 반환하지 않는다.

      예: `updateName(String name)` → void
  *   **Query**: 데이터를 반환하지만 상태는 변경하지 않는다.

      예: `getName()` → String
  *   **생성자 or 팩터리 메서드**

      예: new 혹은 of()로 객체 생성 → 어떤 외부 상태도 변경하지 않음. 단지 새로운 객체가 하나 생성.

      “위대한 오브젝트의 책에서 하나(오브젝트)를 꺼낸 것”에 불과함.
* **적용 대상**: 메서드 수준 (코드 설계의 원칙)
* **목적**: 사이드 이펙트를 분리하고 코드의 **명확성과 테스트 용이성** 확보

***

#### 고민

1.  JPA의 save()와 같은 저장 메서드는 CQS를 위반하는지?

    ```java
    User saved = userRepository.save(new User("Alice"));
    ```

    * DB라는 외부 시스템 상태를 변경험(Command)
    *   동시에 저장된 값을 반환함(Query)

        → 엄격하게 보면 CQS를 위반.

        하지만 많은 실용적인 시스템에서는 이를 예외로 간주함

        **Martin Fowler**

        > “스택의 pop()처럼 상태도 바뀌고 값을 반환하는 메서드는, 그 자체로 완결된 도메인의 행위이며 실용적 예외로 인정할 수 있다.”

        Meyer likes to use command-query separation absolutely, but there are exceptions. Popping a stack is a good example of a query that modifies state. Meyer correctly says that you can avoid having this method, but it is a useful idiom. So I prefer to follow this principle when I can, but I'm prepared to break it to get my pop.

        >
2.  생성자와 정적 팩터리 메서드는 CQS를 위반하는지?

    ```java
    User user = new User("Alice");
    User user = User.of("Alice");
    ```

    * 객체 생성 → 반환
    * 외부 상태(다른 객체나 시스템의 필드 등)를 변경하는 행동을 안함

    → 따라서, CQS 원칙을 위반하지 않음.

    **Bertrand Meyer**

    > [https://bertrandmeyer.com/wp-content/upLoads/OOSC2.pdf](https://bertrandmeyer.com/wp-content/upLoads/OOSC2.pdf) \
    > 내 어딘가..

#### ✅ CQRS (Command Query Responsibility Segregation)

*   **패턴 (Pattern)**:

    "시스템을 **Command 모델과 Query 모델로 분리**한다."
* **의미**:
  * **Command 모델**: 데이터 변경 책임 (쓰기 전용)
  * **Query 모델**: 데이터 조회 책임 (읽기 전용)
* 보통은 각 모델이 **별도의 객체/서비스/심지어 DB**를 가질 수 있음
* **적용 대상**: 애플리케이션 아키텍처 수준
* **목적**:
  * 읽기/쓰기 모델의 **확장성 분리**
  * 복잡한 도메인에서의 **성능 최적화**
  * **이벤트 소싱**과 함께 자주 사용됨

#### ✅차이점 요약

| 항목    | CQS                           | CQRS                                     |
| ----- | ----------------------------- | ---------------------------------------- |
| 의미    | 메서드 수준에서 명령과 조회를 분리           | 시스템 수준에서 쓰기/읽기 책임을 분리                    |
| 적용 대상 | 클래스/메서드 설계                    | 시스템 아키텍처                                 |
| 목적    | 사이드 이펙트 방지, 가독성 향상            | 확장성, 성능 분리, 복잡도 관리                       |
| 예시    | `getUser()` vs `updateUser()` | `UserCommandService`, `UserQueryService` |

> **\[참고]**

* [https://www.linkedin.com/pulse/spring-data-jpa의-save는-cqs-위반인가-toby-lee-plj0e/?trackingId=NznEwDSZS6apxv0%2FA4P4JQ%3D%3D](https://www.linkedin.com/pulse/spring-data-jpa%EC%9D%98-save%EB%8A%94-cqs-%EC%9C%84%EB%B0%98%EC%9D%B8%EA%B0%80-toby-lee-plj0e/?trackingId=NznEwDSZS6apxv0%2FA4P4JQ%3D%3D)
* [https://martinfowler.com/bliki/CommandQuerySeparation.html](https://martinfowler.com/bliki/CommandQuerySeparation.html)
* [https://en.wikipedia.org/wiki/Command–query\_separation?utm\_source=chatgpt.com](https://en.wikipedia.org/wiki/Command%E2%80%93query_separation?utm_source=chatgpt.com)

