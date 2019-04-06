#### 19.04.03
#### 19.04.07

## keywords
network layer, router, IP address, CIDR, subnet, NAT, DHCP, routing algorithm, Dijkstra, Bellman-Ford, AS

## Network layer
- 전송 계층 아래에 위치하여 segment에 헤더를 붙여 호스트간에 전달하는 계층
- 라우터가 실제 전송 과정에 관여하는 계층. 즉, 라우터는 network/ data link/ physical layer를 가진 실체이다
- 전송 계층의 two functions
  - forwarding: 한 라우터에서 input으로 받은 packet을 어디로 output 할지 정하는 일. forwarding table에서 longest prefix matching이 되는 network interface로 내어놓음
  - routing: source 에서 dest 사이 라우터들을 정하는 일. 라우팅 알고리즘이 결정

## IP datagram(packet)
![IP datagram](https://user-images.githubusercontent.com/38183218/55678267-b2d3b800-5931-11e9-91f8-5c623aa0f7b3.JPG)

- fragmentation과 reassemble 위한 필드
  - 네트워크 링크는 MTU(Max Transfer Unit)을 가지며 이를 초과하는 패킷의 데이터는 fragmentation된다

  - __16bit identifier__: data가 쪼개지기에 같은 packet임을 확인하기 위한 id
  - __flags__: 해당 패킷 뒤에도 쪼개진 데이터가 존재하는지(0 or 1)
  - __offset__: 패킷의 reassemble을 위한 위치정보
  
- __TTL__: 패킷의 수명으로 라우터 하나를 거칠때마다 감소
- __upper layer__: 상위계층 프로토콜이 데이터에 포함되었는지를 명시(ex. ICMP: 1 /IGMP: 2 /TCP: 6 등)
- __hearder checksum__: 헤더의 오류 검출
- __32bit source, dest address__(IPv4)
- IP Header : 20 Bytes/ TCP : 20 Bytes. 총 40Bytes와 application layer의 overhead를 가진다
- 
## IP 주소
- 호스트와 라우터 내부의 network interface를 지칭하는 주소
- 그렇기에 하나의 디바이스에 여러 IP 주소 매핑 가능(특히 라우터)
![newtwork area](https://user-images.githubusercontent.com/38183218/55678406-27a7f180-5934-11e9-9880-98665e92a3a5.JPG)
- 라우터를 통해 네트워크와 네트워크가 연결되고 이 포워딩의 효율을 높이기 위해 네트워크의 주소(IP 주소)에 계층이 적용됨(hierarchical addressing)
- IP prefix(네트워크 아이디, 서브넷 아이디)를 이용한 계층화(scalability improved)
![forwadingT](https://user-images.githubusercontent.com/38183218/55678452-eebc4c80-5934-11e9-9b04-e47faeb99741.JPG)
- 과거엔 classful addressing 적용해 class A(8bit prefix)/ class B(16bit prefix)/ class C(24bit prefix) 로 나눔

### CIDR(Classless Inter-Domain Routing)
classful addressing으로 인한 비효율(유동적이지 못한 IP주소체계)을 제거

![cidr](https://user-images.githubusercontent.com/38183218/55678505-01835100-5936-11e9-90dc-fcda9dfdbad8.JPG)

## Subnet
![subnet](https://user-images.githubusercontent.com/38183218/55678518-40b1a200-5936-11e9-870f-9afc785e61d0.JPG)
- IP prefix를 공유하는 device interface
- 라우터의 포워딩을 거치지 않고 physically reach 가능한 집합

## NAT(Network Address Translation)
- IPv4로 IP 주소 공간 부족 문제
- WAN에서 쓰는 public address를 LAN에서 쓰는 임의의 local address로 변환
- local address는 not globally reachable(내부적으로만 유의미)
- local network 내의 디바이스는 port 번호로 구분

![nat](https://user-images.githubusercontent.com/38183218/55678642-16f97a80-5938-11e9-924b-243da1014a50.JPG)


- NAT 단점
  - 1) layering violation: 네트워크 레이어에서 segment를 열어보는(port번호 확인) 문제
  - 2) port 번호를 호스트 찾는데 쓰게됨

## Dynamic Host Configuration Protocol(DHCP)
- 호스트가 네트워크 접속시 자신의 IP 주소를 동적으로 할당받는 방식
- IP주소의 임대, 갱신, 반환 과정
- DHCP 서버가 IP 주소를 보유하고 호스트에 분배하는 역할을 담당
- 규모작은 네트워크에서는 공유기에 탑재된 DHCP 서버를 이용
- 임대 과정
  - ![dhcp](https://user-images.githubusercontent.com/38183218/55678845-6e4d1a00-593b-11e9-94a5-a7ba52285c49.JPG)
  - __DHCP discover__(아무것도 모르니까 다들 도와주세요!): 해당 호스트는 로컬 네트워크 전체에 discover 패킷을 broadcast한다
  - __DHCP offer__(내가 도와줄게): discover 패킷을 받은 DHCP 서버가 응답한다
  - __DHCP request__(그래 너가도와주라~): 응답받은 호스트는 IP 주소를 요청한다. 역시 broadcast이다
  - __DHCP ACK__(ㅇㅋ~): DHCP 서버가 자신의 임대 풀에서 가능한 IP 주소를 찾아 할당하고 해당 주소를 요청 호스트에 날려준다


## 라우팅 알고리즘
특정 알고리즘을 선택해 어떤 네트워크 내의 라우팅 방식을 결정할 수 있다

### Autonomous Systems(AS)
- 자치권이 있는 라우팅 도메인(ex.기관, 학교 등)
- ASN(AS Number) 부여됨(6만개 이상)
- 자체적으로 라우팅 알고리즘을 적용

### Intra-AS 알고리즘
- link state
  - complete topology(모든 라우터와 링크 코스트)를 알고있는 상황
  - 다익스트라 알고리즘 이용
  
- distance vector
  - physically connected 이웃한 라우터와의 링크 코스트만 아는 상황
  - Bellman-Ford equation(DP) 이용
    - "gool news travels fast while bad news travels slow"
    - posioned reverse

### Inter-AS 알고리즘
- 알고리즘 결정 주체가 애매함
- Hierarchical routing
  - provider-customer, peering relationship
- BGP(Border Gatewoat Protocol)
  - Policy-Based routing protocol
  - customer > peer > provider의 preference