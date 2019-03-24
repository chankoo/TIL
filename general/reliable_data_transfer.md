#### 19.03.24

## 목표
TCP를 이해하기 위해 신뢰성 있는 데이터 전송을 가능케 하는 기본 원리들을 알아보자

## keywords
전송계층, Stop and Wait, Pipelining, RDT, GBN, SR

## 신뢰성 있는 데이터 전송(Reliable Data Transfer)
__전송계층__ 에서 신뢰성 있는 데이터 전송(RDT)을 담당한다

여기서 말하는 신뢰성이란 __1) error__ 와 __2) loss__ 를 해결하는 것이다. error는 전송되는 데이터에 손상이 있는 경우를, loss는 목적지까지 데이터가 전달되지 않은 경우를 뜻한다

### Stop and Wait
- __RDT 1.0__: 하위 계층을 완전히 신뢰할 수 있을때 이용
  - 완전히 신뢰할 수 있기에 error와 loss가 없음을 보장
  - 그렇기에 아무 것도 안한다
  - sender는 패킷을 전송하고 receiver는 수신한다
  
- __RDT 2.0__: error가 있는 채널에서 문제를 신뢰를 보장
  - 패킷에 체크섬을 포함한 segment를 전송해 receiver가 데이터의 오류를 점검함
  - 이후 오류가 없으면 긍정응답 ACK를, 오류가 있으면 부정응답 NAK를 전송
  - NAK가 오면 sender는 버퍼에 저장해둔 이전의 segment를 재전송
  - ![rdt_2 0](https://user-images.githubusercontent.com/38183218/54876338-a50e3500-4e51-11e9-8c09-eb9cd304b89b.png)

- __RDT 2.1__: seq number의 삽입
  -  ACK/NAK 역시 패킷이기에 전송 도중 error 발생하면 중복된 패킷이 전송될 위험
  -  이를 해결하기 위해 패킷에 seq number(0 or 1)를 삽입
  -  seq number가 존재하므로 특정 패킷에 대한 ACK, NAK임을 구분하여 중복 전송을 막을 수 있다
  -  ![rdt2 1__1](https://user-images.githubusercontent.com/38183218/54876339-a50e3500-4e51-11e9-9786-e9be9ca78805.png)
  -  ![rdt2 1__2](https://user-images.githubusercontent.com/38183218/54876340-a50e3500-4e51-11e9-8ff2-5678f9cabf02.png)


- __RDT 2.2__: NAK 없는 RDT
  - RDT 2.1이 ACK와 NAK로 복잡하기에 NAK를 없애는 방식이다
  - 현재 패킷에 error가 있을 경우 이전 패킷의 seq number를 다시 보내어 현재 패킷을 재전송하라는 메시지를 전달
  - ![rdt2 2_1](https://user-images.githubusercontent.com/38183218/54876341-a7708f00-4e51-11e9-9089-c36d32daf9ae.png)
  - ![rdt2 2_2](https://user-images.githubusercontent.com/38183218/54876342-a7708f00-4e51-11e9-8a3e-41e034391939.png)

- __RDT 3.0__: error와 loss가 있는 채널에서도 신뢰성을 보장
  - 패킷 전송과 함께 sender는 timer를 켜놓고 응답을 기다림
  - timer 설정 시간이 지나도 응답이 없으면 패킷 loss라 판단해 버퍼에 저장된 이전 패킷을 재전송
  - ![rdt 3 0](https://user-images.githubusercontent.com/38183218/54876537-fe2b9800-4e54-11e9-8c03-b17828eb5ece.jpg)


### Pipelining
RDT 3.0 까지는 모두 Stop and Wait 방식이기에, 한 패킷을 보내고 그에 대한 응답이 올때 까지 기다리는 비효율이 발생한다. 

여러개의 패킷을 묶어 전송할 수 있으면 더 효율적일 것이다. 이때 1)seq number의 범위를 증가시키고 2) sender와 receiver 각각 최소 한 패킷 이상을 버퍼에 저장해야하는 조건을 지키면 pipelining이 가능하다. 여러 패킷이 동시에 전송되기에 각각 유일한 seq number를 가져야하기 때문이다

- __GBN__(Go-Back-N)
  - 전송되는 패킷 중 첫번째를 send base라 하고 send base에 대해서만 timeout을 지정한다
  - window size 만큼 전송하고, 응답까지 완료된 패킷의 다음 패킷이 send base가 되며 이를 sliding window라고 한다
  - 이때 누적 ACK를 사용하여 패킷에 대한 ACK를 보내는 것이 아니라 단순히 ACK의 개수를 count하여 전송 여부를 판단한다
  - timeout 발생시 send base부터 모두 재전송하므로 전송량이 많아지는 단점
  - ![gbn](https://user-images.githubusercontent.com/38183218/54876736-0df8ab80-4e58-11e9-8e96-dba3e50fd4c6.png)
    - 예제에서 보이듯 2번 패킷을 받지못하면 3,4,5번 패킷을 버리며 ACK1만을 응답한다
    - 때문에 send base는 여전히 2이며 sender가 3,4,5를 이미 성공적으로 전송했음에도 send base부터 다시 모두 재전송한다

- __SR__(Selective Repeat)
  - GBN과 달리 패킷마다 개별적인 ACK로 응답하고, 패킷 각각에 타이머를 적용한다
  - receiver 쪽에도 buffer를 만든다
  - ![sr](https://user-images.githubusercontent.com/38183218/54877020-97aa7800-4e5c-11e9-9671-0bf80986d4bd.png)
    - 2번 패킷의 timeout으로 인해 receiver가 순서에 안맞는 3~5번패킷을 받아도 우선 buffer에 저장하고 3~5번 ACK를 보낸다
    - 이후 2번 패킷을 재전송받게 되면 2번 ACK를 보내며 버퍼를 비우고(상위계층에 전달하고) 6번으로의 window sliding을 진행한다