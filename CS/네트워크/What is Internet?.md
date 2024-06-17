
---

# What is Ineternet

인터넷이란 network of networks이다. 라우터, hosts, communication link등이 전부 섞여 뭉치뭉치 있다. 이런 뭉치를 네트워크라고 생각했을 때 internet은 이런 네트워크들의 네트워크라고 생각하면된다. 이런 인터넷은 모두가 같은 방식으로 소통해야하기에 표준화가 중요하다. 이 기관은 IETF이고 여기서 RFC라는 표준안들을 발표한다.

**hosts (end-systems)**

사용자의 어플리케이션을 호스팅해준다고 해서 호스트이다. 그리고 네트워크의 가장자리에 있다 해서end systems이라 부른다.

**router(switches)**

사용자의 메세지를 목적지를 찾아가게해준다. 즉, source부터 destination까지 가기위해는 여러 라우터를 타야한다.

**communication links**

이런 라우터와 host를 연결해주는 물리회선이다.

**protocols**

인터넷에서 메세지를 받고 제어하는 일련의 규칙이다. protocol에서는 보내고 받는 메세지의 포맷, 보내고 받는 순서, 메시지를 받았을 때 어떤 액션을 취해야하는지를 정의해준다.

- IETF: 인터넷 프로토콜 표준화 기관
- RFC: 표준안 발표

---

# Network Edge

### access networks and physical media

유저 호스트(end systems)를 네트워크에 연결해주는걸 Access Networks라고한다. 여기서 중요한 개념으로 bandwith(transmission rate)가 있는데 이 bandwith는 단위 시간당 실어나를 수 있는 비트 수를 의미한다. 흔히 우리가 Mbps, Gbps라고 불렀던 것이다. 이 bandwith는 공유할(shared)수도 분리되어(dedicated) 사용 될 수도 있다.

### Access net: digital subscriber line (DSL)

집에 다 kt, skt, lgu+같은 회사들이 인터넷에 연결해준다. 이런 회사들이 Access network을 제공해주는 회사이다.

이렇게 전화 회사에서 access networks를 제공해줄 수있게 해주는게 DSL이다. 그리고 이 전화회선은 옆집과 함께 shared하는게 아닌 dedicated즉 분리되어있다. 컴퓨터는 DSL모뎀에 연결된다. 그리고 각집에서 DSL모뎀을 통해서 데이터가 전화 회선을 통해 회사의 central office에 DSLAM(DSL Access Multiplexer)로 보내주게된다. 이 때 voice이면 telephone networks로 데이터면 internet networks로 연결 해 준다.

### Access net: cable networks

DSL이 아닌 케이블 회사를 통해 연결될 때는 CMTS라는 멀티플렉싱 장비가있다. 케이블 모뎀을 통해 나온 데이터를 인터넷으로 넘겨주는 역할을 한다.

이 케이블 네트워크와 DSL은 서로 다른점으로 하나의 회선을 탄다. 즉, 하나의 회선을 공유한다. bandwith가 큰 회선을 하나 연결하고 해당 링크를 여러 집에 shared한다. 즉, dedicated link가 아닌 shared link이다. 여기서 이 링크를 HFC(hybrid fiber coax)라고 부른다. 집집마다 연결은 coax로 연결 그리고 큰 대역폭을 전송해줘야하는 CMTS에서 인터넷으로의 연결은 fiber로 연결되어있다.

## Access Net: home network

우린 집에서 주로 와이파이를 사용한다. 이는 집 안에서 home network를 구성하고있기에 가능하다. 데스크탑이 홈네트워크로 연결 → 홈네트워크는 케이블 네트워크, 전화회사 네트워크로 연결되어있고 home network의 코어에 라우터가있다. 이 라우터가 집의 각각의 end system을 묶어 연결해준다. 그리고 이 라우터가 cable 모뎀, DSL 모뎀에 연결되어 멀티플렉싱 장비가있는 central office로 데이터를 전송한다.

## 집이아닌 학교나 기관은 어떻게 연결될까?

학교에서 공부를 하면 컴퓨터실을 예로 뒷편에 큰 검정색의 물체가 있는걸 본 적있다. 이를 ethernet switch라고 하는데 ethernet switch의 수많은 포트는 각각의 end systems를 연결해준다. 그리고 이런 ethernet switch도 한개가 아니라 학교나 기관(회사)에 많이 있다. 이런 이더넷 스위치는 학교 전체나 회사 전체로 연결해주는 라우터로 연결된다. 그리고 이 라우터는 인터넷에 연결해주는 ISP(Internet Service Provider)의 라우터로 연결된다. 그리고 dedicated line을통해 라우터가 ISP에 직접 연결된다.

