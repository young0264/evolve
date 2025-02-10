# 방어적 복사에 대해서 설명

**방어적 복사(Defensive Copy)**

* 방어적 복사를 사용할 경우, 외부에서 객체를 변경해도 내부의 객체는 변경되지 않음 (원본과의 참조를 끊은 복사본을 만들어 사용하는 방식)
* 원본의 변경에 의한 예상치 못한 사이드 이펙트를 방지하여 안전한 코드를 만들 수 있는데 도움이 됨
* 방어적 복사 ≠ 깊은 복사 (서로 다름)

1. 생성자의 인자로 받은 객체의 복사본을 만들어 내부 필드를 초기화
2. getter 메서드에서 객체를 반환할 때, 복사본을 만들어 반환할 수 있음

컬렉션 자료구조를 반환하는 경우라면 자바의 Unmodifiable Collection을 사용하여, 외부에서 Collection에 대해 조회만 할 수 있도록 강제할 수 있음. 자바에서 Unmodifiable Collection은 set(), add(), addAll()처럼 컬렉션에 요소를 추가하거나 변경하는 메서드를 사용하는 경우, 예외를 발생시킴.

```java
public class Lotto {

  private final List<LottoNumber> numbers;

  public Lotto(List<LottoNumber> numbers) {
      validateSize(numbers);
      this.numbers = new ArrayList<>(numbers);  // 방어적 복사
  }
}
```

**`위 코드의 두가지 문제`**

1.  `LottoNumber` 가 변경될 여지가 있음

    (해결방법)

    1. List\<Integer>로 바꾸거나
    2.  내부객체(LottoNumber)까지 **깊은 복사**를 수행

        * 아래의 경우 디버깅도 너무 힘듦(테코블 블로그)

        ```java
        import java.util.ArrayList;
        import java.util.List;

        public class Application {
            public static void main(String[] args) {
                Name crew1 = new Name("Fafi");
                Name crew2 = new Name("Kevin");
                
                List<Name> originalNames = new ArrayList<>();
                originalNames.add(crew1);
                originalNames.add(crew2);

                Names crewNames = new Names(originalNames); // crewNames의 names: Fafi, Kevin

                crew2.setName("Sally"); // crewNames의 names: Fafi, Sally
            }
        }
        ```
2.  검증을 수행하는 시점에 변경될 가능성이 존재

    if) 멀티스레드 환경이라면?

    1.  validate통과 후 방어적 복사를 수행하기 전에 numbers에 값이 추가/수정/삭제가 되는 경우, 검증은 성공했지만 객체의 값은 유효하지 않을 수 있음.

        이런 잠재적인 문제를 해결하기 위해서는 방어적 복사가 검증이 수행되기 전에 이루어져야함.

**TOCTOU**

*   Time of check, Time of use

    참조 타입 객체를 **방어적 복사하기 전**

    \*\*검증(check) 또는 사용(use)\*\*하는 시점에

    같은 주소를 가리키는 다른 곳에서 **변경이 발생할 수도** 있다는 개념

#### **Unmodifiable Collection이란?**

**Unmodifiable Collection**을 이용했을 경우 외부에서 변경 시 예외처리되기 때문에 안전하게 보장할 수 있음.

```java
// unmodifiableList는 원본 객체를 감싼 wrapper만 만드는 것
// 다른곳에서 사용할 경우 원본 객체에 대한 수정을 막을 수는 없음
Collections.unmodifiableList()
```

* unmodifiableList() 메서드를 통해 리턴되는 리스트는 읽기 용도로만 사용할 수 있으며, set(), add(), addAll() 등의 리스트에 변경을 가하는 메서드를 호출하면 UnsupportedOperationException 가 발생
* Unmodifiable과 Immutable은 다름
  * Unmodifiable은 불변을 보장해주지 않음
  * 원본 자체에 수정이 일어나면 unmodifiableList() 메서드를 통해 리턴되었던 리스트 또한 변경이 발생함.

> \[참고]
>
> * [\[10분 테코톡\] 이든의 방어적 복사](https://youtu.be/VsYw2GWgZV0?feature=shared)
> * [테코블 - 방어적 복사와 Unmodifiable Collection](https://tecoble.techcourse.co.kr/post/2021-04-26-defensive-copy-vs-unmodifiable/)
