# IO/NIO

| 구분          | IO              | NIO          |
| ----------- | --------------- | ------------ |
| 입출력 방식      | 스트림 방식          | 채널 방식        |
| 버퍼 방식       | 넌버퍼(non-buffer) | 버퍼(buffer)   |
| 비동기 방식      | 지원 안함           | 지원           |
| 블로킹/넌블로킹 방식 | 블로킹만 지원         | 블로킹, 넌블러킹 지원 |

* 스트림
  * 입력/출력 스트림으로 구분
  * 입/출력 둘 다가 필요하면 객체를 따로 생성하여 로직을 구성
* 채널
  * 양방향 입/출력이 가능
* 버퍼
  * NIO에서 데이터를 입출력하기 위해 사용
  * 읽고 쓰기가 가능한 메모리 배열
  * 스트림에 비해 복수개의 바이트를 한꺼번에 입출력함. → 빠른 성능
* non-blocking(넌블러킹)
  * 입출력시 스레드가 블로킹되지 않음
  * 입출력 작업을 요청한 후, 작업이 즉시 완료되지 않더라도 프로그램은 멈추지 않고 다른 작업을 수행함
  * 다중 클라이언트와 통신하는 서버에서 효율적
  * 우선 null이나 0과 같은 값을 리턴받아, 지속적으로 호출된 함수를 계속 보내며 호출된 함수가 완료되면 데이터를 리턴받는데 이것을 **polling** 이라고 함
  *   **Selector** 클래스에서 Polling이 핵심적인 역할을 함.(NIO의 channel을 감시하는 역할)

      * polling을 사용하여, read, write, accept, connect 등 이벤트 등

      <figure><img src="../../.gitbook/assets/스크린샷 2024-08-23 15.23.51 (1).png" alt="" width="333"><figcaption></figcaption></figure>

      ![](https://prod-files-secure.s3.us-west-2.amazonaws.com/3867e0bf-d699-47e9-9762-be3836599de7/8033a515-2db8-4ce3-87ed-53c5d0d36faa/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA\_2024-08-23\_15.23.51.png)
*   blocking(블로킹)

    * 입출력 작업이 완료될 때까지(데이터를 받기 전까지) 해당 작업에서 기다림
    * 데이터를 받을 때, 그 작업이 완료되기 전까지 다른 작업을 할 수 없음

    <figure><img src="../../.gitbook/assets/스크린샷 2024-08-23 15.23.07.png" alt=""><figcaption></figcaption></figure>

    ![](https://prod-files-secure.s3.us-west-2.amazonaws.com/3867e0bf-d699-47e9-9762-be3836599de7/fa2637cc-96e9-4f8c-8db8-5b1385903ef8/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA\_2024-08-23\_15.23.07.png)
*   버퍼

    * direct vs non-direct buffer 두가지로 나뉨

    | 구분          | 넌다이렉트 버퍼  | 다이렉트 버퍼           |
    | ----------- | --------- | ----------------- |
    | 사용하는 메모리 공간 | JVM 힙 메모리 | 운영체제의 메모리         |
    | 버퍼 생성 시간    | 빠르다       | 느리다               |
    | 버퍼의 크기      | 작다        | 크다(큰 데이터 처리시 유리)  |
    | 입출력 성능      | 낮다        | 높다(입출력이 빈번할 때 유리) |

    * 다이렉트 버퍼 : 한번 만들어놓고 재사용 하는것이 적합
    * 넌 다이렉트 버퍼 : 임시 다이렉트 버퍼를 생성하고 넌 다이렉트 버퍼에 있는 내용을 임시 다이렉트 버퍼에 복사, 임시 다이렉트 버퍼를 사용해 native I/O 수행. → 입출력 성능이 떨어짐

***

#### NIO

**byte 해석순서(Byteorder)**

* 앞쪽부터 처리하는 것 : Big endian. (JVM은 무조건 Big endian)
* 뒤쪽부터 처리하는 것 : Little endian. (Windows. Mac OS)

**Buffer의 위치 속성**

* position : 현재 읽거나 쓰는 위치값. limit 보다 큰 값을 가질 수 없음.
* limit : 버퍼에서 읽거나 쓸 수 있는 위치의 한계(capacity보다 같거나 작은 값을 가짐.)
* capacity : 버퍼의 최대 데이터 개수. 인덱스 값이 아닌 수량임
* mark :
  * reset()메서드를 실행했을 때 돌아오는 위치를 지정하는 인덱스. mark()메서드로 지정 가능
  * position, limit 보다 작아야함. ifnot → mark는 제거됨. 여기서 reset메서드 호출 → Exception

**Buffer 변환**

* 채널이 데이터를 읽고 쓰는 버퍼는 모두 ByteBuffer

**FileChannel(파일채널)**

* 파일 읽기, 쓰기 가능
* 동기화 처리가 되어있기 때문에 멀티 스레드 환경에서 사용해도 안전

**파일 비동기 채널**

* FileChannel의 read(), write()는 파일 입출력 작업 동안 블로킹 → UI 갱신이나 이벤트 처리를 할 수 없음
* JAVA NIO는 불특정 다수의 파일 및 대용량 파일의 입출력 작업을 위해서 비동기 파일 채널(AsynchronousFileChannel)을 별도로 제공하고 있음
*   AsynchronousFileChannel의 특징은 read(), write() 메서드를 호출하면 스레드풀에게 작업을 요청하고 이 메서드들을 즉시 리턴시킴

    → 스레드풀의 작업 스레드가 작업 처리를 함

**TCP 블로킹 채널**

NIO를 이용해 TCP 서버/클라이언트 개발시 (1)블로킹, (2)넌블러킹, (3)비동기 구현방식 중 하나를 결정해야함.

* **Socket Channel vs Server Socket Channel**
  1. **Socket Channel**
     * 버퍼 사용o, 블로킹, 논블로킹 모두 지원
  2. **Server Socket Channel**
     * 버퍼 사용o, 블로킹, 논블로킹 모두 지원
     * 클라이언트 SocketChannel의 연결 요청을 수락 후 통신용 SocketChannel을 생성
* **SocketChannel 객체**
  * 버퍼로 읽고 쓰는 작업을 함
  * read(), write()의 경우에 blocking이 됨 → 클라이언트 연결 하나에 작업 스레드 하나를 할당해서 병렬처리 해야함

**TCP 넌블로킹 채널**

넌블로킹 방식의 핵심 객체인 셀렉터(Selector)dp eogks dlgo.

* **넌블로킹 방식의 특징**
  * 블로킹 방식에선 accept(클라 요청 대기), read(데이터수신 대기), write(쓰기 작업 대기)에서 블로킹됨.
  *   넌블로킹 방식은 connect(), accept(), read(), write() 메서드에서 블로킹이 없음.

      연결 요청이 없으면 accept()는 null, 데이터를 보내지 않으면 read()는 0을 리턴해 매개값으로 전달한 ByteBuffer에는 어떤 데이터도 저장되지 않음.
  *   넌블러킹 방식에서는 연결 요청을 하지 않으면 무한 루프를 계속해서 돔.

      accept() 메서드가 블로킹되지 않고 바로 리턴되어 클라이언트가 요청을 보내기 전까지 while 블록 내의 코드가 쉴새없이 실행되어 CPU가 과도하게 소비되는 문제점이 발생함

      그래서 넌블로킹은 이벤트 리스너 역할을 하는 셀렉터(Selector)를 사용함

      ```java
      while(true){
      	WocketChannel socketChannel = serverSocketChannel.accept()
      }
      ```

      넌블러킹 채널에 Selector를 등록해 놓으면 클라이언트의 연결 요청이 들어오거나 데이터가 도착할 경우, 채널은 Selector에 통보함. Selector는 통보한 채널들을 선택해 작업 스레드가 accept()또는 read()메서드를 실행해서 작업을 처리하도록 함
  * Selector
    1. 채널은 Selector에 자신(ServerChannel)을 등록시 작업 유형을 SelectionKey로 생성하고 Selector의 관심 키셋(interest-set)에 저장
    2. 클라이언트가 요청을 처리
    3. Selector는 관심키셋에 등록된 키 중에 선택된 키셋(selected-set)에 별도로 저장
    4. 작업스레드가 selected-set에서 키를 하나씩 꺼내와 키와 연관된 채널 작업을 처리
    5. selected-set에 있는 키를 처리하면 비워진 후, Selector는 다시 관심키셋에서 작업 처리 준비가 된 키들을 선택해서 선택된 키셋을 채움
* **Socket Channel Server Socket Channel**
  * 예제코드
* **TCP 비동기 채널**
  *   NIO는 TCP블로킹, 넌블러킹 채널 이외에 TCP 비동기 채널로 AsynchronousSocketChannel과 AsynchronousServerSocketChannel을 제공함

      각각 SocketChannel과 ServerSocketChannel에 대응하는 클래스임.
  * connect(), accept(), read(), write() 를 호출하면 스레드풀에게 작업 처리를 요청하고 이 메서드들은 즉시 리턴됨. 작업 완료 후 callback 메서드가 자동 호출되기때문에 작업 완료 후 실행해야 할 코드가 있다면 콜백 메서드에서 작성하면 됨
  * 실질적인 작업처리는 스레드풀의 작업 스레드가 담당함
  * (1) accept(), (2) 즉시리턴, (3) 작업처리요청, 스레드풀 진입, (4) 작업 큐, (5) 각 스레드에서 작업 처리, (6) 콜백 메서드 호출
* **UDP 채널**
  * NIO에서 UDP 채널은 DatagramChannel이다. DatagramChannel도 TCP 채널과 마찬가지로 블로킹과 넌블로킹 방식으로 사용가능

***

> * blocking, non-blocking IO model, async, signal-driven, IO multiplexing … 등등 [https://liakh-aliaksandr.medium.com/java-sockets-i-o-blocking-non-blocking-and-asynchronous-fb7f066e4ede](https://liakh-aliaksandr.medium.com/java-sockets-i-o-blocking-non-blocking-and-asynchronous-fb7f066e4ede)