## Host: sends packets of data

네트워크 어플리케이션을 호스트한다고해서 호스트라 부른다. 이 호스트에서는 어플리케이션 메세지가 발생하고 이를 Access Network로 내보내게된다. 이때 패킷이라는 chunk로 잘라서 보낸다. 이때 패킷 전송 delay의 계산은 아래와 같다.

`packet transmission delay = L(bits) / R(bits per seconds)` 즉, 패킷의 크기를 Access Network의 transmission rate로 나누는 것이다.

## Physical media

링크의 종류는 두가지로 나뉜다. 물리적 와이어를 사용하는 guided media, 사용하지 않는 unguided media이다.

Guided Media 사용

- copper (Ethernet)
- fiber, coax (HFC)

Unguided media 사용

- radio(wifi, cellular)

여기서 radio link type의 예로 LAN(wifi), wide-area(cellular), statellite가 있습니다. 이는 물리적인 케이블에 비해 단점으로 방해물을 만났을 때 시그널이 reflection이 일어날 수있고 물리적인 미디어를 타는게 아니라 주변 노이즈에 민감하고 radio signal끼리도 서로 방해된다. 장점으로는 물리적 와이어를 설치하지 않고도 통신이 가능하다는 장점이 있다.

---

# Network Core

network edge에서 각 호스트를 하나로 연결해주는 router의 목적은 source로부터 destination까지 유저의 어플리케이션 메세지를 전달하는게 목표이다. 이때 전달해주는 방법은 Circuit switching, packet switching 크게 두가지가 있다.

## Circuit switching

전화 네트워크에서 사용되었다. 특징으로 유저 메세지를 전달하기전 무조건 Call이 있어야한다. 이때 유저가 call을 하면 call set up과정을 거친다. 이때 하는일은 source로 부터 destination까지 어떤 라우터를 거쳐갈지 경로를 정하고 해당 자원을 예약(resource reservation)된다.

### FDM(Frequency Division Multiplexing) vs TDM(Time Division Multiplexing)

FDM은 별도의 frequency를 할당받아 할당받은 frequency를 사용할 수있다. TDM은 frequency전체를 사용자가 사용할 수있게 하되 시간으로 잘라서 사용할 수 있게 제공된다.

## Packet Switching

인터넷이 나오면서 데이터 네트워크에는 circuit switching이 매우 비효율 적이다. 순간적으로 어플리케이션 메세지가 발생을 했다가 한동안 아무런 요청이 없을 수도있는데 circuit switching처럼 하나의 경로를 할당받는게 매우 비효율 적이기 때문이다. 그래서 새로운 기법으로 packet switching이 등장했다.

패킷 스위칭은 call set up과정이 없다. 즉, resource reservation역시 일어나지 않는다. 필요한 경우 그때 그때 사용할 수있게 해준다. 만약 유저 애플리케이션 메세지의 크기가 크다면 패킷으로 잘라서 Access Network로 전송하여 각 패킷이 링크를 점유하는 시간을 고정적으로 차지합니다. 아래 패킷 스위칭 특징을 보자.

### store-and-forward

패킷스위칭에서 라우터는 패킷 전체가 도달할 때 까지 받는 작업(store)만 하게된다. 그 후 전부 받으면 주소를 파싱해서 보내줍니다. 이를 store-and-forward라고한다. (각 패킷 마다 목적지 정보를 가지고있다)

### queing delay, loss

패킷이 목적지로 보내지는 것 보다 받는 양이 더 많으면 queing delay가 발생한다. 이는 resource reservation이 이뤄지지 않고 통신을 했기 때문이다. 이때 queing할 수 있는 공간이 full로 채워졌을 때 계속 패킷 메세지가 라우터에서 받을 때 데이터 Loss가 발생할 수있다.

# Internet structure

인터넷은 이전 블로그에서 네트워크들의 네트워크라 했었다. 즉, 구조를 가지고있고 연결되어 있다. 수많은 Access network들은 각각 서로 1 to 1연결이 아닌 global ISP에 라우터들에 각각 연결되어 있고 이 ISP의 라우터들이 서로 경로를 가지고 있게 연결되도록 설계되었다. 그리고 이 ISP들끼리 역시 서로 연결되어있다(peering link) 또는, 이 ISP끼리 연결해 주는 것 중에 IXP가있다. 결론적으로 Access network가 ISP에 연결만 되어있으면 된다.

---

# Delay, loss, throughput in networks

# Four sources of packet delay

패킷이 한 홉을 지나가는데있어 delay가 일어나는 4가지 종류가 있다.

