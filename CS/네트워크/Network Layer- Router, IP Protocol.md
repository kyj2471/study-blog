

d네트워크 계층의 목표는 트랜스포트 계층의 segment를 sending host에서 receving host로 전달하는 것이다. 이때 네트워크 계층은 트랜스포트 계층에서 내려보낸 segment를 캡슐화를하고 헤더를 붙혀 데이터 그램을 만든다. 그리고 receiving host에선 이 데이터 그램을 받아 디캡슐화를 해서 내려준다. 네트워크 계층의 프로토콜은 `IP`, `ICMP`, `라우팅 프로토콜` 3가지가 있다.

### 네트워크 계층 대표적 기능 2가지

**- 포워딩** 실제 IP 데이터그램이 인풋포트로 들어올 때 적합한 아웃포트로 패킷을 내보내는것

**- 라우팅** 목적지 별로 어떻게 그 경로를 계산 하는 것

### Network Service Model

`개별적인 데이터그램`에 대해 정의하는 서비스냐 아니면 데이터그램의 흐름에 의해 정의하는 서비스냐에 따라 달라진다. 개별적 데이터그램에 대한 서비스는 각 데이터그램의 delivery를 보장하거나, 특정 지연 시간 내에 delivery를 보장한다. flow of datagram은 최소한의 대역폭(bandwith)을 보장하거나, 플로우에 속하는 패킷간에 도착하는 시간 간격을 특정 간격으로 보장한다.

### Connection, connection-less service

데이터 그램은 network layer에서 connectionless 서비스를 제공한다. 그리고 virtual-circuit은 network layer에서 connection서비스를 제공한다.

### Virtual Circuits

Virtual Circuits(VC)는 가상 회선으로 네트워크 상에서 두 지점 간의 논리적인 경로를 설정하고 데이터를 전송하는 방식이다. 주로 패킷 교환 네트워크에서 사용되며 안정적이며 일관적인 데이터를 전송한다. virtual circuits서비스는 데이터를 내보내기전 call setup을 한다. 그리고 데이터를 다 주고 받은 다음엔 call setup을 분해한다. 이때 call setup을 하는 과정에서 경로가 결정된다. 그리고 call setup이 할당 되는 동안 이 VC의 id가 할당된다. 그리고 경우에 따라 패킷스위칭 이지만 resource를 VC에 할당할 수 있다. virtual circuits을 구현하는 네트워크에서는 VC별로 최소한 경로가 결정되야하고, VC id를 할당해 주어야한다. 그리고 이런 정보를 forwarding table에 저장한다. 그리고 VC 번호는 각각의 링크를 타면서 바뀌게된다.

### Router

router의 두가지 key function은 Routing과 Forwarding입니다.

- routing은 라우팅 프로토콜에 따라 경로를 계산하는데 필요한 정보를 라우터들 끼리 주고받는다.
- 실제 패킷이 들어오면 결정된 경로를 따라 패킷이 목적지를 따라 갈수있게 전달하는 것을 Forwarding이라한다.

![스크린샷 2024-07-15 오후 12.21.18.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/ad79c095-c62e-4268-8fe9-c9d202ae92f5/0d8c8a04-5cb5-4753-8c0b-76b3453fc466/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2024-07-15_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_12.21.18.png)

사용자 데이터를 주고받는 data plane은 빠른속도로 전달되어 하기에 사용자 데이터 전달을 구현하는 건 하드웨어에서 구현된다. 그리고 라우팅 프로토콜 메세지를 주고받는 control plane은 소프트웨어에서 구현된다.

### input port function

![스크린샷 2024-07-15 오후 12.30.37.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/ad79c095-c62e-4268-8fe9-c9d202ae92f5/42979a6e-25e4-4581-a8c4-6521f648011f/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2024-07-15_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_12.30.37.png)

