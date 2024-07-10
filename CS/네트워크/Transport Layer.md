---

transport layer는 application 커뮤니케이션 간에 logical 커뮤니케이션을 제공합니다.
프로토콜로는 TCP, UDP가 대표적이다 각각의 주요 특징은 아래와같다

TCP

- congestion control (network를 오버하지 않기 위해)
- flow control (수신자의 buffer를 오버하지 않기 위해)
- connection setup

UDP

- TCP처럼 network계층에게 뭔가를 해주는게 아닌 proccess delivery만 해줌
- no handshacking
- 각 segment는 각자의 목정지 정보를 가지고 있다.
- 네트워크가 데이터를 전송하면 전송하는 거다 데이터가 유실되거나 손상되어도 알 수 없다.

TCP와 같은 연결 지향은 여러개의 소켓을 열고 source ip, source port, destination ip, destination port의 정보를 알아야 하지만(multiplexing) UDP의 경우는 하나의 소켓만 오픈하고 destination ip, destination port정보만 있으면된다(demultiplexing).

UDP header인 segment에서 checksum필드가 있다. 이는 오류 검출을 해주는 필드이지만 UDP는 단순 자신이 받은 segment에 오류가 발생 유무만 확인하고 끝이다. 그 결과로 어떤 동작을 할지 프로토콜에서 정해지지 않다. 또는 오류가 있는 경우 데이터를 드롭시키는 경우도있다.

### TCP

- **reliable, in order byte stream**

TCP는 데이터를 네트워크 계층에 전송전 버퍼에 데이터를 담아두고 해당 데이터를 올려보내거나 받을 때 congestion-control, flow control을 한다. 이 과정에서 통신간에 너무 데이터가 커지지 않게 byte stream으로 본다

- **point - to - point**

TCP는 sender하나에 receiver하나로 point to point이다.

- full duplex connection

TCP의 클라이언트 프로세스와 서버 프로세스에서 TCP connection을 맺으면 클라이언트에서 서버쪽으로 메세지를 보낼 수 있지만 반대로 동일한 커넥션을 통해 서버가 보내기도한다.

- piplelined

위의 byte stream의 내용이다. TCP는 파이프라인에서 메세지를 주고 받을 때 해당 파이프라인 효율을 높이기 위해 즉, 둘 사이의 throuput의 작업량을 높이기 위해 한번에 전송가능한 최대의 데이터 크기를 파이프에 쌓아놓는다. 이때 최대한의 사이즈를 우린 window라 하고 이 window는 flow control과 congestion control에 따라 결정된다.

### TCP sequence number, acks

- **TCP sequence number**

시퀀스 번호는 세그먼트 페이로드에 실려있는 데이터의 첫번째 바이트의 번호

- TCP acks

상대방이 다음 어떤 시퀀스 번호를 보낼지에 대한 기대이다. 즉, 이 acks의 number 전까진 데이터를 전부 잘 받았다는 의미이다.

### TCP sender events

TCP는 sender가 될수도 receiver가 될수도있는 계층이며 timeout이 발생할 수 있다.
TCP가 application layer로 부터 데이터를 받을 때 segment(header)를 붙힌다. 그리고 그 헤더에 믿을 수 있는 데이터를 위해 sequence number를 붙혀야하며 checksum을 계산하여 segment의 checksum필드에 추가한다. 그리고 timeout이 발생하면 발생한 segment를 재전송 해준다.

### TCP: retransmission scenarios

![스크린샷 2024-07-03 오전 7.45.42.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/ad79c095-c62e-4268-8fe9-c9d202ae92f5/49cefaaf-85ba-4ba2-8e2d-233d713e97b2/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2024-07-03_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_7.45.42.png)

첫번째 case는 sender는 host A, receiver는 host B인 상황. seq num은 92이며 payload에는 8byte의 데이터가 들어있을 것이다. 즉, 총 99까지 잘 받았기에 host B 는 ACK를 100을 보냄으로 100번째 byte를 기다리고 있다 알려준다. 하지만 더이상 보낼 데이터가 없는 host A는 timeout이 발생하게 될 것이고 이때 다시 데이터를 내보내고 ACK를 받는다.

두번째 case는 sender인 Host A에서 seq num 92, 100을 전송 했으며 각 8byte와 20byte의 데이터를 전송했고 host B역시 받은 데이터에 대해 ACK를 100과 120을 전송했다. 하지만 첫번째 ACK를 받기 이전 timeout이 발생하였고 seq num 92인 8byte의 데이터를 재 전송하게된다. 이때 host B에선 이미 받았던 데이터 임을 확인하고 마지막 ACK 120을 재 전송해준다.

### TCP fast retransmit

일반적으로 time-out period는 길다. 그렇기에 실제 loss가 발생하여 time-out이 발생하면 이것이 복구되는데 오래 걸린다. 이것을 보안하기위해 sender에서 복제된 ACK가 반복적으로 들어오면 sender측에선 time-out이 발생하기 전에 재전송을 하는데 이를 fast retransmit이라 한다.

![스크린샷 2024-07-03 오전 8.14.33.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/ad79c095-c62e-4268-8fe9-c9d202ae92f5/f2c400ab-0296-4105-ab69-459f060e1404/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2024-07-03_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_8.14.33.png)

위 사진에서 Host A에서느 5개의 segment를 Host B에 전송을 하였다. 이때 Host B에서는 2번째 segment의 데이터 loss가 있었고 이에 seq num을 99까지만 잘 받았다고 ACK 100을 Host A에 리턴하게된다. 이때 Host A는 duplicated된 ACK를 확인하고 timeout이 발생하기 전 seq 100의 segment를 재전송한다.

### TCP flow control

![스크린샷 2024-07-03 오전 8.40.11.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/ad79c095-c62e-4268-8fe9-c9d202ae92f5/363f421a-5c39-4936-953f-ea395fe9f1b9/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2024-07-03_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_8.40.11.png)

위의 사진에서 링크계층에서 frame헤더를 받은것을 확인후 네트워크 계층으로 IP헤더를 확인 후 TCP로 내려보낸다. 위 사진의 노란색으로 표시된 헤더가 segment이다. 이때 TCP에서는 데이터를 buffer에 담아 저장하며 데이터 전송을 하는데 이때 버퍼의 구조는 아래와 같다.

![스크린샷 2024-07-03 오전 8.43.36.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/ad79c095-c62e-4268-8fe9-c9d202ae92f5/309d36ec-f789-406c-8402-fc61b030a077/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2024-07-03_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_8.43.36.png)

buffer의 남은 공간을 rwnd라 하는데 이 free buffer space인 rwnd를 TCP헤더 추가하여 전송한다. 그리고 이렇게 함으로 sender는 rwnd를 넘지 않도록 데이터를 전송하게 함으로 buffer가 overflow되는걸 막는데 이것을 flow control이라한다.
