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
유저가 url을 타이핑한 후 웹페이지가 디스플레이되기까지 걸린 시간을 말한다.response time은 아래와같다

response time = 2RTT + file transmission time

RTT(Round Trip Time)란 아주 작은 크기의 패킷이 클라이언트에서 서버에 갔다가 클라이언트로 돌아오는데까지 걸린 시간을 말한다.

한번의 오브젝트를 받은 후 서버 tcp를 닫아버리기 때문에 매 오브젝트마다 tcp새로 열어야한다.

→ 각 오브젝트마다 2번의 RTT필요 (5번의 오브젝트라면 2 * 5 )

### [ Persistent HTTP ]

TCP 커넥션 오픈한채로 유지하므로 request message ↔ response message 계속 주고 받는다.

request message 무시할 만큼 짧다고 해도 1RTT이다.

### [ HTTP Requset Message ]

HTTP메세지는 두가지 종류를 사용합니다. reqiest, reponse이다. 이 메세지는 아스키코드로 되어있다.

**request line** 어떤 작업을 해달라고 서버에 요청하는 것이며 메서드(GET) 뒤의 파일을 보내달라고 요청하는 것 입니다.

**HOST** HTTP서버의 호스트입니다.

**User-Agent** 클라이언트 브라우저에대한 정보입니다.

**Accept-Language** 특정 웹페이지에 여러 언어 버전이있습니다. 이렇게 en-us로 보내면 영어버전을 서버에서 보내주게됩니다.

**Connection** 서버에게 가능하면 TCP connection을 keep open하게(persistent)동작하게 하도록 요청하는 것 입니다.

# HTTP response message
**Status line** 서버의 HTTP버전, status코드 정보를 제공한다.

Date 리스폰스를 제공하는 시간

**Server** 서버의 종류 정보 제공

**Last-Modified** 이 웹페이지가 마지막으로 수정된 시간 정보

**Connection** 요청한대로 Connection을 닫지않고 open된 상태로 제공하겠단 의미

## Status codes

**200ok** request가 성공하여 response의 body에 request된 objects가 실려있다는 정보

**301 Moved Permanently** request된 objects가 다른곳에 옮겨져 저장되어있다. 이 경우 헤더라인에 location이 존재하고 값으로는 objects가 저장된 url정보를 제공한다.

**400 Bad Request** request메세지가 잘못되어 이해가 불가능하다.

**404 Not Found** request document가 이 웹서버에는 없다는 뜻

**505 HTTP Version Not Supported** 클라이언트 HTTP버전을 서버가 서포트 불가능하다.

## User-server state: cookies

목적 → http가 stateless인데 이를 보완해 서버사이트와 트랜잭션에 히스토리를 유지하게 하기 위해 사용
1. client에서 아마존 server측에 msg를 발생시킨다.
2. 아마존 서버는 이 client에 ID를 생성해 부여하고 이 정보를 backend database에 추가한다.
3. response message에 header line에 set-cookie정보를 전송한다.
4. client는 이 cookie데이터를 cookie file에 저장한다.
5. 추후에는 서버와 client의 통신시 해당 쿠키를 통해 database에 access한다.

이렇게 쿠키를 사용함으로 유저의 로그인 상태를 계속 유지할 수 있다. 흔히 우리가 부르는 세션로그인이 결국은 쿠키 때문에 가능하다.

## Web caches

cache를 로컬 사이트에 둠으로 server까지 안가더라도 어떠한 object를 가져올 수 있게 한다. 브라우저에서 캐시를 통해서 web access를 가능하게 셋팅을 한다 했을 때 브라우져는 http request시 cache에 요청하고 만약 해당 데이터가 있는 경우는 바로 response를 해주고 아닌경우는 서버에 request해 object를 받아온다. 이처럼 서버같은 역할을 해준다 해서 web cache를 proxy server라 부르기도 한다.

회사, 대학, 지역 ISP에서 web cache를 사용하는 이유는 response time이 줄어들고 또한 상위 ISP와 통신 할 떄 요청이 많아지면 traffic으로인해 높은 throuput을 요구하게된다. 하지만 web cache로 인해 상위 ISP와의 traffic을 감소할 수 있다.

web cache없이 서로의 ISP가 서로 access link를 통해 커뮤니케이션할 때 로컬에서 100Mbps로 데이터를 밀어 넣는데 ISP상에서 throuput이 이 데이터를 밀어 넣는 양을 견디지 못하면 문제가 된다. 이 상황에서 물론 throuput을 늘려 해결 할 수 있지만 이러면 높은 cost가 발생할 것이다. 그렇기에 로컬 server에서 web cache를 이용해 traffic감소를 할 수 있다.

