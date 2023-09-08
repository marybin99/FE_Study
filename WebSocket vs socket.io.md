# WebSocket vs socket.io
`WebSocket`은 양방향 소통을 위한 프로토콜  
(* 프로토콜: 서로 다른 컴퓨터끼리 소통하기 위한 약속)   
`socket.io`는 양방향 통신을 하기위해 웹소켓 기술을 활용하는 라이브러리   
→ 자바스크립트와 jQuery의 관계와 비슷


## WebSocket
- HTML5 웹 표준 기술
- 매우 빠르게 작동
- 통신 시 아주 적은 데이터 이용
- 이벤트를 단순히 듣고, 보내는 것만 가능

#### 예제 (Node.js)
```powershell
$ npm install ws
```

- 서버 측

```js
const WebSocket = require("ws");

const wss = new WebSocket.Server({ port: 3000 });

wss.on("connection", (ws) ==> {
    ws.on("message", (message) => {
        console.log("received: %s", message);
    });

    ws.send("something");
});
```

- 클라이언트 측

```js
const ws = new WebSocket("ws://localhost:3000");

ws.on("open", () => {
    ws.send("something");
});

ws.on("message", (data) => {
    console.log(data);
});
```

## socket.io
- 표준 기술 X, 라이브러리
- 소켓 연결 실패 시, fallback으로 다른 방식으로 알아서 해당 클라이언트와 연결 시도
- 방 개념을 이용해 일부 클라이언트에게만 데이터 전송하는 브로드캐스팅이 가능
- socket.io 이용시 클라이언트 측에서 반드시 `socket.io-client` 라이브러리 이용

같은 기능을 구현하더라도 약간 느리지만, 많은 편의성 제공.   
Java, C++, Python 등 여러 언어들의 라이브러리 또한 지원

#### 예제 (Node.js)
```powershell
$ npm install ws
```

- 서버 측

```js
const server = require("http").createServer();

const io = require("socket.io")(server);
io.on("connection", (socket) => {
    socket.on("message", (msg) => {
        console.log(msg);
    });
});

server.listen(3000);
```
(`express`라면)
```js
const app = require("express")();
const server = require("http").createServer(app);
const io = requier("socket.io")(server);

io.on("connection", (socket) => {
    /* ... */
});

server.listen(3000);
```

- 클라이언트 측 (socket.io-client)

```powershell
$ npm install socket.io-client
```

```js
const io = require("socket.io-client");

const socket = io("http://localhost:3000");

socket.emit("message", "hello world!");
```

## 무엇을 사용하는 게 좋을까
- 서버에서 연결된 사용자들을 세밀하게 관리해야 하는 서비스인 경우   
-> `socket.io`가 유지보수 측면에서 이점 多   
- 데이터 전송이 많은 경우   
-> 빠르고 비용이 적은 표준 `WebSocket` 이용


> toy project 중 socket.io를 사용하였는데, 발표 중 웹소켓과 socket.io의 차이를 아는지에 대한 질문을 받고 의문이 들어 공부하게 됨!
