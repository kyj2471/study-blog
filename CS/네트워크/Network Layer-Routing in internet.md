

![스크린샷 2024-07-17 오후 7.52.51.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/ad79c095-c62e-4268-8fe9-c9d202ae92f5/d4376df9-2d56-493d-a9b6-f5cf62b37da6/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2024-07-17_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_7.52.51.png)

인트라 AS라우팅 프로토콜은 IGP(interior gateway protocol)라고 부른다. 가장 초기 사용한 인트라 as라우팅 프로토콜은 RIP이고 OSPF, IGRP같은 라우팅 프로토콜도 있다.

![스크린샷 2024-07-17 오후 7.56.11.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/ad79c095-c62e-4268-8fe9-c9d202ae92f5/9f604188-282d-4b55-a5a2-4b106b8e7a39/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2024-07-17_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_7.56.11.png)

RIP는 distance vector 알고리즘을 적용한다. 소규모 네트워크에 적용하기 적합하고 네트워크가 적어 링크가 비슷하고 모든 링크 코스트를 1로 가정한다. 따라서 distance 매트릭이 홉으로 카운팅 된다. 가령 목적지 까지 가는데 3개의 링크를 거치면 디스턴스 링크는 3홉이다. 맥시멈 15카운트이고 16카운트는 무한대를 의미한다. RIP는 advertisement 메세지에 distance 벡터를 실어 이웃과 교환한다. 메세지에 실릴 수 있는 목적지의 개수는 최대 25개 이고 매 30초 마다 교환한다.

![스크린샷 2024-07-17 오후 8.06.53.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/ad79c095-c62e-4268-8fe9-c9d202ae92f5/d3ea4afd-9a96-43f6-b001-6347eb575996/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2024-07-17_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_8.06.53.png)

RIP를 이용하는 경우 라우팅 테이블 예시이다. 파란 구름이 목적지 서브넷이고 총4개다. 각 목적지에 대한 엔트리를 테이블에 가지고 있다. W로 가기 위해 먼저 A로 가야하고 홉 카운트는 2이다. 디스턴스 벡터를 교환하다 이웃인 A가 A에서 Z로의 경로 길이가 4라는 메세지를 보내면 D는 엔트리의 넥스트 홉을 A로 바꾸고 홉 카운트도 바꾼다.

![스크린샷 2024-07-17 오후 8.08.12.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/ad79c095-c62e-4268-8fe9-c9d202ae92f5/a7b3f2af-4187-409e-8006-34fbdf3ec7af/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2024-07-17_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_8.08.12.png)

link failure이 발생해 x와 y 두 라우터 간의 링크가 끊어지면 둘 사이 advertisement가 교환이 안된다. x는 30초 마다 오던 메세지가 180동안 안오면 링크가 fail되었다 생각하고 y를 넥스트 홉으로 하는 모든 루트를 invalidate하고 루트를 새로 계산해 새 advertisement를 이웃에 보낸다. 디스턴스 벡터에서 발생하는 핑퐁 루프를 피하기 위해 RIP는 poision reverse를 쓴다. 그러나 이 방식이 모든 루프를 브레이크 할 순 없다.

![스크린샷 2024-07-17 오후 8.12.05.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/ad79c095-c62e-4268-8fe9-c9d202ae92f5/61b7e970-ba0c-4cd9-9341-50470c5c6ec1/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2024-07-17_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_8.12.05.png)

RIP는 애플리케이션 레벨 프로세스로 구현되었는데 이 프로세스 이름을 route-d 혹은 daemon이라 한다. 이 프로세스는 운영체제 안에서 계속 돌며 30초 마다 advertisement메세지를 UDP에 보내고 UDP는 세그먼트로 encapsulate해서 IP계층으로 내보내고, IP계층은 이웃 라우터로 보내 다시 이웃의 route-d로 올라가 포워딩 테이블이 업데이트 된다.

![스크린샷 2024-07-17 오후 8.14.04.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/ad79c095-c62e-4268-8fe9-c9d202ae92f5/06f6a06a-9461-4af9-8d2d-494b63664522/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2024-07-17_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_8.14.04.png)