인풋 포트에는 3가지 파트가 있다. big level reception을 하는 physical layer기능을 수행하는 line termination파트가 가장 먼저 온다. 그리고 이어서 line termination에서 비트를 모아 프레임을 형성하면 link layer프로토콜 파트에서 프레임이 홉을 잘 건너왔는지 확인한다. 그 뒤로 프레임이 제대로 된 것이면 목적지로 보내기 위해 다음홉으로 내보내기 위해 가야할 아웃풋 포트를 찾는데 이를 forwarding table lookup이라 한다.

인풋 포트에서 모든 프로세스는 line speed 즉, 데이터가 미디어를 통해 유입되는 속도와 동일한 속도로 내보내 주는게 목표이다. 그리고 인풋포트와 아웃풋 포트를 연결하는 스위칭 속도보다 더 빠르게 데이터그램이 도착하면 큐잉이 발생할 수 있어 버퍼를 둔다.

![스크린샷 2024-07-15 오후 12.35.27.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/ad79c095-c62e-4268-8fe9-c9d202ae92f5/4cbd22d9-0c9c-42db-9692-8e1384cb9f3d/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2024-07-15_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_12.35.27.png)

switching fabric은 인풋포트와 아웃풋 포트를 연결하는데 이 전달 속도가 라우터의 모든 인풋 포트의 전송 속도보다 느리면 라우터의 인풋포트에서 큐가 만들어지고 큐잉 딜레이와 심한 경우 인풋버퍼 오버플로우로 인한 loss도 발생한다. 그리고 인풋포트에서는 Head-of-the-line(HOL)이 발생할 수 있다. 이는 큐의 맨 앞의 데이터그램 때문에 다른 데이터 그램이 switching fabric에 의해 아웃풋포트로 갈 수 있으나 그 기회를 잃는 경우를 말한다. 이런 현상이 나타나는 원인은 switching fabric은 동시에 두개 이상의 데이터 그램을 동일한 아웃풋 포트로 전달하지 못하기 때문이다.

## switching fabrics

switching fabrics의 가장 중요한 특성은 얼마나 빠른 속도로 인풋포트의 패킷을 아웃풋포트로 전달하는 것 입니다. input포트가 n개 이면 n*line rate 정도의 속도면 딜레이가 없을 것이다. switching fabrics의 종류로는 메모리, 버스, 크로스바 세가지가 있다.

### Switching memory

![스크린샷 2024-07-15 오후 12.52.03.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/ad79c095-c62e-4268-8fe9-c9d202ae92f5/ce312d4d-b9f2-46fe-b5c9-01a10b5a899d/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2024-07-15_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_12.52.03.png)

메모리는 일반적으로 first generation 라우터에 사용되었던 switching fabric의 형태이다. 스위칭이 CPU의 직접적인 제어 하에 이루어지면 인풋포트 패킷은 메모리로 카피되고 다시 메모리로 부터 아웃풋 포트로 카피가 이뤄진다. 즉, 인풋포트에서 아웃풋 포트로 가기위해선 메모리를 거쳐 시스템 버스를 두번 타야지만 이동 가능하다. 결론적으로 메모리 bandwith에 의해 속도가 결정된다.

### Switching Bus

![스크린샷 2024-07-15 오후 12.55.35.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/ad79c095-c62e-4268-8fe9-c9d202ae92f5/cf1f3bfe-feb6-478a-bf24-7035df710058/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2024-07-15_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_12.55.35.png)

메모리에 의한 인풋, 아웃풋 포트의 연결의 제약점을 극복하기 위해 둘을 직접 연결하는 독점적인 버스를 Switching fabric으로 사용하는 구조로 사용되었다. 이런 공유되는 버스를 통해 데이터그램이 메모리에서 아웃풋 포트로 이동하면 여러 데이터 그램이 버스를 공유해 속도가 제한되는 bus contention이 발생한다. 이때 스위칭 스피드는 버스의 banwith에 의해 결정된다.

### interconnection network

![스크린샷 2024-07-15 오후 12.59.04.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/ad79c095-c62e-4268-8fe9-c9d202ae92f5/d27ee092-c598-4b6d-b17c-d61ad5617a30/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2024-07-15_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_12.59.04.png)

