# Strategy for Multiple Bean

1. @Autowired 필드 명 매칭
2. @Qualifier → @Qualifier 끼리 매칭 → 빈 이름 매칭
3. @Primary 사용



***



**@Autowired 필드 명 매칭**

* @Autowired는 타입 매칭을 시도하고, 이때 여러 빈이 있으면 필드의 이름, 파라미터 이름으로 빈 이름을 추가 매칭한다.
*   먼저 타입 매칭을 시도. 그 후 여러 빈이 있을 때 추가로 동작하는 기능이 필드명 매칭

    ```java
     //전
     @Autowired
     private DiscountPolicy discountPolicy

     //후 : 필드명을 빈 이름으로 변경 'rateDiscountPolicy'
     @Autowired
     private DiscountPolicy rateDiscountPolicy
     
    ```



**@Qualifier → @Qualifier 끼리 매칭 → 빈 이름 매칭**

*   @**Qualifier**는 추가 구분자를 붙여주는 방법

    주입시 추가적인 방법을 제공하는 것이지 빈 이름을 변경하지는 않음
* 생성자나 수정자 자동 주입 방법에도 사용 가능
*   즉 (1). @Qualifier끼리 매칭, (2) bean이름 매칭, (3)NoSuchBeanDefinitionException 예외 발생과 같은 순서라고 보면 됨

    ```java
    @Component
    @Qualifier("mainDiscountPolicy")
    public class RateDiscountPolicy implements DiscountPolicy {}

    @Component
    @Qualifier("fixDiscountPolicy")
    public class FixDiscountPolicy implements DiscountPolicy {}
    ```



**@Primary 사용**

* 우선순위를 정하는 방법.
*   @Autowired시에 여러 빈이 매칭된다면 @Primary가 우선권을 가짐

    위의 코드에서 우선적으로 넣을 할인 정책에 @Primary를 사용

    ```java
    //예
    @Component
    @Primary //제외시 FixDiscountPolicy와 NoUniqueBeanDefinitionException 터짐
    public class RateDiscountPolicy implements DiscountPolicy
    ```



**@Qualifier vs @Primary**

* **@Qualifier**의 단점은 주입 받을 때 모든 코드에 @**Qualifier** 를 붙여줘야 함
* 메인 db connection spring bean , Sub db connection spring bean이 있다고 가정할 때 main connection을 획득하는 쪽에서는 @Primary, sub connection을 획득하는 쪽에서는 @Qualifier를 사용하도록 설계할 수 있다.
* @Primary는 기본값처럼, @Qualifier 는 상세한 동작 제어 → @Qualifier가 @Primary보다 우선권이 높음.



**(custom) annotaion 직접 만들기**

*   @Qualifier("mainDiscountPolicy") 와 같이 사용하면 compile시 타입체크가 불가능.

    ```java

     package hello.core.annotataion;
     import org.springframework.beans.factory.annotation.Qualifier;
     import java.lang.annotation.*;
     @Target({ElementType.FIELD, ElementType.METHOD, ElementType.PARAMETER,
     ElementType.TYPE, ElementType.ANNOTATION_TYPE})
     @Retention(RetentionPolicy.RUNTIME)
     @Documented
     @Qualifier("mainDiscountPolicy")
     public @interface MainDiscountPolicy {
    }
    ```
* RequiredArgsConstructor로 생성되는 생성자를 통해 의존성을 주입할 때에Qualifier를 명시하지 못함. 직접 생성자를 만들어야함.
* 조회한 빈이 모두 필요할 때(List, Map) → \*\*spring을 통한 전략패턴(\*\*AllBeanTest.class 파일 참고)



**자동/수동 bean 등록의 올바른 실무 운영 기준**

* 업무 로직 빈(controller, service, repository 등) 일때는 숫자가 많고 상대적으로 에러 포인트를 파악하기 쉽기에 자동 빈 등록 기능을 적극적으로 사용하는 것을 추천
* 기술지원 빈일 때는 app에 광범위하게 영향을 미침. 이런 경우 수동빈등록을 사용하여 작ㄱ업 포인트를 명확하게 드러내는 것이 좋음.