OSPF라우팅 프로토콜은 퍼블릭하게 이용 가능하다. 링크 스테이트 알고리즘 기반 프로토콜로 링크 스테이트 패킷을 네트워크 전체에 알리고 모든 라우터들은 네트워크 전체의 topology map을 그리고, 그래프 전체에 자신을 소스로 하는 다익스트라 알고리즘을 적용해 라우팅 테이블을 작성한다. 서브넷 별 엔트리가 있던 RIP와 달리 OSPF에는 각 엔트리가 이웃별로 있다. OSPF는 IP위에 구현이 되어 UDP세그먼트로 인캡슐레이트 되지 않고 바로 이웃 IP에 전달된다.

![스크린샷 2024-07-17 오후 8.16.37.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/ad79c095-c62e-4268-8fe9-c9d202ae92f5/113e2f9e-b159-40ed-a031-e80787d05bf4/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2024-07-17_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_8.16.37.png)

OSPF가 RIP에 비해 진보된 점이다. 규모가 큰 AS에 적용이 가능하고 계층적인 OSPF를 적용한다. 또 딜레이, bandwith, cost등을 기준으로 하는 링크 코스트를 다양하게 사용한다. 각각의 링크 스테이트에 대한 advertise가 필요하고 각각의 라우팅 테이블을 별도로 계산해야 한다. 심지어 복수의 링크 코스트도 사용 가능하다. 또 OSPF의 프로토콜 메세지는 authenticated하여 진짜 유효한 라우터가 보낸지를 식별한다. 또 여러 라우터에 메세지를 동시에 보내는 MOSPF 라우팅이 있다.

![스크린샷 2024-07-17 오후 8.20.16.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/ad79c095-c62e-4268-8fe9-c9d202ae92f5/29666c67-4099-401e-bb18-5f3d48a1295b/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2024-07-17_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_8.20.16.png)

위처럼 OSPF가 큰 규모의 AS를 다룰 수 있음을 확인이 가능하다. 이러면 노드와 링크가 많아 계층적인 OSPF가 사용된다. 어떤 한 링크에서 지나가는 링크 스테이트 패킷이 양이 많아지기 떄문이다. 즉, as의 규모가 제한되고 as를 몇개의 구역으로 나누고 링크 스테이트 브로드캐스팅을 구역안에서만 하여 전체 as규모가 늘어나더라도 구역을 일정하게 하면 한 링크에서 오가는 패킷의 양은 변함이 없게 한다.

그러나 한 지역 내 라우터는 구역 밖에있는 서브넷 혹은 라우터로 가는 경로를 모른다. 따라서 각 구역마다 area border 라우터를 두어 우리 area안의 서브넷 각각 거리를 계산해 이를 area 라우터 끼리 교환한다.

area border 라우터를 포함해 backbone area가 다시 구성이 된다. 이 역할은 개별적 area를 붙여주는 역할을 한다. backbone내의 자체적 OSPF가 돌며 이 안에서 링크 스테이트가 브로드캐스팅 된다. 따라서 백본내에 속한 border area 라우터 길이를 정확히 알고 있다. 이를 통해 개별 area 내부의 정보와 area border가 backbone을 통해 area 외부의 경로 정보를 알게되어 최종적으로 경로를 알 수 있다.

따라서 local area와 백본인 two-level hierarchy이다. local area와 백본 내에서 OSPF 라우팅이 이뤄진다. area border라우터는 내부 area의 정보를 담은 summary를 교호나하고 boundary라우터는 또 다른 as 데이터를 교환한다.

![스크린샷 2024-07-17 오후 8.30.45.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/ad79c095-c62e-4268-8fe9-c9d202ae92f5/896762e7-909c-4640-8f01-7318b12fe7ff/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2024-07-17_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_8.30.45.png)

