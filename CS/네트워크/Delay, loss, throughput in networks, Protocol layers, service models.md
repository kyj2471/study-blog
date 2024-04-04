
---

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

# Protocol Layers

프로토콜은 방대하고 복잡해서 레이어링 구조를 가지고있다. 이렇게 레이어링으로 모듈화 하면 복잡한 시스템에서 각각의 피스를 identification하고 관계를 정의하기에 용이해진다. 또한, 업데이트하고 유지보수하는데 용이하다. 하지만 이런 레이어링으로 인해 각 레이어는 각각의 기능을 동작하는데 중복된 기능이 오버래핑되는 경우가 있다.

## Internet protocol stack

### application

유저 애플리케이션 프로그램에서 발생된 데이터를 캡슐화해서 만들어준다. ex) FTP(file transfer), SMTP(email), HTTP(web)

### transport

source-destination(end to end)간에 어플리케이션 메세지를 전달해주는 역할 ex) TCP, UDP

### network

라우팅을 통해 source에서 destination까지 배달하기위해 홉 바이 홉으로 전달할 수 있게 전달해주는 작업을 한다. ex) IP, routing protocols

### link

홉바이 홉으로 데이터를 전달해야하는데 이렇게 홉간의 연결을 해준다. ex)Ethernet, PPP

### physical

링크계층에서 한홉을 넘어가기 위해 physical계층에 데이터 실어주는 역할을 한다.

![스크린샷 2024-04-04 오후 7.18.37.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/898fd640-37c6-4c5e-b62a-1705c22fa72e/c206d6a7-069c-4867-a94d-cba165a05d6e/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2024-04-04_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_7.18.37.png)

위의 사진으로 source에서 destination까지 가는 과정에 대해 풀어보겠다.

우선 애플리케이션에서 메세지를 생성한다. 그리고 process to process통신을 위해 transport계층에 내려주면서 destination까지 전달을 목표로한다. 이 때 destination에서 목표한 목적지에 도착했다 알려주기위해 segment헤더를 추가한다.

그후 네트워크 계층에선 host to host딜리버리를 제대로 동작하게 하기위해 헤더를 붙히게되는데 이는 segment에 추가로 헤더를 설정하고 이를 데이터그램이라한다.

그리고 링크 계층에 전달하며 다음 홉을 건너갈 것을 부탁하고 다시 헤더를 추가하고 이를 Frame이라 한다.

마지막으로 물리 계층을통해 데이터를 밀어넣게 되고 switch의 물리 계층에서 받게되고 이를 링크 계층으로 다시 올려주어 다음 홉으로 넘어가게한다. 이 때 switch에서 다음 홉을 갈 수있도록 다시 Frame을 만들어 router에 전달한다.

다시 라우터의 물리계층에서 받아 switch의 frame을 복구해서 헤더를 확인해서 한홉을 잘 건너왔다는 것을 확인 후 해당 헤더를 제거하고 네트워크계층으로 올려준다.

그리고 router의 network계층에서 source네트워크에서 받은 헤더를 확인해 목적지를 위한 다음 홉을 결정하고 다시 헤더를 제거하고 추가하여 destination물리계층에서 받는다.

destination에서 link에서 받은 frame은 라우터에서 생성한 frame을 받게되고 한 홉을 잘 건너왔다는 것을 확인후 제거후 네트워크 계층에 올려주고 네트워크 계층역시 router의 네트워크 계층에서 만든 데이터그램을 확인후 내가 destination인걸 확인 후 다음 홉으로 보내는 것이 아닌 destination에서 transport계층으로 올려주고 transport에서 source의 transport에서 만든 segment를 확인 후 최종적으로 헤더를 제거후 최초 source에서 생성한 메세지만 destination의 애플리케이션 계층으로 올려줍니다.

ps) 각 프로토콜 계층에서 만든 데이터 유닛을 PDU(protocol data unit)이라 부른다