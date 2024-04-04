# Network Edge, Network Core

---

## Host: sends packets of data

네트워크 어플리케이션을 호스트한다고해서 오린 호스트라 부른다. 이 호스트에서는 어플리케이션 메세지가 발생하고 이를 Access Network로 내보내게된다. 이때 패킷이라는 chunk로 잘라서 보낸다. 이때 패킷 전송 delay의 계산은 아래와 같다.

`packet transmission delay = L(bits) / R(bits per seconds)` 즉, 패킷의 크기를 Access Network의 transmission rate로 나누는 것이다.

## Physical media

링크의 종류는 두가지로 나뉜다. 물리적 와이어를 사용하는 guided media, 사용하지 않는 unguided media이다.

Guided Media 사용

- copper (Ethernet)
- fiber, coax (HFC)

Unguided media 사용

- radio(wifi, cellular)

여기서 radio link type의 예로 LAN(wifi), wide-area(cellular), statellite가 있습니다. 이는 물리적인 케이블에 비해 단점으로 방해물을 만났을 때 시그널이 reflection이 일어날 수있고 물리적인 미디어를 타는게 아니라 주변 노이즈에 민감하고 radio signal끼리도 서로 방해된다. 장점으로는 물리적 와이어를 설치하지 않고도 통신이 가능하다는 장점이 있다.

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

패킷스위칭에서 라우터는 패킷 전체가 도달할 때 까지 받는 작업(store)만 하게된다. 그 후 전부 받으면 주소를 파싱해서 보내줍니다. 이를 store-and-forward라고한다.

### queing delay, loss

패킷이 목적지로 보내지는 것 보다 받는 양이 더 많으면 queing delay가 발생한다. 이는 resource reservation이 이뤄지지 않고 통신을 했기 때문이다. 이때 queing할 수 있는 공간이 full로 채워졌을 때 계속 패킷 메세지가 라우터에서 받을 때 데이터 Loss가 발생할 수있다.

# Internet structure

인터넷은 이전 블로그에서 네트워크들의 네트워크라 했었다. 즉, 구조를 가지고있고 연결되어 있다. 수많은 Access network들은 각각 서로 1 to 1연결이 아닌 global ISP에 라우터들에 각각 연결되어 있고 이 ISP의 라우터들이 서로 경로를 가지고 있게 연결되도록 설계되었다. 그리고 이 ISP들끼리 역시 서로 연결되어있다(peering link) 또는, 이 ISP끼리 연결해 주는 것 중에 IXP가있다. 결론적으로 Access network가 ISP에 연결만 되어있으면 된다.