inter as routing인 BGP이다. 각 as는 자기가 원하는 intra as 라우팅 프로토콜을 선택할 수 있다. 그런데 as간을 연결하는 프로토콜은 각 as가 다른 프로토콜을 사용해서는 안된다. 다 같이 동일하게 사용해야 딜리버리 되기 떄문이다. 따라서 인터넷에서는 단 하나의 inter as 라우팅 프로토콜을 사용하는데 이것이 BGP이다. eBFP는 서브넷에 도달 가능한 정보를 이웃 as로 부터 알아오고 iBGP는 그렇게 알아온 정보를 as내부에 소문내주는 프로토콜이다. as내부에는 이 정보를 통해 외부 서브넷으로 가는 좋은 경로를 선택하는데, 이 BGP가 중요하게 생각하는 것은 정책이다. 가령 어떤 as에서 목적지 as로 갈 때 경로상 거치는 as중 보안상 거치면 안되는 as 혹은 비용이 너무 큰 as등이 있다면 다른 경로를 택하는 것이다.

![스크린샷 2024-07-17 오후 8.37.15.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/ad79c095-c62e-4268-8fe9-c9d202ae92f5/cb62c87a-1601-4296-b05b-d0f85573853d/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2024-07-17_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_8.37.15.png)

BGP 라우터는 이웃한 것 끼리 만들어지며 서로 반 영구적 TCP 커넥션을 맺는다. 이 커넥션을 통해 BGP advertisement를 교환한다. 가령 as3에서 as1에 어떤 서브넷 프리픽스 X에 대해 그 경로를 advertise를 하면 as1이 as3으로만 메세지를 넘기면서 알아서 x로 포워딩 해준다는 약속을 한다.

![스크린샷 2024-07-17 오후 8.40.53.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/ad79c095-c62e-4268-8fe9-c9d202ae92f5/1ee3ece0-0ec7-4f1e-8aa7-8057dda48b91/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2024-07-17_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_8.40.53.png)

BGP라우터가 주고받는 경로 정보의 핵심 내용은 목적지 서브넷의 prefix와 그 경로로 가기 위한 attribute로 구성된다. attribute의 핵심 요소는 서브넷 prefix로 가는데 거쳐야 할 as 리스트인 AS-PATH와 첫번째 as라우터에서 도달하는 넥스트 홉 라우터의 정보를 적는 next-hop이 필요하다. 이경우에 따라 as가 복수의 게이트 웨이를 사용할 경우 각 게이트웨이 메세지마다 어느곳으로 보낼지를 명시해야 하기 때문이다. 게이트웨이 라우터가 advertisement를 들으면 information을 우리 as에서 쓸 것인지를 지정하는 import policy를 수립하게된다.

![스크린샷 2024-07-17 오후 8.47.25.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/ad79c095-c62e-4268-8fe9-c9d202ae92f5/82c5c8af-181d-4cbd-b491-d3a6ecc95926/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2024-07-17_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_8.47.25.png)

BGP는 세션을 오픈하기 위해 OPEN 메세지를 이웃 라우터에 줘 TCP커넥션이 일어난다. OPEN요청을 받은 라우터는 이를 받는 ack메세지로 keep alive메세지를 보낸다. 이렇게 세션이 생성되면 BGP 라우터를 새로운 경로 정보를 발견하거나 이전에 보낸 정보를 다시 보낼 때 마다 UPDATE메세지로 갱신한다. 또한 어떠한 이벤트가 발생하지 않으면 TCP커넥션이 종료 됨으로 이를 막기위해 KEEPALIVE메세지를 정기적으로 주고 받는다. NOTIFICATION 메세지는 이전 메세지에서 오류가 생기면 보고하고 BGP 세션을 종료하고자 할 때 사용한다.

![스크린샷 2024-07-17 오후 9.00.18.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/ad79c095-c62e-4268-8fe9-c9d202ae92f5/b3d843cf-60d0-4704-a0a2-22d6bd94d6e4/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2024-07-17_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_9.00.18.png)

BGP와 OSPF는 어떻게 협력하여 다른 AS의 목적지를 위한 포워딩 테이블을 세팅할 수 있을까? 3개의 as가 연결된 예가 있다. as는 BGP 메세지로 루트 정보를 교환한다. 이 정보는 목적지 서브넷의 프리픽스, 목적지 서브넷으로 가는 경로 정보인 attributes (AS-PATH, next-hop)이다. as path는 목적지 as로 가기 위해 거쳐야 하는 as의 리스트 이고 넥스트 홉은 as path를 통해 지나가는 첫번째 as이다.

