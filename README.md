<h1>확인</h1>

  <div>양방향 통신에는 주로 웹 소켓을 사용한다. 웹 소켓을 알기에 앞서 HTTP 프로토콜을 아는 것이 좋은데, 웹 소켓 프로토콜은 기본적으로 HTTP 프로토콜의 발전된 버전이기 때문이다. </br>
  웹 개발을 할 때에 흔하게 사용되는 HTTP 프로토콜은 기본적으로 요청-응답(request-response)의 단방향 통신을 진행한다. 다만 HTTP 프로토콜은 클라이언트와 서버의 연결을 지속해야 하는, '실시간으로 여러 사용자와 양방향 상호작용'하는 애플리케이션을 제작하는 데에 제약이 존재한다. <br />

  1. HTTP 연결의 주체는 클라이언트로, 서버는 클라이언트의 요청을 기다려야만 한다. HTTP 풀링으로 서버를 주기적 호출하는 방법이 있으나 이는 비효율적이다. <br />
  2. 서버와 클라이언트 간 연결이 유지되지 않아 기존 문맥과 상태를 유지하며 최소한의 정보만 주고받기 어렵다. HTTP 통신의 헤더가 차지하는 공간이 크기 때문에, 채팅 앱과 같은 단문 메세지를 나눌 경우 비효율적이다.<br />
  <br /></div>

<h2>웹 소켓 통신 과정</h2>
  <div>웹 소켓 연결 역시 처음에는 client 딴에서 서버로의 GET 요청이 요구된다. 이 때 <b>'Connection'</b>이나 <b>'Upgrade'</b>라는, 조금 특이한 헤더를 함께 보내는 것이 차이점이다.<br />
  </div>

  ```
  GET /chat HTTP/1.1
Host: www.test.com
Connection: Upgrade
Upgrade: websocket
Sec-WebSocket-Key: dGhlIHNhbXBsZSBub25jZQ==
Sec-WebSocket-Version: 13
  ```

  <h4>통신 방법</h4>
  1. Websocket 클래스를 사용해 웹소켓 객체를 하나 만든다. ws나 wss 프로토콜을 사용하는 서버의 URL을 WebSocket 클래스의 생성자에 넘기면 된다.<br />
  웹소켓 객체가 만들어졌음은 handshake 과정이 끝나고 웹소켓을 통해 서버와 메세지를 주고받을 수 있는 상태가 되었다는 말이다.<br />
  (이를 통해 open, message, error, close를 처리할 수 있다.)<br />
    이벤트 처리 핸들러는 assEventListener 메서드를 통해 설정할 수 있다.
    
    ```
    const socket = new WebSocket("사이트 주소");
    socket.addEventListener("open", (event) => {console.log("서버와의 연결 진행")});
    socket.addEventListener("message", (event) => {console.log("서버로부터 받은 메세지: ", event.data);});
    socket.addEventListener("close", (event) => {console.log("서버와의 연결 종료")});
    ```
    
  코틀린에서의 websocket 예제는 다음과 같겠다(springboot 상정)::
    
    ```
    //data
    package com.example.websoc.chat.domain
    data class ChatMessage (
      var type: MessageType,
      var content: String?,
      var sender: String
    )
    //type
    enum class MessageType {
      CHAT,
      JOIN,
      LEAVE
    }
    //이후 controller, eventListener 추가
    ```


---
<h4>참고 자료</h4>
- https://medium.com/@ykm7003/android-%EC%8B%A4%EC%8B%9C%EA%B0%84-%EC%96%91%EB%B0%A9%ED%96%A5-%ED%86%B5%EC%8B%A0-about-%EC%9B%B9%EC%86%8C%EC%BC%93-8d5b7f6626ec
- https://www.daleseo.com/websocket/
- https://basketdeveloper.tistory.com/entry/Kotlin-Spring-boot-WebSocket-%EC%82%AC%EC%9A%A9%EB%B2%95
