---
description: ']https://young1403.tistory.com/83https://young1403.tistory.com/83'
---

# layer, protocol, http

> **네트워크 계층, 4계층**

#### 📌 컴퓨터 네트워크와 네트워크 레이어

* **컴퓨터 네트워크에 대해서 설명해주세요.**
  * 컴퓨터 네트워크란 다른 컴퓨터간에 데이터를 공유하고 통신하는 시스템입니다.
* **현대 컴퓨터 네트워크에서 데이터를 어떻게 전달할까요?**
  * 물데네\~\~
  * 현대 컴퓨터 네트워크에서는 여러 프로토콜을 이용하여 데이터를 전달하는데 현대에서는 TCP/IP 4계층을 활용하여 데이터를 전달합니다.
* **라우터와 스위치에 대해 설명해주세요.**
  * 나 : 스위치는 라우터를 향하여 통신을 하고 라우터는 로컬에서 인터넷으로 전달하는 역할을 합니다.
  *   추가 :

      스위치는 MAC주소를 기반으로 데이터링크 계층에서 동작하며 라우터는 IP를 기반으로 동작하는 네트워크 계층에 속해있습니다.
  *   디바이스는 스위치를 통해 현장에서 연결되고, 네트워크는 라우터를 통해 다른 네트워크와 연결된다.

      원문보기:

      [https://www.itworld.co.kr/howto/259714#csidx9bc76e366885e9096004a4a8656cdea](https://www.itworld.co.kr/howto/259714#csidx9bc76e366885e9096004a4a8656cdea)

      *   참고

          [네트워크 스위치와 허브, 라우터는 어떻게 다른 걸까](https://www.itworld.co.kr/tags/1354/%EC%8A%A4%EC%9C%84%EC%B9%98/167585)
* **프로토콜이 뭔가요? 프로토콜 스택에 대해서 설명해주세요.**
  * 프로토콜이란 네트워크에서 통신을 하기 위한 규약(규칙입니다)
  *   **프로토콜 스택**이란 프로토콜들이 계층화된 구조(계층화 시킨것)로 모여있는 집합을 말합니다. 대표적으로 OSI7계층과 TCP/IP 4계층이 있습니다. 이렇게 계층을 나누는 이유는 각 **계층**을 나눔으로써 역할을 분담하기 위해서입니다.

      데이터를 어떻게 패킷화해서 보내며 수신측에서 어떻게 해석하고 처리할지에 대한 지침(규약)을 제공함. 이렇게 계층화하면 보다 효율적인 통신이 가능하게 됩니다.
* **🚨 OSI 7 Layer에 대해서 설명해주세요. 물데네전세표층**
  * **(꼬리질문) 각 계층별로 설명해줄래요?**
  * OSI 7계층은 물리계층 - 데이터링크계층 - 네트워크 계층 - 전송 계층 - 세션계층 - 표현계층 - 응용계층으로 나뉩니다.
  * 1계층 - 물리계층 : 케이블 (+리피터 허브 )등을 통해서 물리차원에서의 데이터를 전송합니다. 전기 신호로 전달하는데 1과 0 신호로 전달합니다 // TODO : 다른방법도 있었음
  * 2계층 - 데이터링크 계층 : MAC주소를 가지고 통신을 하게되고 단위는 프레임, 스위치등의 장비를 사용합니다.
  * 3계층 - 네트워크 계층 : 라우팅, 흐름제어, 오류제어, 세그멘테이션 등을 수행함. IP주소를 바탕으로 라우터를 사용하여 패킷을 전달해 줍니다.
  * 4계층 - 전송 계층 : TCP/UDP 프로토콜을 사용하며, 데이터 전송을 담당하는 계층입니다. TCP는 신뢰성, 연결지향적, 헤더가 큰 특징이있고 UDP는 비연결성, 비신뢰성, 헤더가 단순하다는 특징이 있음
  * 5계층 - 세션 계층 : 데이터가 통신하기 위한 논리적인 연결. (통신을 위한 대문) 세션설정, 유지, 종료, 전송 중단시 복구 등의 기능이 있음. TCP/IP 세션을 만들고 없애는 책임을 짐.
    * 통신하는 사용자들을 동기화하고 오류복구 명령들을 일괄적으로 다룬다. 통신을 하기 위한 세션을 확립/유지/중단 (운영체제가 해줌)
  * 6계층 - 표현 계층(presentation) : 데이터 타입을 판별하고(txt인지 gif인지 jpg인지) MIME 인코딩이나 암호화등의 동작이 이 계층에서 이루어짐
  * 7계층 - 응용 계층(application): HTTP, FTP, SMTP, Telnet등과 같은 프로토콜이 있음. 응용 프로그램과 관계하여 응용서비스를 수행함. 컴퓨터 네트워크의 가장 상위 계층으로, 사용자와 상호 작용함. 웹브라우징, 이메일통신, 파일전송등을 지원
* **🚨 TCP/IP에 대해서 설명해주세요.**
  * TCP/IP는 프로토콜스택 중 하나로써 데이터 통신을 통신 규칙의 모음입니다. (TCP와 IP는 그 안에서 하나의 프로토콜일뿐)
  * TCP/IP 4계층은
    * 응용계층 ,
    * 전송계층(TCP/UDP)
    * 인터넷계층(IP, ARP)
    * 네트워크 액세스 계층(MAC주소) 으로 나뉩니다.
* **OSI 7 Layer 또는 TCP/IP 4계층 처럼 프로토콜들을 계층화하면 어떤 장점이 있을까요?**
  * **(꼬리질문 예시) 웹 개발시 레이어드 아키텍처를 어떻게 적용했나요?**
  * 역할 분리를 통해 시스템을 모듈화와 단순화를 할 수 있습니다. 그러하기에 각 계층별로 테스트나 개발 및 유지보수를 할 수 있습니다.
  * 또한 디버깅등을 통해 네트워크 문제를 식별하고 해결하기가 더 쉬워집니다.
*   **사용해봤던 프로토콜에 대해서 설명해줄 수 있어요?**

    javaMailSender를 이용한 SMTP프로토콜과 TCP/IP통신을 하며 TCP와 IP프로토콜 그리고 HTTP 프로토콜을 사용해보았습니다. SMTP와 HTTP는 OSI 7계층의 응용계층의 프로토콜이며 TCP는 전송계층, IP는 네트워크계층의 프로토콜 입니다.
* 추가
  * ARP란?
  * Address Resolution Protocol의 약자로 IP 주소를 MAC주소와 매칭 시키기 위한 프로토콜 입니다. 로컬 네트워크 에서 단말과 단말 간 통신 시 IP주소와 MAC주소를 이용하는데, 그 IP주소와 MAC Address를 매칭하여 IP 단말이 소유한 MAC 주소를 향해 제대로 찾아가기 위해 ARP 를 사용합니다.

#### 📌 HTTP 프로토콜

* **🚨 HTTP 프로토콜이 무엇인가요?**
  * Hyper Text Transfer Protocol의 약자로 응용계층에서 사용되는 통신규약 프로토콜 중 하나입니다.
  * app 단계, 4단계와 연결짓기
* HTTP 속성
* **🚨 HTTP의 요청/응답 모델에 대해 설명해주세요.**
  *   요청 : 헤더/바디(가끔)

      데이터를 보낼때 사용하고, http메서드를 정해서 요청을 보냅니다.
  *   응답 : 헤더/바디

      데이터를 받을때 사용하고 응답 상태코드를 통해 데이터가 어떻게 처리되었는지 알 수 있고 응답헤더, 응답바디 등이 있습니다. 상태코드는 응답헤더에 포함되어 옵니다.
* **🚨 HTTP 메서드가 뭔가요? 알고 있는 메서드 몇가지 알려주세요.**
  * 이러한 HTTP 메서드는 RESTful API와 웹 애플리케이션 개발에서 중요하게 사용되며, 각 메서드는 특정한 동작을 수행하기 위해 설계되었습니다.
  * 자원의 동작(verb)을 http 메서드를 통해 나타낼 수 있습니다.
  * GET, POST, PATCH, PUT, DELETE등이 있습니다.
* 멱등하다 : 한번 혹은 여러 번 실행됐는지에 상관없이 같은 결과를 반환한다면 그 트랜잭션은 멱등(idempotent)하다고 말함. GET, HEAD, PUT, DELETE, TRACE, OPTIONS메서드들은 멱등하다고 이해하면 됨
* **HTTP 메서드중 GET과 POST의 차이점은 뭔가요?**
  * GET은 조회, POST는 생성의 의미를 띄고있는 http 메서드입니다. 또한 GET 메서드는 query string을 통해서 데이터를 보내며, POST메서드는 http body에 데이터를 보냅니다. 데이터 양도 GET메서드 같은 경우 url길이제한 2048자가 있지만 POST는 큰 데이터도 전송이 가능합니다. 보안적인 면에서도 POST가 query가 노출되는 GET에 비해 안전합니다. GET은 결과가 캐싱이 될 수 있지만 POST는 캐싱이 불가능한 메서드입니다.
* **HTTP 메서드중 PUT과 PATCH의 차이점은 뭘까요?**
  * PUT은 자원의 전체를 바꾸어 수정하는 의미의 메서드이고, PATCH는 자원의 일부를 수정하는 메서드 입니다. 비즈니스 로직을 꾸밀때 만약 기존자원의 데이터를 참고하지 않고 아예 다른 자원으로 갈아 끼우는 형태라면 PUT, 기존자원의 데이터를 참고해 +1이나 -1같은 상황일때는 PATCH메서드를 사용하면 http메서드만 보고도 을 조금 더 명확하게 표현할 수가 있습니다.
*   **🚨 HTTP 상태 코드가 뭔가요? 알고 있는 상태 코드 몇가지 알려주세요.**

    * 200번 , 301번, 404, 500번입니다
    * 200번은 정상적으로 요청이되어 OK가 되었다는 의미이고 301번은 URL이 영구적으로 redirect한다는 상태코드이고 (요청을 완료하려면 추가작업 필요) 404는 클라이언트 오류로 인해 서버가 요청을 수행할 수 없다라는 상태코드이고 500은 서버측 에러라는 상태코드 입니다.

    ***

    *   400과 500의 차이 : 500\~db가 켜지지 않은

        → 자세히 찾아보기
* **HTTP 헤더가 뭘까요? 알고 있는 헤더 몇 가지 설명해주세요.**
  * HTTP 요청과 응답에서 메타데이터를 전달하는 데 사용되는 부가적인 정보를 포함하는 부분입니다. HTTP 헤더는 클라이언트와 서버 간의 통신을 제어하고 리소스를 설명하는 역할을 합니다.
  * HTTP 헤더에는 Content-Type과 HTTP메서드, 경로
* **🚨 HTTP의 무상태성(Stateless)에 대해서 설명해주세요.**
  * HTTP는 비상태성(STATELESS)적인 성격을 가지고 있는 프로토콜입니다. 그렇기 때문에 로그인,장바구니 등 상태가 유지되어야 할 정보들이 페이지가 바뀌면 그 정보가 유지가 되지 않는 특징입니다. 이것을 해결하기위해 쿠키나 세션을 사용하곤 합니다.
  * gpt : HTTP 요청이 서로 독립적이며 이전 요청과 상태를 공유하지 않는다는 원칙을 의미합니다.
  * 쿠키 세션의 차이
* **HTTP Keep-Alive에 대해서 설명해주세요.**
* **HTTP 파이프라이닝에 대해서 설명해주세요.**
  * **요청에 대한 응답을 받기 기다리지 않고 여러개의 요청을 보내는 것입니다. 응답을 기다리지 않기 때문에 서버와 클라이언트간의 대기 시간을 줄일 수 있습니다.**
  *

      <figure><img src="../../../.gitbook/assets/스크린샷 2023-12-12 14.27.07.png" alt="" width="375"><figcaption></figcaption></figure>
  *   먼저 보낸 요청이 완료되기 전에 다음 요청을 보내는 기술이다. 수신측 응답을 기다리지 않고 우선 송신측에서 요청을 보내 다음 응답까지의 대기시간을 없애, 네트워크 성능을 향상 시킨다. Keep-Alive를 전제로 하며, 서버는 요청이 들어온 순서대로 응답을 반환합니다.

      ***

      HTTP/1.0에서는 HTTP Request의 소켓에 write를 한 뒤, 송신측 으로부터 Response를 받은 후 다음 Request를 보내는 형태로 웹이 동작하였다. 이렇게 되면 여러 요청에 대해 이전의 응답 시간을 기다려야하는 Latency가 길어지기 때문에 큰 비용 요구했다. 그리고 HTTP/1.0에서는 stateless로 HTTP 요청들은 연결의 맺고 끊음을 반복하기 때문에 서버 리소스적으로도 적지 않은 비용을 요구했다. HTTP/1.1에 들어서는 클라이언트에서는 각 요청에 대한 응답을 기다리는게 아니라, 여러개의 Request를 하나의 TCP/IP Packet으로 연속적으로 해서 Packing을 하여 요청을 보낸다. 파이프라이닝이 적용되면, 하나의 Connection으로 다수의 Request와 Response를 처리할 수 있어 Network Latency를 줄일 수 있습니다.
  *   다만..완전한 멀티 플렉싱이 아닌 응답을 기다리는(응답을 미루는) 방식으로 응답의 **처리는 순차적으로 처리되며,** 결국 후순위의 응답은 지연될 수 밖에 없습니다. 이것이 HTTP **Head Of Line Blocking**이라고 불리며 한 요청이 오래걸리면 그 다음 요청이 지연된다는 것을 말합니다.

      따라서 HTTP/1.1의 웹서버는 서버측에 Multiplexing을 통한 요청의 처리를 요구하며, 각 Connection당 요청과 응답의 순서가 보장되는 통신을 합니다. HTTP 파이프라이닝은 HTTP/2.0이 등장하면서 멀티플렉싱 알고리즘으로 대체가 되었고, 모던 브라우저들에서도 기본적으로는 활성화하지 않고 있습니다.
* **HTTP/1.1, HTTP/2, HTTP/3 각각의 특징에 대해 설명해 주세요.**
  * HTTP/1.1 : 에서의 **파이프라이닝은** 하나의 커넥션에서 처리해야해서 Head Of Line Blocking (HOLB) 문제가 발생.
  *   HTTP/2.0 : HTTT2에서는 요청과 응답의 **멀티 플렉싱을 지원**합니다. **멀티플렉싱**은 한개의 TCP연결에서 여러 개의 데이터를 섞이지 않게 보내는 기법으로 이 각각의 데이터 흐름을 stream이라고 합니다. 요청과 응답이 동시에 이루어지니 섞이게 되고 곧 순서 없이 여러개를 병렬처리 할 수 있습니다. 그래서 부분적으로는 **HOLB문제도 해결**할 수 있습니다.

      하지만 이또한 하나의 커넥션에서 처리. TCP 프로토콜을 사용하는 이상 request에 대한 response가 받아져야 통신이 됩니다. 하지만 하나의 TCP커넥션으로 처리해서 TCP패킷이 네트워크 경로에서 손실되면 스트림에 공백이 생겨 그 다음에 오는 바이트들도 재전송으로 인해서 전달이 되지 않아 전송이 지연됩니다. → 이렇게 HTTP2는 여러개의 HTTP 스트림을 하나의 TCP 커넥션으로 처리하기 때문에 크게 영향을 받습니다.
  * HTTP/3.0 . QUIC : [이 블로그에 너무 잘 나와있다. 밑은 블로그 글의 요약](https://evan-moon.github.io/2019/10/08/what-is-http3/)
    * HTTP3의 가장 큰 특징은 TCP가 아닌 UDP를 사용한다는 것입니다. HTTP3는 **QUIC라는 프로토콜** 위에서 돌아가는 HTTP입니다. QUIC란 Quick UDP Internet Connection의 약자로 UDP를 사용하는 프로토콜입니다. TCP의 3way-handshake, 4way-handshake등 오버헤드와 HOLB등의 문제를 피할 수 없지만 QUIC는 TCP handshake 과정을 최적화하는 것에 초점을 맞추어 설계되어있습니다. UDP는 데이터그램 방식을 사용하는 프로토콜이기에 각각의 패킷 간 순서가 존재하지 않고 독립적입니다. 그리고 UDP의 헤더는 TCP보다 가볍고, 개발자가 구현을 어떻게 하느냐에 따라서 **신뢰성을 확보**할 수 있습니다.
    * HTTP2와 마찬가지로 멀티플렉싱의 이점을 그대로 가져갑니다.
    * 클라이언트의 IP가 바뀌어도 그대로 유지됩니다
      * TCP는 발신지IP, 수신지IP, 발신지 port, 수신지port를 식별해 클라이언트 IP가 바뀌면 연결을 끊습니다. 이를 다시 연결하려면 3-wayhandshake를 거쳐야 하기에 레이턴시가 발생합니다. 요즘처럼 모바일 통신이 많은 상황에서 wifi에서 셀룰러로 전환하는 등 문제가 심각할 수 있습니다. 하지만 **QUIC**는 **connection ID를 사용**해서 서버와 연결을 생성합니다. (QUIC의 Connection ID는 QUIC 연결을 식별하고 관리하는 데 중요한 역할을 하는 고유한 식별자입니다.) 이는 클라이언트 IP와는 전혀 무관하기 때문에 클라이언트의 IP가 변경되어도 기존의 연결을 계속 유지할 수 있습니다.
* 참고
  * 총정리 : [https://evan-moon.github.io/2019/10/08/what-is-http3/](https://evan-moon.github.io/2019/10/08/what-is-http3/)
  * [https://ijbgo.tistory.com/26](https://ijbgo.tistory.com/26)
  *   pipelining

      [HTTP 1.1 vs HTTP 2.0](https://velog.io/@tnehd1998/HTTP-1.1-vs-HTTP-2.0)
  *   QUIC

      [HTTP/3는 왜 UDP를 선택한 것일까?](https://evan-moon.github.io/2019/10/08/what-is-http3/)
  *   pipelining

      [Keep-Alive와 파이프라이닝](https://seonghui.github.io/TIL/docs/http/keep-alive-and-pipelining.html)
  *   pipelining

      [HTTP/1.1 의 HTTP Pipelining 과 Persistent Connection 에 대하여](https://jins-dev.tistory.com/entry/HTTP11-%EC%9D%98-HTTP-Pipelining-%EA%B3%BC-Persistent-Connection-%EC%97%90-%EB%8C%80%ED%95%98%EC%97%AC)
  *   \[HTTP] 파이프라이닝 & HTTP 버전별 특징 (1.0, 1.1, 2.0, 3.0)

      [https://young1403.tistory.com/83](https://young1403.tistory.com/83)

