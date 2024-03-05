---
coverY: 0
---

# Servlet

*   서블릿(Servlet) ?

    * 클라이언트 요청을 처리하고 그 결과를 반환하는 웹 프로그래밍 기술
    * 자바프로그램이 하는 클라이언트의 요청에 대한 결과 반환

    <figure><img src="../.gitbook/assets/스크린샷 2024-03-05 09.51.39.png" alt="" width="375"><figcaption></figcaption></figure>

    * 이런 값들의 처리를 뒷단에서 Servlet이 해줌 → 개발자는 비즈니스 로직에 집중

```java
@WebServlet(name="thisIsServlet", urlPatterns = "/hello")
public class HelloServlet extends HttpServlet {

    @Override
    protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        super.service(request, response);
    }
}
```

* Servlet 동작 방식
  * 사용자가 url을 입력해서 요청하면 http request가 servlet container로 전송함.
  * 요청 받은 servlet container는 HttpRequest, HttpResponse 객체를 생성
  * URL이 어느 서블릿에 대한 요청인지 찾음.(name) 여기서 `thisIsServlet` 를 찾음
  * service 메서드 호출 후 http method에 따라 doGet, doPut, doPost, doDelete 등의 메서드를 찾아 호출함
  * 동적 페이지 생성 후 HttpServletResponse 객체에 응답을 보냄
  * 응답 후 HttpServletRequest, HttpServletResponse 객체를 소멸함.(gc랜덤)
* Servlet live cycle(생명주기)



<figure><img src="../.gitbook/assets/스크린샷 2024-03-05 10.57.47.png" alt="" width="375"><figcaption></figcaption></figure>

* 클라이언트로부터 요청이 들어오면 서블릿이 메모리에 있는지 확인 → 없으면 init() 메서드 호출하여 적재
*   service() 메서드를 통해 요청에 대한 응답이 doGet(), doPost()로 분기

    이때 HttpServletRequest, HttpServletResponse에 의해 request, response 객체가 제공됨
* 컨테이너가 서블릿에 종료 요청을 하면 destroy() 메서드가 호출됨. 종료시 어떤 작업을 추가하고 싶으면 destroy() 메서드 오버라이딩하여 구현.

→ 즉, 서블릿은 클라이언트와 DB 사이의 서버(여기선 Web Application Server인 Servlet)에서 중간다리 역할을 해주는 역할을 한다. 이로써 프로토콜마다 다른 형태의 데이터의 변환 작업등을 서블릿 컨테이너를 통해서 해줌으로써 개발자는 개발에 집중할 수 있게된다.

> 참고 : [https://tecoble.techcourse.co.kr/post/2021-05-23-servlet-servletcontainer/](https://tecoble.techcourse.co.kr/post/2021-05-23-servlet-servletcontainer/)