**nodal processing** 노드에 패킷이 도착했을때 패킷이 도착하는데 있어 에러가 있었는지, 다음 어느링크로 가야할지 결정하는 경우이다.

**queueing delay 버퍼에서 다음 경로로 가는데 해당 패킷의 차례가될 때까지 delay된다.**

**transmission delay 패킷을 링크에 넣어주는 시간이 있다. 패킷의 크기와 링크의 bandwith에 의해 결정된다. `L/R`**

**propagation delay 비트가 링크에 실려있지만 시그널이 전달되는 시간이다. 링크의 길이, 전파속도에 따라 결정된다.**

### packet loss

queue의 크기는 유한하다. 그런데 큐의 길이가 점점 길어져 꽉 차게된 경우에 패킷이 도착하면 패킷을 버릴 수 밖에 없고 이 상황에서 loss가 발생된다.

### Throughput

단위 시간당 source로부터 destination까지 배달한 트래픽의 양이다. 여기서 source to destination까지 연결된 수많은 링크중 가장 capacity가 작은 링크가 이 Throughput을 결정하고 해당 링크를 bottleneck link라고한다. 그리고 bottleneck현상은 network core가 아닌 network edge에서 발생한다.

---

# Protocol Layers

프로토콜은 방대하고 복잡해서 레이어링 구조를 가지고있다. 이렇게 레이어링으로 모듈화 하면 복잡한 시스템에서 각각의 피스를 identification하고 관계를 정의하기에 용이해진다. 또한, 업데이트하고 유지보수하는데 용이하다. 하지만 이런 레이어링으로 인해 각 레이어는 각각의 기능을 동작하는데 중복된 기능이 오버래핑되는 경우가 있다.

## Internet protocol stack

### application

유저 애플리케이션 프로그램에서 발생된 데이터(메세지)를 캡슐화해서 만들어준다. ex) FTP(file transfer), SMTP(email), HTTP(web)

### transport

source-destination(end to end)간에 어플리케이션 메세지를 전달해주는 역할 ex) TCP, UDP

### network

라우팅을 통해 source에서 destination까지 전달하기위해 홉 바이 홉으로 전달할 수 있게 전달해주는 작업을 한다. ex) IP, routing protocols

### link

홉바이 홉으로 데이터를 전달해야하는데 이렇게 홉간의 연결을 해준다. ex)Ethernet, PPP

### physical

링크계층에서 한홉을 넘어가기 위해 physical계층에 데이터 실어주는 역할을 한다.

# source에서 destination까지 가는 과정

source에서 destination까지 가는 과정에 대해 풀어보겠다.

- 우선 애플리케이션에서 메세지를 생성한다.
- process to process통신을 위해 transport계층에 내려주면서 destination까지 전달을 목표로한다. 이 때 destination에서 목표한 목적지에 도착했다 알려주기위해 segment헤더를 추가한다.
- 네트워크 계층에선 host to host딜리버리를 제대로 동작하게 하기위해 헤더를 붙히게되는데 이는 segment에 추가로 헤더를 설정하고 이를 데이터그램이라한다.
- 링크 계층에 전달하며 다음 홉을 건너갈 것을 부탁하고 다시 헤더를 추가하고 이를 Frame이라 한다.
- 물리 계층을통해 데이터를 밀어넣게 되고 switch의 물리 계층에서 받게되고 이를 링크 계층으로 다시 올려주어 다음 홉으로 넘어가게한다. 이 때 switch에서 다음 홉을 갈 수있도록 다시 Frame을 만들어 router에 전달한다.
- 다시 라우터의 물리계층에서 받아 switch의 frame을 복구해서 헤더를 확인해서 한홉을 잘 건너왔다는 것을 확인 후 해당 헤더를 제거하고 네트워크계층으로 올려준다.
- router의 network계층에서 source네트워크에서 받은 헤더를 확인해 목적지를 위한 다음 홉을 결정하고 다시 헤더를 제거하고 추가하여 destination물리계층에서 받는다.
- destination에서 link에서 받은 frame은 라우터에서 생성한 frame을 받게되고 한 홉을 잘 건너왔다는 것을 확인후 제거후 네트워크 계층에 올려주고 네트워크 계층역시 router의 네트워크 계층에서 만든 데이터그램을 확인후 내가 destination인걸 확인 후 다음 홉으로 보내는 것이 아닌 destination에서 transport계층으로 올려주고 transport에서 source의 transport에서 만든 segment를 확인 후 최종적으로 헤더를 제거후 최초 source에서 생성한 메세지만 destination의 애플리케이션 계층으로 올려줍니다.

ps) 각 프로토콜 계층에서 만든 데이터 유닛을 PDU(protocol data unit)이라 부른다