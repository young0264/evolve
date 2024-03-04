# String vs StringBuilder vs StringBuffer

#### String

* 문자열을 더할 때마다 새로운 주소값을 가진 객체를 만든다. 그리고 사용된 객체는 gc의 관리 대상이 된다. → GC를 하면 할수록 CPU를 사용하게 되고 시간도 많이 소요됨. 메모리 소요를 최소화 해야함.
* 짧은 문자열을 더할 때 사용

#### StringBuilder

*   StringBuilder 소스코드 일부

    <figure><img src="../../.gitbook/assets/스크린샷 2024-03-05 01.01.45.png" alt=""><figcaption></figcaption></figure>
* 동기화를 하지 않음(그러기에 리소스가 적게 들어 StringBuffer에 비해 빠름)
* 스레드 safe 여부에 관계 없는 개발 ex) 메서드 내 변수

#### StringBuffer

*   StringBuffer 소스코드 일부

    <figure><img src="../../.gitbook/assets/스크린샷 2024-03-05 01.01.32.png" alt=""><figcaption></figcaption></figure>
* 기본적으로 synchronized 가 붙어있음. → 동기화
* 스레드 안전한 프로그램 사용시
* static 문자열 변경, singleton 클래스에 선언된 문자열일 경우
*   **성능비교 소스 코드**

    <figure><img src="../../.gitbook/assets/Untitled (18).png" alt=""><figcaption></figcaption></figure>

    ```java
    package org.main.java_performance_tuning_story;

    import java.util.logging.Logger;

    public class String_Builder_Buffer {
        public static void main(String[] args) {

            final String aValue = "abcde";
            Logger logger = Logger.getLogger("Logger");

            for (int i = 0; i < 10; i++) {
                String a = new String();
                StringBuffer b = new StringBuffer();
                StringBuilder c = new StringBuilder();

                long startTime = System.currentTimeMillis();
                for(int j=0; j<100000; j++) {
                    a += aValue;
                }
                long endTime = System.currentTimeMillis();
                logger.info("String spent time1 : " + (endTime - startTime));

                long startTime2 = System.currentTimeMillis();
                for (int j = 0; j < 100000; j++) {
                    b.append(aValue);
                }
                long endTime2 = System.currentTimeMillis();
                logger.info("String spent time2 : " + (endTime2 - startTime2));

                long startTime3 = System.currentTimeMillis();
                for (int j = 0; j < 100000; j++) {
                    c.append(aValue);
                }
                long endTime3 = System.currentTimeMillis();
                logger.info("String spent time3 : " + (endTime3 - startTime3));
                logger.info("=================================================");
            }

        }
    }

    ```