### Conditional GET

하지만 위의 경우 로컬 cache에서 계속 같은 object만 전송하면 실제 server에서는 해당 data에 변화가 있었을 수 있다. 그렇기에 server에 Conditional GET을 요청한다. 헤더 정보에 `if-modified-since:<date>` 를 전송하고 date에는 last-modified에 정보를 보낸다. 만약 modified되지 않았다면 response로 throuput에서는 영향을 주지 않게 modified되지 않았다는 정보와 함께 object는 전송하지 않는다. 그렇지 않고 변경이 있었던 경우는 data를 response로 보내준다.

---

# Electronic mail

mail server는 두가지 스트럭쳐를 가지고있다. mailbox, message queue가 있다. 만약 user가 메일을 작성해서 전송하면 message queue에 저장된다. mailbox는 사용자 별로 있고 message queue는 사용자 별로 있지 않다.

**user Agent로는** eg) Outlook, iPhone mail client가 있다.

유저가 메일을 작성하면 SMTP프로토콜을 통해 sender’s mail server에 전송되고 이를 다시 수신자 에게 전송하는데 역시 SMTP를 사용하게된다. 그리고 최종적으로 mail을 받을 때는 POP, IMAP, HTTP 프로토콜이 사용된다.

**SMTP** 프로토콜은 TCP위에서 동작한다. well-known port는 25번에서 대기한다. 전송할 땐 handshaking → 메세지를 전송 → closure 3가지로 이뤄진다.

---

# DNS: domain name system

웹브라우져에 웹사이트를 access할 때 www.naver.com과 같이 작성한다. 우리는 이를 host name 또는 domain name이라 한다. 일반 사용자들은 이렇게 domain name을 작성하지만 클라이언트 프로세스에서 서버 프로세스를 컨택할 땐 host name이 아닌 32bit로 이루어진 IP주소를 사용한다. **그래서 host name을 IP주소로 매핑해 주는 system이 필요하다.** 이러한 매핑을 제공해주는 시스템이 DNS이다.

여기서 DNS는 웹 서버가 여러개인 경우 사용자가 host name으로 접근시 서로 다른 웹 서버로 load distribution 역할 역시 수행한다. 이런 DNS는 유지보수, traffic volume등의 이유로 centralize되지 않고 분산해 놓는다.

TLD(Top-Level-Domain)는 DNS에서 가장 높은 수준의 도메인을 의미한다. 주로 보는 `.com` `.net` `.kr` 과 같은 부분을 TLD라 한다.

이런 DNS는 3단계 hierarchy로 나눠져 있는데 아래와 같다.

- authoratative → 기관내 등록된 호스트들에 대한 host to IP주소에 대한 정보를 알고 있다.
- TLD → 기관별 authoratative 서버 정보
- Root → 가장 큰 도메인인 TLD 서버에 대한 정보

### DNS : caching, updation, records

DNS 시스템에서 메인 시스템이 쿼리에 대한 reply를 일시적으로 캐싱을 할 수 있어 reply의 response 시간을 줄일 수 있고 traffic 역시 줄일 수 있다. 이런 캐싱은 항상 out-of-date가 문제가 된다. 그렇기에 전달받은 TTL이 만료 될 때 까지 만 캐싱한다.

## DNS records

DNS는 distributed database에 대한 정보를 저장하고있다. 이 저장하는 정보를 Resource Records라고 부른다. `RR format: (name, value, type, ttl)` 로 이루어져있다. 여기서 name과 value를 결정짓는 것이 type이다.

- `type=A`
    - name은 hostname
    - value는 IP 주소
- `type=CNAME`
    - name에 alias name이 들어간다.
    - value에는 canonical(실제 이름)이 들어간다 예로 www.ibm.com의 canocical name은 servereast.backup.ibm.com과 같을 수 있다.
- `type=NS` (TLD서버에서 알고있다. → authoritative server정보를 저장함)
    - name에는 domain
    - value에는 authoritative name server

---

### P2P applications

- end systems끼리 서로 communicate
- always-on-server가 아니다
- torent, skype가 예로 있다.

p2p구조의 경우에는 파일을 원하는 클라이언트 수가 늘어나면 파일을 업로드할 수있는 capability도 늘어나 클라이언트 수에 비례해 파일 업로드시간이 늘어나지 않아 확장성이 좋다.