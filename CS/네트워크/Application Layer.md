# Application layer

어플리케이션 아키텍쳐는 두가지 구조를 가진다.

- client-server
- peer-to-peer(P2P)

## Client-Server architecture

### server host

- 항상 on되어있는 상태이다.(always-on)
- 영구적으로 할당된 IP주소를 가지고있다.
- 데이터 센터 형식으로 존재하기도 합니다.

### Client host

- server host와 커뮤니케이션 합니다.
- 인터넷에 커넥트 될 때 마다 변경 가능한 IP주소를 가지고있다.
- 인터넷에 연결되었다 안되었다 합니다.(서버 호스트와 반대로)

## P2P architecture

- 커뮤니케이트하는 두 host가 user host에있습니다.
- always-on server형태가 아닙니다.
- 다른 peers에 요청하기도하고 제공하기도 합니다. 즉, 별도의 서버 없이 서로의 peer끼리 커뮤니케이트라 시스템의 규모가 커지더라도 확장성있게 가져갈 수있다,
- 관리가 복잡하다.

## Sockets

프로토콜 레이어링에서 애플리케이션에서 메세지를 만들면 transport계층으로 내려주고 destination에서도 transport계층에서 application계층으로 올려주는데 이때 애플리케이션 계층과 transport계층간에 연결되는 문이 있는데 이를 소켓이라 한다.

## Addressing processes

메세지를 전달하기 위해서는 정확한 주소가 필요하다. 이때 호스트를 식별하는 IP주소와 포트번호를 통해서 식별합니다. 한 호스트에는 여러 프로세스가 발생하고있다.

HTTP server의 포트번호는 80, mail server는 25와 같이 보편적으로 사용되는 server포트번호는 고정되어있다. 이와 같은 포트 번호를 well-known포트 번호라 한다

## Transport Protocols Services

### TCP

- **reliable transport** → 연결 지향적(connection-oriented)으로 동작하기에 신뢰성이 높다.
- **flow control** → sending TCP에서 빠르게 데이터를 보냄으로 수신 TCP에서 버퍼가 가득차게되면 커넥션을 통해 이 상황을 알리는 flow control을 한다.
- **connection-oriented** → 상대방 TCP와 handshacking으로 연결을 지향한다. 이를 통해 각 TCP는 버퍼에 패킷을 받을 때 sending TCP가 더 빠른 속도로 보내면 받는 TCP의 버퍼는 너무 빠르게 보내고있다고 sending쪽에 알려줄 수 있고 flow를 조율할 수 있다.
- **congestion control** → 네트워크가 오버로드 시 송신자 조절을 한다.

### UDP

- **unreliable data transfer →** 100% 데이터의 안정성을 보장받지 못한다.
- 연결이 없습니다. 데이터를 전송하기 위해 사전에 연결을 설정할 필요가 없습니다. 이로 인해 속도가 빠르지만, 데이터 전송의 순서와 안정성에 대한 보장이 없습니다.
- TCP는 순차적으로 데이터를 전송하는데 반해 데이터를 실시간으로 전송하는 데 적합합니다. 실시간 비디오 스트리밍, 음성 통화, 온라인 게임 등에서 사용됩니다. 신속한 데이터 전달이 중요한 경우에 적합합니다.

## Application protocol에서 정의된 것

- 교환하는 메세지 종류를 정의 합니다. (e.g> request, response)
- 메세지 syntax정의
- 어떤 request를 받을때 어떤 타이밍에 어떻게 response해야하는지에 대한 규칙을 정의
- HTTP, SMTP와 같은 오픈된 application protocol은 RFC문서에 전부 오픈되어있다.

---

# Web and HTTP

웹페이지는 여러개의 objects로 구성이되고 웹페이지의 틀은 base HTML-file을 묘사해 주고 이 파일에는 여러 objects들에 대한 reference정보가 있다.(object는 HTML file, JPEG image, Java applet, audio file…등)

## HTTP(hypertext transfer protocol)

web application을 위해 디자인 되었다. client-serer모델을 사용하고있다. 여기서 브라우저가 client프로세스이다. 웹페이에서 특정 url로 진입시 서버에 필요한 내용을 request하고 받아서 화면을 보여줍니다. server프로세스는 request를 기다리다 요청이 들어오면 해당하는 웹페이지(objects)를 전송해주는 역할을 한다.

HTTP라는 application protocol은 전송 요청을 TCP에 요청한다. HTTP가 서포트하는 메인 application이 web이고 웹은 데이터의 무결성이 중요하기 떄문에 TCP에 요청한다.

사용자가 url을 입력시 웹서버에서 그 url에대한 웹페이지를 받기위해 HTTP request message를 만드는데 그전에 TCP를 사용하기에 TCP커넥션을 initiate해야합니다. 이 때 HTTP는 항상 port번호 80번이고 서버의 IP주소와 port번호를 이용해서 서로 handshacking이 맺어지고 그 후 HTTP메세지를 주고받습니다. 이렇게 데이터를 주고 받고 TCP커넥션이 닫히게 됩니다.

그리고 HTTP는 ‘stateless’입니다. 즉, http는 유저가 이전에 보낸 request가 어떤것이였는지 기억하지 않습니다. HTTP는 또한 TCP connection은 response를 주면 연결을 close하는 Non-persistent이다.

# HTTP

TCP위에서 동작

### [ Non-persistent HTTP ]

하나의 오브젝트 보내면 서버에서 TCP커넥션을 닫는다.

여러개의 오브젝트로 구성되어있다면 각 오브젝트를 위해 하나의 오브젝트 보내고 닫고를 반복해야한다.

### 순서

1. 특정 url을 검색한다.
2. client가 server(80번 포트번호를 가지고 대기중)로 tcp커넥션을 initiate한다.
3. server가 accept하게 되어 커넥션을 맺고 클라이언트에 알려준다.
4. client가 리퀘스트 메세지를 전송한다.
5. server가 tcp커넥션을 종료한다.
6. client는 리스폰스 메세지에 실려있는 html file을 디스플레이 해준다.
7. html file을 파싱해 추가적인 reference jpeg objects를 발견하지만, 이미 tcp커넥션이 닫겨있음으로 2-6을 반복한다.

### response time
유저가 url을 타이핑한 후 웹페이지가 디스플레이되기까지 걸린 시간을 말한다.