인풋포트와 아웃풋 포트 사이를 하나의 버스로 연결해 버스 bandwith에 의해 스위칭 스피드가 제한되는 것을 극복하기위해 제안되었다.

### Output ports

![스크린샷 2024-07-15 오후 1.02.54.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/ad79c095-c62e-4268-8fe9-c9d202ae92f5/d22b265c-be45-4fb6-89c3-4d48fb536940/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2024-07-15_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_1.02.54.png)

아웃풋 포트는 네트워크에서 미디어로 데이터를 내보내는 역할이다. 인풋포트와 반대로 데이터그램 버퍼가 제일 먼저 위치하고 링크 계층, 피지컬 계층을 담당하는 line termination파트가 온다. 트랜스미션 미디어에 실을 수 있는 양보다 빠르게 switching fabric을 통해 데이터가 유입되면 이용가능하기 전 까지 데이터를 저장하는 버퍼가 필요하다. 또한 버퍼링된 데이터 그램중 먼저 뽑아낼지 결정하는 scheduling discipline이 결정된다. 그리고 아웃풋 포트에서도 버퍼링이 있으니 도착하는 시간이 뽑는 시간보다 빠르면 큐잉 딜레이가 발생한다.

# IP: Internet Protocol

![스크린샷 2024-07-15 오후 1.14.16.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/ad79c095-c62e-4268-8fe9-c9d202ae92f5/052b91f0-f667-40ff-9511-85074d176dab/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2024-07-15_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_1.14.16.png)

IP protocol은 데이터 전송에 관한 프로토콜을 정의한다. 라우팅 프로토콜은 경로를 선택하고 사용되는 정보를 주고받는 사항을 정의하고 결정된 정보에 의해 데이터 포워딩을 할 포워딩 테이블을 결정한다. ICMP 프로토콜은 에러 보고와 같은 컨트롤 메세지에 의한 시그널에 대한 사항들을 정의한다.

어떤 프로토콜의 역할을 알기 위해선 프로토콜 데이터 유닛의 헤더를 봐야한다.

![스크린샷 2024-07-15 오후 1.25.53.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/ad79c095-c62e-4268-8fe9-c9d202ae92f5/4713c79c-d170-4983-b7e1-c3683d0b77a2/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2024-07-15_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_1.25.53.png)

헤더에는 IP 프로토콜의 버전, 가변적 헤더의 길이, 데이터를 필요로하는 서비스 타입이 들어있다. ttl에는 해당 데이터 그램이 거칠수있는 최대의 홉 양을 표시한다. 홉을 하나 거칠 때 마다 라우터에서 ttl을 하나씩 감소시킨다. upper layer에는 페이로드에 실은 세그먼트를 내려보낸 것이 tcp인지 udp인지를 표시한다. 그래야 어떤 프로세스로 올려보낼지 알 수 있기 떄문이다. length필드에선 헤더를 포함한 데이터그램의 전체 길이를 표기한다. 2번째 줄은 fragmentation과 reassembly에 사용되는 필드이다. option필드는 가변 길이이고 데이터 그램이 경유하는 각 라우터가 자신의 IP 주소를 기록하여 루트를 확인할 수 있게 한다.

### IP fragmentation, reassembly

네트워크의 각 링크에서는 최대의 transfer size가 있다. 이는 링크 계층에서 생성하는 프레임의 가능한 최대 크기가 된다. 이런 데이터그램의 크기가 커지면 라우터는 하나의 데이터를 여러개로 나누는 프레이밍을 하는데 이를 fragmentation이라 한다. 한번 프레이밍된 데이터는 독립적인 취급을 받는다.

reaseemble에 대해 알아보면 assemble의 뜻이 모으다라는 뜻이다. 즉, 다시 모으는 작업이다. 프레이밍된 데이터가 목적지 호스트에 최종적으로 도달한 이후 합쳐지는 것을 의미한다. 인터넷은 복잡한 작업은 다 엔드 시스템으로 보내기에 전에는 독립적으로 취급해 단순히 끝으로 보내기만 한다.

