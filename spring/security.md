---
description: 스프링 시큐리티
---

# Security

<figure><img src="../.gitbook/assets/Untitled (1).png" alt=""><figcaption><p>spring에서 http request 흐름도</p></figcaption></figure>



*   #### spring security 란?

    사용자 정의가 가능한 인증 및 액세스 제어 프레임워크입니다. 이는 Spring 기반 애플리케이션 보안을 위한 표준
* **Filter Chain**
  * Spring Security는 Filter 방식으로 여러 체이닝 된 Filter를 거치며 앞선 Filter에서 인증이 완료되면 뒤 따라오는 Filter에 걸리지 않고 인증된 사용자가 되는 것. 이 Filter를 통해 인증이 되지 않았으면 그 User는 인증되지 않은 사용자인 것.
* **기본용어**
  * Principal(접근주체) : 접근하는 대상(User)
  * Authentication(인증) : 리소스에 접근한 User가 누구인지 식별
  * Authorize(인가) : 접근한 User가 리소스에 접근 권한이 있는지 검사
*   순서

    * Filter
    * Dispatcher Servlet
      * handler
    * Interceptor
    * Controller





참고 : [https://spring.io/projects/spring-security/](https://spring.io/projects/spring-security/)
