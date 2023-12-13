# 목차

1. [**양방향 통신에서의 HTTP**](#1-양방향-통신에서의-http)
2. [**Websocket**](#2-websocket)
3. [**Websocket 작동방식**](#3-websocket-작동방식)
4. [**Websocket의 문제점**](#4-websocket의-문제점)
5. [**샘플 코드**](#5-샘플-코드)

## 1. 양방향 통신에서의 HTTP

> HTTP의 비연결성(Connectionless)

- HTTP는 기본적으로 연결을 유지하지 않는 통신 방식
  - 서버와 클라이언트의 연결이 일시적
  - 서버 자원을 효율적 사용 가능
  - 단, 매 요청 처리시마다 TCP/IP 연결을 매번 새로 해야 한다.
- 비연결성으로 인해 실시간 통신이 필요한 "채팅", "실시간 데이터" 부분에서 한계를 보임
- 기존 HTTP 방식은 요청 처리시 페이지 이동이 발생합니다.

### AJAX

> XMLHttpRequest 객체가 서버에 요청 <br>
> Vue 에서는 Axios가 해당 역할 수행

    - 페이지 갱신 없이 서버와 비동기 통신 가능
    - 동일한 페이지 안에서 DOM을 조작하는 방식
    - HTTP 전송 중에도 페이지 사용 가능

  <!-- http ajax 비교이미지 -->

![HTTPvsAJAX](https://github.com/SSAFYSEOUL06CSSTUDY/06CSSTUDY/assets/108852263/e71d8d70-095d-4cee-922a-e181d00a3ce7)

### AJAX의 문제점

- 여전히 요청후 응답이라는 과정 을 통해 동작
- 반드시 요청 헤더가 전송되기에 큰 틀에서 HTTP를 벗어나지 못 함

## 2. Websocket

> 웹소켓은 HTML5 표준 기술로, 사용자의 브라우저와 서버 사이의 동적인 양방향 연결 채널을 구성

- 서버와 클라이언트가 지속적으로 양방향 데이터 전송을 유지해야 할 때 웹소켓을 사용한다.
- 하나의 HTTP 접속으로 양방향 메세지 전송 가능

> Websokcet의 특징

- 한번의 HTTP 통신으로 접속 유지
- HTTP, AJAX와는 반대로 장시간 연결을 위해 설계
- 장시간 연결이 필요한 서비스 구축시 HTTP 기반 통신보다 서버 부담이 적다<br>
  => 헤더가 중복 전송되지 않아 오버헤드가 적다.
- HTTP를 대체하는 개념 X, 실시간 통신에서 보완하는 개념 O
- 서버에서 클라이언트를 호출 할 수 있기에 완전한 양방향 통신

- 최초 접속시에만 http프로토콜 위에서 handshaking을 하기 때문에 http header를 사용
- 웹소켓을 위한 별도의 포트는 없고, 기존 포트를 사용
- 프레임으로 구성된 메시지라는 논리적 단위로 송수신
- 메시지에 포함될 수 있는 교환 가능한 메시지는 텍스트와 바이너리

## 3. Websocket 작동방식

<!-- 작동방식 요약이미지 -->
<img width="447" alt="스크린샷 2023-12-13 오전 9 08 48" src="https://github.com/SSAFYSEOUL06CSSTUDY/06CSSTUDY/assets/108852263/3a7aa256-d0a1-4301-928b-13dab3880a7a">

### (1) 웹 소켓 요청

#### [ 클라이언트 => 서버 ]

```
[예시 코드]
GET /chat
Host: https://chanstory.dev
Origin: https://chanstory.dev
Connection: Upgrade
Upgrade: websocket
Sec-WebSocket-Key: ...
Sec-WebSocket-Version: 13
```

- HTTP 버전은 1.1이상
- 반드시 GET메서드를 사용
- 헤더에 소켓을 사용하기 위한 Upgrade, Connection, WebSocket에 관한 정보를 함께 전송
- Sec-Websocket-Key는 클라이언트가 요청하는 여러 서브 프로토콜을 의미

### (2) 웹 소켓 응답

#### [ 서버 => 클라이언트 ]

```
HTTP 1.1 101 Switching Protocols
Upgrade: websocket
Connection: Upgrade
Sec-WebSocket-Accept: ...

```

- 연결이 정상적으로 되면 101 Switching Protocols 응답이 클라이언트로 전송
- Sec-WebSocket-Accept : Sec-Websocket-Key에 문자를 추가 후 암호화 한 값, 인증 키 값 개념

### (3) 웹 소켓 통신

이후 'ws' 혹은 'wss' 프로토콜을 이용해 양방향 통신을 진행

## 4. Websocket의 문제점

### (1) 호환성

> HTML5 이후에 나온 기술<br>
> 때문에 이전 버전인 IE 등에서 사용이 불가능

### Socket.io

> HTML5 이전의 기술로 구현된 서비스 에서 웹 소켓처럼 사용할 수 있도록 도와주는 기술(Javascript 기반)

<!-- 크로스 브라우징 이미지 -->

![websocket_cross](https://github.com/SSAFYSEOUL06CSSTUDY/06CSSTUDY/assets/108852263/adec5f52-5765-4fe1-8afe-324f492ee86e)

### (2) 연결 비정상 종료시 불명확한 에러코드

> 서버와 클라이언트 간의 연결이 끊어졌을 때 생성되는 에러 메세지가 구체적이지 않아 디버깅이 어려움

## 5. 샘플 코드

### (1) 웹 소캣 서버 [server.js]

<details>
<summary>공지사항 기능으로 양방향 통신 테스트</summary>

```
const WebSocket = require("ws");
const readline = require("readline");

const server = new WebSocket.Server({ port: 3000 });

server.on("connection", (ws) => {
  ws.on("message", (message) => {
    console.log(`받은 메시지: ${message}`);
    // 서버로부터 받은 메시지를 모든 클라이언트에게 전송
    server.clients.forEach((client) => {
      if (client.readyState === WebSocket.OPEN) {
        client.send(message);
      }
    });
  });
  ws.send("모의 채팅 서버에 오신 것을 환영합니다!");
});

const rl = readline.createInterface({
  input: process.stdin,
  output: process.stdout,
});

console.log("WebSocket 서버가 ws://localhost:3000에서 시작되었습니다.");
console.log("연결된 모든 클라이언트에게 알림을 보내려면 메시지를 입력하세요:");

rl.on("line", (input) => {
  server.clients.forEach((client) => {
    if (client.readyState === WebSocket.OPEN) {
      client.send(`[공지사항]: ${input}`);
    }
  });
});

```

</details>

### (2) 메세지 변환 코드

<details>
<summary>Blob을 텍스트로 변환</summary>
```
const chatBox = document.getElementById("chatbox");
const messageInput = document.getElementById("messageInput");

const webSocket = new WebSocket("ws://localhost:3000");

webSocket.onmessage = async function (event) {
let message;
if (event.data instanceof Blob) {
message = await event.data.text();
} else {
message = event.data;
}
chatBox.innerHTML += `<div>${message}</div>`;
chatBox.scrollTop = chatBox.scrollHeight;
};

function sendMessage() {
const message = messageInput.value;
if (message) {
webSocket.send(message);
messageInput.value = "";
}
}

```
</details>
```