# IP addressing

IP address는 호스트와 라우터 인터페이스를 위한 32비트로 이루어진 식별자이다. 인터페이스는 호스트/라우터와 피지컬계층 사이 커넥션이다. 라우터는 여러 인터페이스를 가지고 호스트는 한두개의 인터페이스만 가진다. 하지만 와이파이 액세스 포인트와 이더넷 스위치는 링크 계층까지만 탑재함으로 IP address를 할당받지 않는다.

## Subnets

![스크린샷 2024-07-15 오후 1.56.35.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/ad79c095-c62e-4268-8fe9-c9d202ae92f5/c09cf404-3a9c-4c77-8879-297df02e41f3/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2024-07-15_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_1.56.35.png)

서브넷은 라우터의 개입 없이 통신할 수 있는 장치 인터페이스이다. IP주소는 서브넷과 호스트로 나뉘는데 IP의 high order bit는 서브넷 파트를 식별하고 low order bit는 호스트 파트를 식별한다. 장치의 IP주소와 서브넷 파트는 동일한 서브넷 파트를 가진다. 위 사진 처럼 223.1.1~로 동일한 서브넷 파트로 이용된다. 그리고 호스트와 라우터를 기준으로 구분해 고립된 서브넷들이 각각 다른 서브넷이 된다.

![스크린샷 2024-07-15 오후 2.01.58.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/ad79c095-c62e-4268-8fe9-c9d202ae92f5/9f9debd6-c47c-4f08-8ad6-63b51a17b4f6/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2024-07-15_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_2.01.58.png)

CIDR은 IP주소체계이며 IP주소의 서브넷 포션의 길이를 고정적으로 정하는 주소 클래스를 없애고 서브넷 포션의 길이를 서브넷에서 필요로 하는 IP주소에 맞춰 가변적으로 둘 수있도록 한다.

### IP address: end system에서 어떻게 IP주소를 할당 받을까?

![스크린샷 2024-07-16 오후 12.34.33.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/ad79c095-c62e-4268-8fe9-c9d202ae92f5/285add50-a731-4850-8163-53c7562b1cf2/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2024-07-16_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_12.34.33.png)

어떻게 호스트나 서브넷이 IP주소를 할당받을 수 있을까? 첫 번째는 시스템 관리자에 의해 IP주소를 하드코딩하는 것이다. 윈도우 시스템의 제어판에 들어가 IP를 타이핑 하는 것과 같다. 이러면 IP 주소가 잘 변경되지 않는다. 두번째로는 DHCP라는 어플리케이션 계층 프로토콜로 서버로 부터 네트워크 연결 할 때 마다 다이나믹하게 그때그때 IP주소를 할당 받는 것이다.

DHCP를 사용하면 호스트가 네트워크에 있는 동안만 IP주소를 할당받고 호스트가 종료하면 다른 호스트에 IP주소를 넘기며 IP주소의 활용도가 높아진다. DHCP는 호스트가 DHCP서버를 발견하기 위해 서브넷에 브로드캐스트하는 DHCP discover 메시지, DHCP서버가 자신을 서브넷에 알리기 위한 DHCP offer메시지, IP주소를 요청하는 DHCP request 메세지와 이에 답하는 DHCP ack메세지가 있다.

![스크린샷 2024-07-16 오후 12.41.00.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/ad79c095-c62e-4268-8fe9-c9d202ae92f5/6afef357-925b-47a9-bb7b-b3b12b274f39/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2024-07-16_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_12.41.00.png)

위의 이미지는 DHCP가 동작하는 방식이다. discover에서 src는 아직 IP주소를 얻지 못했으니 0.0.0.68이다. dest에서는 다음 목적지 주소를 가지고 있다. DHCP는 웰노운 포트번호 67번을 사용한다. yiaddr은 클라이언트를 위해 할당하는 IP주소이다. transaction ID는 id를 기입해 어떤 메세지의 응답인지 기록한다. 다음 offer로 넘어가면 lifetime필드가 추가되는데 이는 클라이언트가 이 주소를 사용할 수 있는 시간을 의미한다.

