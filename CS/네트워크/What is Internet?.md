
---

# What is Internet

인터넷이란 network of networks이다. 라우터, hosts, communication link등이 전부 섞여 뭉치뭉치 있다. 이런 뭉치를 네트워크라고 생각했을 때 internet은 이런 네트워크들의 네트워크라고 생각하면된다. 이런 인터넷은 모두가 같은 방식으로 소통해야하기에 표준화가 중요하다. 이 기관은 IETF이고 여기서 RFC라는 표준안들을 발표한다.


**hosts**

사용자의 어플리케이션을 호스팅해준다고 해서 호스트이다. 그리고 네트워크의 가장자리에 있다 해서 end systems이라 부른다.

**router(switches)**

사용자의 메세지를 목적지를 찾아가게해준다. 즉, source부터 destination까지 가기위해는 라우터를 타야한다.

**communication links**

이런 라우터와 host를 연결해주는 물리회선이다.

**protocols**

인터넷에서 메세지를 받고 제어하는 일련의 규칙이다. protocol에서는 보내고 받는 메세지의 포맷, 순서, 메시지를 받았을 때 어떤 액션을 취해야하는지를 정의해준다.

# Network Edge, Network Core

### access networks and physical media

유저 호스트(end systems)를 네트워크에 연결해주는걸 Access Networks라고한다. 여기서 중요한 개념으로 bandwith가 있는데 이 bandwith는 단위 시간당 실어나를 수 있는 비트 수를 의미한다. 흔히 우리가 Mbps, Gbps라고 불렀던 것이다. 이 bandwith는 공유할(shared)수도 분리되어(dedicated) 사용 될 수도 있다.

### Access net: digital subscriber line (DSL)

집에 다 kt, skt, lgu+같은 회사들이 인터넷에 연결해준다. 이런 회사들이 access network이다.

이렇게 전화 회사에서 access networks를 제공해줄 수있게 해주는게 DSL이다. 그리고 이 전화회선은 옆집과 함께 shared하는게 아닌 dedicated즉 분리되어있다. 컴퓨터는 DSL모뎀에 연결된다. 그리고 각집에서 DSL모뎀을 통해서 데이터가 전화 회선을 통해 회사의 central office에 DSLAM(DSL Access Multiplexer)로 보내주게된다. 이 때 voice이면 telephone networks로 데이터면 internet networks로 연결 해 준다.

### Access net: cable networks

DSL이 아닌 케이블 회사를 통해 연결될 때는 CMTS라는 멀티플렉싱 장비가있다. 케이블 모뎀을 통해 나온 데이터를 인터넷으로 넘겨주는 역할을 한다.

이 케이블 네트워크와 DSL은 서로 다른점으로 하나의 회선을 탄다. bandwith가 큰 회선을 하나 연결하고 해당 링크를 여러 집에 shared한다. 즉, dedicated link가 아닌 shared link이다. 여기서 이 링크를 HFC(hybrid fiber coax)라고 부른다. 집집마다 연결은 coax로 연결 그리고 큰 대역폭을 전송해줘야하는 cmts에서 인터넷으로의 연결은 fiber로 연결되어있다.

## Access Net: home network

우린 집에서 주로 와이파이를 사용한다. 이는 집 안에서 home network를 구성하고있기에 가능하다. 데스크탑이 홈네트워크로 연결 → 홈네트워크는 케이블 네트워크, 전화회사 네트워크로 연결되어있고 home network의 코어에 라우터가있다. 이 라우터가 집의 각각의 end system을 묶어 연결해준다. 그리고 이 라우터가 cable 모뎀, DSL 모뎀에 연결되어 멀티플렉싱 장비가있는 central office로 데이터를 전송한다.

## 집이아닌 학교나 기관은 어떻게 연결될까?

학교에서 공부를 하면 컴퓨터실을 예로 뒷편에 큰 검정색의 물체가 있는걸 본 적있다. 이를 ethernet switch라고 하는데 ethernet switch의 수많은 포트는 각각의 end systems를 연결해준다. 그리고 이런 ethernet switch도 한개가 아니라 학교나 기관(회사)에 많이 있다. 이런 이더넷 스위치는 학교 전체나 회사 전체로 연결해주는 라우터로 연결된다. 그리고 이 라우터는 인터넷에 연결해주는 ISP(Internet Service Provider)의 라우터로 연결된다. 그리고 dedicated line을통해 라우터가 ISP에 직접 연결된다.