하나의 라우터가 동일한 서브넷 프리픽스에 대해 루트 정보를 받을 수 있다. 따라서 라우터는 그 중 하나를 선택해야 한다. 가장 먼저 고려하는 선택 정보가 policy이다. 만약 정책이 같으면 as path가 짧은 것이 우선이다. 만약 이마저 같으면 가까운 홉이 있는 곳을 결정하는 hot potato 라우팅을 적용한다.

![스크린샷 2024-07-17 오후 9.04.18.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/ad79c095-c62e-4268-8fe9-c9d202ae92f5/0bb8810e-3e25-4222-a10b-82b328efaee1/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2024-07-17_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_9.04.18.png)

최단 as path가 결정되면 as path상의 첫번째 as를 연결하는 라우터인 next hop attribute를 이용한다. 위 예시에서 첫번째 as가 as2의 첫 라우터인 2a로 연결한다. 출발지에서 2a로 가는 최단 경로는 OSPF에 의해 발견하고 결정한다. 최종적으로 해당 프리픽스에 대한 포워딩 테이블이 작성된다.

![스크린샷 2024-07-17 오후 9.11.47.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/ad79c095-c62e-4268-8fe9-c9d202ae92f5/285e49fd-2068-456f-9b8c-2108c43a8633/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2024-07-17_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_9.11.47.png)

프리픽스에 대한 여러 경로가 있는데 만약 경로들끼리 정책이 같으면 as path가 짧은 것이 우선이다. 만약 이도 같으면 가장 가까운 홉이 있는 곳을 결정하는 hot potato 라우팅을 적용한다. 1c는 as3과 as2에서 정보를 받는데 as3으로 가는 넥스트 홉은 3a 이고 as2는 2a 인데 3a가 더 가까우니 as3을 선택한다.

![스크린샷 2024-07-17 오후 9.13.16.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/ad79c095-c62e-4268-8fe9-c9d202ae92f5/e0fb1f4d-1388-4dc8-ac49-175c8b01ed1d/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2024-07-17_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_9.13.16.png)

요약하면 라우터는 BGP를 주고 받아 prefix를 발견하고, BGP를 통해 prefix로 가는 여러 외부 루트 중 가장 좋은 경로로 as route를 결정하며, OSPF를 통해 내부 as-routing으로 외부 inter-as0route next hop으로 가는 최단 경로를 알아내고 그 최단 경로로 가기 위해 라우 터의 어떤 포트로 내보내야 하는지 판단한다. 그리고 프리픽스에 대한 라우터 포트가 결정되면 포워딩 테이블에 넣는다.

![스크린샷 2024-07-17 오후 9.14.56.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/ad79c095-c62e-4268-8fe9-c9d202ae92f5/0cd9e3e4-3bf1-4c20-8cf9-29b5ebbba7a6/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2024-07-17_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_9.14.56.png)

![스크린샷 2024-07-17 오후 9.18.10.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/ad79c095-c62e-4268-8fe9-c9d202ae92f5/df137fd9-bc81-4beb-a82d-97af44c911a6/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2024-07-17_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_9.18.10.png)

BGP에서 advertisement를 하기 위해 적용되는 정책을 알아보면 a,b,c는 w,x,y에 대한 provider네트워크이다. w,x,y는 customer이다. x는 두개의 네트워크에 연결된 dual-homed이다. x는 본인이 알게된 루트 정보를 b나 c에게 제공하지 않는다. x는 b와 c에 대한 provider가 아니기 떄문이다.

2번째 정책으로 A는 W가 본인에게 딸린 프리픽스를 알려주면 A에서 W로 가는 as path를 B와 C에게 알려준다. 이들에게 알려줘야 W로 가는 메세지가 왔을 때 A로 전해줘야, 본인의 customer로 가는 것을 전해줄 수 있기 때문이다. B는 BAW 경로를 x에게 알려준다 그래야 x가 w로 갈 때 어디로 보낼지 알기 떄문이다. 그러나 B는 이 정보를 C에게 알려주지 않는다. C나 w가 본인 customer가 아니라 의무가 없기 때문이다. 따라서 본인에게 revenue가 생기냐에 따라 알림 여부가 결정된다.