DHCP는 서브넷에 IP주소 할당 이외 몇가지 중요한 기능을한다.

- 패킷을 처음으로 어디로 내보낼지 첫 홉 라우터 주소를 알려준다.
- 로컬 DNS서버의 이름과 IP주소를 클라이언트에게 알려준다.
- 서버가 할당하는 IP주소에서 서브넷 포션이 얼마 만큼인지 알려준다.

![스크린샷 2024-07-16 오후 12.52.27.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/ad79c095-c62e-4268-8fe9-c9d202ae92f5/c687a797-3299-410a-a93b-cd57db42519e/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2024-07-16_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_12.52.27.png)

서브넷에 들어온 end system(랩탑)이 DHCP 서버로부터 통신을 받는 예이다. 랩탑이 켜지면 DHCP 클라이언트가 동작해 request를 보내고 생성된 request는 랩탑의 프로토콜 스택을 따라가며 UDP 세그먼트에 동봉된다. 이후 서브넷에 브로드캐스트 되며 이더넷 프레임을 DHCP서버가 받아 DHCP링크 계층 이더넷 서버에 demultiplexing(demux)가 일어나 IP계층으로 가고 차례로 올라가 DHCP 서버 프로세스에 도달한다. DHCP 서버 프로세스에서는 ack메세지를 생성해 ack 메세지가 랩탑에서 차례로 demultiplexing이 되며 클라이언트 프로세스가 이를 받아 랩탑이 IP address, 첫 홉, DNS 서버를 알 수 있게된다.

![스크린샷 2024-07-16 오후 12.57.55.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/ad79c095-c62e-4268-8fe9-c9d202ae92f5/eff2d7bd-95b7-4faf-a523-2dc4cccbb261/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2024-07-16_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_12.57.55.png)

서브넷은 사용할 IP주소를 얻기 위해 서브넷을 연결시키는 isp로부터 일련의 주소 블록을 할당받는다. 위의 이미지가 그 블록이다. 8개 기관을 인터넷 접속 기능을 제공하기 위해 사용된다면, isp는 초록색으로 칠해진 3비트를 사용해 각 기관을 식별한다. 그리고 앞의 밑줄부분이 서브넷 아이디로 할당되게 된다.

![스크린샷 2024-07-16 오후 1.05.05.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/ad79c095-c62e-4268-8fe9-c9d202ae92f5/3142206b-29e8-469b-8466-8cb7a6cc53ee/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2024-07-16_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_1.05.05.png)

IP주소가 계층적이면 어떤 영향을 줄까? 두 ISP 중 flybynight는 첫 20비트가 200.23.16이고 r-us는 첫 16비트가 199.31.0.0이다. 포워딩 테이블을 만들기 위해 라우터들은 IP주소를 알아야하는데 isp들은 자기가 사용하는 ip주소를 인터넷에 알려야 하고 계층적인 특성 때문에 flybynight에서 목적지 주소가 200.23.16.0으로 시작하면 모두 내게 보내라는 알람을 보낸다. r-us도 마찬가지이고 이로인해 알려야 하는 정보의 양을 크게 줄일 수 있다. 만약 어떤 기관에서 ISP를 변경하면 그 ISP에 속하는 ip주소로 모든 시스템의 주소를 바꿀 필요가 없다. 대신 org 1이 r-us로 올 때 org1에 해당하는 서브넷 주소를 추가로 advertise하면 된다.

![스크린샷 2024-07-16 오후 1.16.25.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/ad79c095-c62e-4268-8fe9-c9d202ae92f5/36641c0d-5cdc-41af-915d-2746a1a981cc/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2024-07-16_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_1.16.25.png)

