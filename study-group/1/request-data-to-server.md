# Request Data To Server

* ## 서버의 개요
  * 서버의 OS는 멀티프로세스 & 멀티스레드 기능을 통해 다수의 프로그램을 동시에 작동 가능
  * 서버와 클라이언트의 구분
    *   다양한 형태로 데이터 송수신을 지원하기 위해 클라이언트와 서버로 **역할 구분하지 않고**

        **좌우 대칭 형식의 송.수신 구조**로 어디서나 자유롭게 데이터를 송신할 수 있도록 하는게 좋음.
    *   소켓 연결

        * 접속 동작시 접속 대기 소켓을 복사하여 소켓을 만듦
        * 접속 대기 Socket과 복사되는 Socket들은 서로 같은 port번호를 갖는데 client로 부터 각각의 통신으로부터 **Socket들을 구별**해야함 (port 번호를 달리주면 클라이언트에서 식별이 불가능)
        * (1) client IP address, (2)server IP address, (3)client port number, (4)server port number 이렇게 **4가지 정보로 소켓을 식별**함.
        * 여기에서 소켓을 만든 직후인 accept 상태일 때, 네가지 정보는 모르는 상태인데 이때 **소켓을 식별**하기 위해 필요한게 **디스크립터**


* ## 서버의 수신 동작
  *   **LAN 어댑터**에서 수신 신호를 **디지털 데이터로 변환**

      * **인터럽트**를 통해 CPU **커널에 접근**하여 패킷의 **도착을 알리**는 형태
        1. CPU 작업 → LAN 드라이버로 작업 전환
        2. 패킷추출
        3. MAC헤더의 데이터에 따라 프로토콜 판별
        4. 프로토콜에 맞는 소프트웨어 호출 ( ex. 적합한 프로토콜 스택에 패킷 전달)
      * 데이터 크기에 따라 패킷분할 & 분할 유무에 따른 패킷 복원
      * 접속대기 소켓을 보고 패킷 복사 & 새소켓 생성 & TCP헤더 생성 후 response(ACK패킷)
      * 위의 4가지 정보에 맞는 소켓을 찾고
      * (데이터는)패킷에서 데이터를 추출해 수신 버퍼에 저장후 데이터를 분할하기 전의 상태로 되돌림
      * 보통 패킷이 도착전에 read를 먼저 호출하고 데이터 받을 준비를 끝내서 기다리는 상태임


* ## TCP 연결 끊기 등장
  * **HTTP 1.0**에서는 서버에서 **연결끊기**를 시작함(HTTP 1.1부터는 소켓을 바로 닫지않고 기다림.)
  * FIN 패킷의 컨트롤비트 1설정 → ACK 반송 →ACK 보낸쪽에서 FIN 1 패킷 전송 → ACK반송
  * 이 경우도 연결 끊기 동작이 끝나면 잠시 기다렸다가 소켓을 말소함



* ## 리퀘스트 메시지 해석
  * URI를 실제 파일명으로 변환함
  * CGI 프로그램을 작동하면 리퀘스트 메시지가 HTML문서에 액세스 할때와는 다름
    * jsp
  *   클라이언트의 주소, 도메인명, 사용자와 패스워드

      * 이 세가지 조건에 따라 액세스 제어를 할 수 있음


* ## 응답메시지에 따라 화면에 표시함
  * 응답메시지의 Content Type 따라 데이터의 타입을 알 수 있음.
  * html문서 , 일반텍스트 등 데이터는 브라우저가 화면에 표시함
  * 소프트웨어로 실행하는 경우에는 해당 app을 호출하여 호출된 프로그램을 표시함