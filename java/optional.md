# Optional

> **Brian Goetz ( Java’s language architect )** has mentioned:

> _Optional is intended to provide a limited mechanism for library method return types where they’re needed to be a clear way to represent “no result,” and using null for such, was overwhelmingly likely to cause errors._

코드 정리 github link : [https://github.com/young0264/java-playground/blob/main/src/test/java/org/main/basic/OptionalExam.java](https://github.com/young0264/java-playground/blob/main/src/test/java/org/main/basic/OptionalExam.java)

→ null 을 사용하는 것은 오류를 일으킬 가능성이 높으며, Optional은 메서드의 반환타입으로 제한적으로 쓰이게 ‘결과 없음’을 나타내는 명확한 방법이다.

*   Optional의 장점

    * Null-safe
    * 가독성이 좋아짐
    * optional을 반환하는 메서드는 예외를 던지는 메서드보다 유연하고 사용하기 쉬우며 null 반환하는 메서드보다 오류 가능성이 적음


*   새로운 문제들(Optional의 단점)

    *   필드에 optional을 사용하면 직렬화시 문제(지원X) or jackson library 사용(버전관리 불편)

        ```java
        public class Member implements Serializable {
            private Optional<String> name;
        }
        ```



    *   null 처리 (가독성 문제)

        ```java
        // optionalMember 자체가 null일경우
        public void signUp01(Optional<Member> optionalMember) {
            Member member = optionalMember.orElseThrow(Exception::new);   
        }

        // 일일이 null처리 하자니 가독성 X (저하)
        public void signUp02(Optional<Member> optionalMember) {
        	  if (optionalUser != null && optionalUser.isPresent()) {
        			  
        		}
        		throw new Exception();
        }
        ```
    * Optional 의 사용은 기본적으로 Wrapper 클래스기 때문에 시간/공간적으로 비용 증가


* 올바른 Optional 사용 가이드 (Effective java)
  * Optional 변수에 null 할당 X
  * 값이 없을 때 Optional.orElseOO()를 기본 반환타입으로 가져가기
  * 단순히 값을 얻는 목적일 때 사용 X
    * Brian Goetz : getter에 Optional을 얹어 반환하는 것은 남용
  * 생성자 수정자 메서드 파라미터등으로 Optional 사용 X
    * 계속 null체크를 해줘야하므로 번거로움
  * collection, stream, Array, Optional같은 컨테이너 타입은 옵셔널로 감싸면 X
    * 박싱된 타입을 담은 옵셔널을 반환하지 말자. : 두번 감싸기 때문에 너무 무거워짐
  * 성능에 민감한 로직이라면 optional 사용대신 null or Exception 을 던지는 형태로 구현 고려.