인터넷은 서브넷 prefix에 대해 루트를 advertise한다. 라우터는 가까울 수록 더 자세한 포워딩 테이블을 가지고 먼 목적지일 수록 크게 라우트 aggregation한다.즉, 더 짧은 prefix에 대한 포워딩 테이블을 둔다. 그러므로 더 긴 address prefix를 가진 주소로 prefix 매칭이 이뤄진다.

### NAT: netwrok address translation

![스크린샷 2024-07-16 오후 1.22.33.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/ad79c095-c62e-4268-8fe9-c9d202ae92f5/6a0aff1e-10e2-499e-9e56-be1f0ebbe854/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2024-07-16_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_1.22.33.png)

NAT는 IP주소를 효율적으로 활용하기 위해서 사용된다. 서브넷 전체가 하나의 IP주소를 사용하는 방식이다. 위의 이미지 처럼 로컬 서브넷 내의 임의의 prefix를 사용해 각 end system에 IP를 할당하는데 이 서브넷을 인터넷에 연결하는 라우터 쪽에서는 하나의 IP주소가 할당되어있다. 위 이미지의 가운데 라우터가 NAT역할을 한다

NAT의 또다른 기능으로 로컬 네트워크 내에 IP 변경이 자유롭고, ISP가 변경되어도 로컬네트워크에서는 영향이 없어 외부와 로컬 네트워크 관리가 분리되고, 로컬 네트워크 장치가 외부에 보이지 않아 보안적인 측면에서도 좋다. 내부에서 외부로 나가는 데이터그램은 자신의 사설 IP주소와 포트번호를 적어 나가는데 NAT 라우터는 이 두가지를 새로운 주소와 포트번호로 변경한다. 그렇게 해서 외부의 파트너가 데이터그램을 받아 갔을 때 사설에서 보낸 것이 아닌 NAT가 보낸 것으로 응답해 응답을 보낼 때도 NAT로 보낸다. NAT 라우터가 이 응답을 받는데 NAT 라우터는 NAT translation table을 통해 나가는 데이터의 주소와 포트번호 매핑 정보를 기록하고 외부에서 응답이 들어오면 그 정보를 테이블에서 찾아 로컬 디바이스에 전한다.

NAT의 문제점은 서버가 NAT를 통해 클라와 연결되려면 NAT 트랜슬레이션 테이블이 있어야한다. 하지만 이 테이블은 내부 서버가 외부 클라에 데이터를 보내야만 만들어진다. 그러나 서버는 클라의 컨택 없이는 데이터가 나가지 않는데 외부 클라는 내부 서버 주소를 알 길이 없어 테이블이 만들어지지 않는 문제가 있다. 이런 문제를 해결하기 위해 특정 서버 프로세스를 위해 관리자가 미리 직접 테이블 매핑 엔트리를 정의하는 것이고 이런 방법을 자동으로 수행해주는 프로토콜이 UPNP가 있다.

### ICMP: internet control message protocol

![스크린샷 2024-07-16 오후 2.29.40.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/ad79c095-c62e-4268-8fe9-c9d202ae92f5/05261990-be2e-45c7-b7e0-b4359a447a14/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2024-07-16_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_2.29.40.png)

ICMP는 호스트와 라우터 간 에러 리포팅 등 서로 살아있는지 확인하기 위한 네트워크 계층 제어 정보를 교환하는 프로토콜이다. ICMP는 IP보다 상위 계층으로 ICMP 메세지를 IP데이터 그램으로 encapsulate한다. ICMP 메세지는 타입, 코드, 문제가 된 IP 데이터그램의 첫 8바이트가 필드로 담긴다. 타입과 코드에 어떤 문제가 발생했는지 오류 정보가 담겨있다. 문제가 발생한 경우 라우터가 소스에게 전달하지 못하는 데이터그램의 첫 8바이트를 ICMP 메세지에 싣고 코드와 타입을 적어 보낸다. 타입 8이고, 코드 0이면 서로의 네트워크가 살아있다는 echo request이다.

