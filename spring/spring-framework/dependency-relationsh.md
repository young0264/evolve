# Dependency Relationsh

* 의존관계 주입 방법
  1. 생성자 주입
  2. 수정자 주입(setter)
  3. 필드 주입
  4.  일반 메서드 주입



      *   **생성자 주입**

          * 생성자를 통해 의존관계 주입
          *   생성자 호출 시점(생성시)에 1번 호출되는 것이 보장됨

              → 불변, 필수 의존관계에 사용
          * 생성자가 1개일 시, @Autowired를 생략해도 자동 주입됨 (spring container에 bean으로 등록되어있다는 전제)

          ```java
          public class OrderServiceImpl implements OrderService {

              private final MemberRepository memberRepository;
              private final DiscountPolicy discountPolicy;

              public OrderServiceImpl(MemberRepository memberRepository,
                                      DiscountPolicy discountPolicy) {  // 외부로부터 결정
                  this.memberRepository = memberRepository;
                  this.discountPolicy = discountPolicy;
              }
          }
          ```



      *   **수정자 주입(setter)**

          * setter라 불리는 필드 값을 setter 메서드를 통해 주입하는 방법

          ```java
           @Component
          public class OrderServiceImpl implements OrderService {
              private MemberRepository memberRepository;
              private DiscountPolicy discountPolicy;
              @Autowired
              public void setMemberRepository(MemberRepository memberRepository) {
                  this.memberRepository = memberRepository;
              }
               @Autowired
              public void setDiscountPolicy(DiscountPolicy discountPolicy) {
                  this.discountPolicy = discountPolicy;
              }
          }
          ```

          * `@Autowired` 의 기본 동작은 주입할 대상이 없으면 오류가 발생한다. 주입할 대상이 없어도 동작하게 하 려면 `@Autowired(required = false)` 로 지정



      *   **필드 주입**

          * 필드에 바로 주입하는 방식.
          * 코드 간결, 하지만 외부에서 변경불가능한 단점
          * DI 프레임워크(spring등등)가 없으면 변경 불가능
          * 사용 지양
          * _**참고:**_ 순수한 자바 테스트 코드에는 당연히 @Autowired가 동작하지 않는다. `@SpringBootTest` 처럼 스프링 컨테이너를 테스트에 통합한 경우에만 가능하다.
          * _**참고:**_ 다음 코드와 같이 `@Bean` 에서 파라미터에 의존관계는 자동 주입된다. 수동 등록시 자동 등록된 빈의 의존 관계가 필요할 때 문제를 해결할 수 있다.

          ```java
          @Component
          public class OrderServiceImpl implements OrderService {
              @Autowired
              private MemberRepository memberRepository;
              @Autowired
              private DiscountPolicy discountPolicy;
          }
          ```



      * **일반 메서드 주입**
        * 일반 메서드를 통해 주입받음
        * 한번에 여러 필드를 주입받을 수 있음.
        * 사용 잘 안함
        *   의존관계 자동 주입은 스프링 컨테이너가 관리하는 스프링 빈이어야 동작한다. 스프링 빈이 아닌 `Member` 같은 클래스에서 `@Autowired` 코드를 적용해도 아무 기능도 동작하지 않는다.

            ```java
             @Component
            public class OrderServiceImpl implements OrderService {
                private MemberRepository memberRepository;
                private DiscountPolicy discountPolicy;
                @Autowired
                public void init(MemberRepository memberRepository, DiscountPolicy
              discountPolicy) {
                     this.memberRepository = memberRepository;
                     this.discountPolicy = discountPolicy;
            } }
            ```



* **생성자 주입을 추천**
  1. 불변
     1. 객체를 생성할 때 1번 호출되므로 호출될 일이 없음. → 불변하게 설계
     2. 수정자주입은 set으로 열어두는게 단점
        1. 누군가 실수로 변경가능성,
        2. 변경하면 안되는 메서드를 열어두는건 좋은 설계 방법이 아님
     3. 프레임워크에 의존하지 않는, 순수 자바 언어 특징을 살리는 방식



* _**final 키워드**_
  * 생성자 주입을 사용하면 필드에 `final` 키워드를 사용할 수 있음
  * `필드를 final`로 설정하면 생성자에서 값을 넣어주지 않으면 `**컴파일에러**`가 뜸
