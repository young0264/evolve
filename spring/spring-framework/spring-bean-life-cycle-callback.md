# Spring Bean Life-cycle Callback

*   스프링 빈 생명주기 콜백

    *   스프링 빈의 라이프 사이클 : 객체생성 → 의존관계 주입

        * 스프링빈은 객체 생성, 의존관계 주입이 끝난 다음에 필요한 데이터를 사용할 수 있는 준비가 되는 것.
        * 따라서 초기화 작업은 의존관계 주입이 끝난 후에 호출해야 함.
        * 스프링 빈에게 콜백 메서드를 통해서 초기화 시점을 알려줌으로써 의존관계 주입이 완료되었음을 알 수 있음.
        * 스프링 컨테이너가 종료되기 직전에 소멸 콜백을 줌


    *   스프링 빈의 이벤트 라이프 사이클

        1. 스프링 컨테이너 생성
        2. 스프링 빈 생성
        3. 의존관계 주입
        4. 초기화 콜백 (빈이 생성되고, 빈의 의존관계 주입이 완료된 후 호출)
        5. 사용
        6. 소멸전 콜백 (빈이 소멸되기 직전에 호출)
        7. 스프링 종료


    *   객체의 생성과 초기화를 분리

        * 생성자 : 필수 정보(파라미터)를 받고, 메모리를 할당해 객체를 생성함
        * 초기화 : 이렇게 생성된 값들을 활용해서 외부 커넥션을 연결하는 등 무거운 동작을 수행함
        * 따라서 생성시 초기화를 같이 하는 것 보다 생성과 초기화 하는 부분을 나누는 것이 유지보수 관점에서 좋다.


    *   싱글톤 빈

        * 스프링 컨테이너가 종료될 때 싱글톤 빈들도 함께 종료되기 때문에 스프링 컨테이너가 종료되기 직전에 소멸전 콜백이 일어남.
        * 컨테이너의 시작과 종료까지 생존하는 bean도 있고
        * 컨테이너와 무관하게 생명주기가 짧은 빈들도 있음. bean이 종료되기 직전에 소멸전 콜백이 일어남


    *   **3가지 방법**으로 **빈 생명주기 콜백을 지원**함

        1. 인터페이스(InitializingBean, DisposableBean)
        2. 설정 정보에 초기화 메서드, 종료 메서드 지정 (using @Bean)
        3. @PostConstruct, @PreDestroy 애노테이션 지원



        **(1) 인터페이스(InitializingBean, DisposableBean)** `implements InitializingBean, DisposableBean`

        * InitializingBean 은 afterPropertiesSet() 메서드로 초기화를 지원한다.
        * DisposableBean은 destroy() 메서드로 소멸을 지원한다.
        *   이러한 초기화, 소멸 인터페이스의 단점

            * 스프링 전용 인터페이스임
            * 코드가 스프링 전용 인터페이스에 의존함
            * 초기화 소멸 메서드의 이름 변경 불가능
            * 외부 라이브러리에 적용 불가능
            * 옛날 방식임



        **(2) 빈 등록 초기화, 소멸(종료) 메서드 지정**

        ```java
        @Configuration
        static class LifeCycleConfig {
            @Bean(initMethod = "init", destroyMethod = "close")
            public NetworkClient networkClient() {
                NetworkClient networkClient = new NetworkClient();
                networkClient.setUrl("http://hello-spring.dev");
                return networkClient;
            }
        }
        ```

        처럼 초기화, 소멸 메서드를 지정할 수 있다.

        * 메서드 이름을 자유롭게 지정 가능
        * 스프링 빈이 스프링 코드에 의존하지 않음
        * 코드가 아니라 설정 정보를 사용하기 때문에 코드를 고칠 수 없는 외부 라이브러리에도 초기화, 종료 메서드를 적용할 수 있음



        &#x20;**종료 메서드 추론**

        * @Bean 의 destroyMethod 속성에는 아주 특별한 기능이 있음.
        * 라이브러리는 대부분 close, shutdown 이라는 이름의 종료 메서드를 사용함
        * @Bean의 destroyMethod는 기본값이 (inferred) (추론)으로 등록되어 있음.
        * 이 추론 기능은 close, shutdown 이라는 이름의 메서드를 자동으로 호출해줌. 이름 그대로 종료 메서드를 추론해서 호출해줌
        * 따라서 직접 스프링 빈으로 등록하면 종료 메서드는 따로 적어주지 않아도 잘 동작함
        * 추론 기능을 사용하기 싫으면 destoryMethod=” ”처럼 빈 공백을 지정하면 됨

        \
        **(3)@PostConstruct, @PreDestroy 애노테이션**

        ```java
        import jakarta.annotation.PostConstruct;
        import jakarta.annotation.PreDestroy;
        ```

        * 스프링에서 권장하는 방법
          * 스프링에 종속적인게 아니라 JSR-250 자바 표준임
          * 스프링이 아닌 다른 컨테이너에서도 동작함
        * componentScan과 사용하기 유용
        *   단점

            * 외부 라이브러리에 적용하지 못함.
            * 외부 라이브러리를 초기화 혹은 종료 해야하면 @Bean기능을 사용하도록



    **정리**

    * 자바 표준인 @PostConstruct, @PreDestroy 애노테이션 사용 권장
    *   코드 수정이 불가능한 외부 라이브러리를 초기화, 종료 시점에 어떤 작업을 해야할 경우엔 기능 사용을 권장

        ```java
         @Bean(initMethod = "init", destroyMethod = "close")
        ```