![스크린샷 2024-07-16 오후 2.40.55.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/ad79c095-c62e-4268-8fe9-c9d202ae92f5/bb1e1496-cf89-4271-849b-1b3de36bb979/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2024-07-16_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_2.40.55.png)

위 사진은 ICMP로 구현된 네트워크 애플리케이션 프로그램이다. 호스트 1 gaia에서 eurecom까지 가는데 거치는 모든 라우터의 리스트와 IP주소, 각 라우터 까지 딜레이를 세번 측정한 결과가 보인다. 8번째 줄에 보면 딜레이가 늘어나는 것을 볼 수 있고 대답이 없는경우는 17, 18번째 줄처럼 *이 출력된다.

Traceroute는 소스 호스트에서 UDP세그먼트를 계속 보낸다. 이 UDP 세그먼트는 3개가 한 세트이다. 첫번째로 내보낼 때 헤더에 TTL 필드를 1로 셋팅한다. N번째는 N으로 셋팅한다. n번째 라우터가 이를 ICMP 메세지를 만들고 IP데이터그램을 보낸 소스로 ICMP를 보내며 드랍시킨다. 소스는 이를 확인해 라우터의 이름과 n번째 라우터의 RTT를 알 수 있다. TTL이 충분히 커져 목적지 호스트까지 도달하면 목적지 호스트는 port unreachable이라는 메세지를 보내 소스 경로 파악이 끝났음을 알고 종료한다.

## IPv6

IPv6는 IPv4의 주소 공간 부족 문제를 해결하기 위한 것이다. IPv6는 주소를 128비트로 표현한다. 이는 영원히 고갈되지 않을 수준이다. 또한, 가변적이지 않으며 고정된 길이의 주소 필드를 사용하고 fragmentation을 없애므로 데이터그램의 프로세싱 시간을 단축한다. 아래 이미지의 필드들이 qos로 추가된다.

![스크린샷 2024-07-16 오후 3.06.46.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/ad79c095-c62e-4268-8fe9-c9d202ae92f5/a0c4bf53-8892-4b16-bdaa-17296a82f1e9/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2024-07-16_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_3.06.46.png)

IPv4와 IPv6의 다른점으로 IPv6에서는 헤더에 오류 가 없다 가정하기 떄문에 checksum이 삭제된다. 옵션 필드도 사라진다또한 ICMPv6라는 ICMP의 새로운 버전도 만들어졌다.

![스크린샷 2024-07-16 오후 3.14.32.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/ad79c095-c62e-4268-8fe9-c9d202ae92f5/0e21af6f-6873-4d3e-a3d2-19f18bdfc0c5/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2024-07-16_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_3.14.32.png)

IPv4 체계를 하루아침에 IPv6로 모두 바꿀 수는 없고, 중간중간에 IPv6로 바꿔준다. 그러나 IPv4는 IPv6를 이해할 수 없으므로 "터널링"을 활용하여 IPv4 데이터그램 속에 IPv6를 동봉한다.

![스크린샷 2024-07-16 오후 3.17.44.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/ad79c095-c62e-4268-8fe9-c9d202ae92f5/2fbd5eee-4900-4388-9118-b86e9ad1c0cf/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2024-07-16_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_3.17.44.png)

물리적으로는 중간에 IPv4가 있다. 논리적으로는 B다음의 IPv6인 E쪽으로 연결된다. A가 보낸 메세지를 B는 IPv6이므로 이해한다. 그러나 다음 홉인 C는 IPv4이므로 이해하지 못한다. 다음 IPv6는 E이므로 그 쪽까지 터널링하기 위해 목적지를 E로 설정한 IPv6데이터그램을 IPv4에 동봉한다. 즉 IPv4헤더가 붙는 것이다. 목적지인 E에 도착하면 헤더를 뜯어내므로 헤더가 없어지고 나면 IPv6 데이터그램이 되고, E는 똑같이 IPv6을 다음 홉으로 전달한다. 즉 터널링은 IPv6에 IPv4 헤더를 붙이는 것이다.