#### 19.03.24

## keywords
segment, rdt, flow control, congetion management, congetion control

## TCP
- 프로세스와 프로세스(엄밀히는 소켓과 소켓)를 잇는 논리적인 개념의 __point-to-point__ 통신
- __reliable__: 응용계층에서 받은 데이터의 error와 loss가 없음을 보장
- __in-order byte stream__: 데이터의 순서를 유지
- __Pipelined__ : window size 단위로 패킷을 묶어 통신
- __send & receive buffers__: 송신 측과 수신 측 모두 send, receive buffer를 각각 가짐
- __Full duplex__: 통신하는 host 양쪽 모두가 데이터 전송 가능
- __Connection oriented__: handshake를 먼저 수행한 후 통신. handshake에서 window size, buffer, seq number와 같은 정보를 교환함
- __Flow control__ 
- __Congetion control__
## TCP의 특징
### segment structure
응용계층에서 내려온 data(Message)에 header를 붙인 __Segment__ 가 통신의 단위가 된다

![seg](https://user-images.githubusercontent.com/38183218/54878073-9da75580-4e6a-11e9-8761-1740196a3030.JPG)

- sequence number : 전송할 세그먼트의 seq #로 byte stream number이다
- acknowledgement number : (ack # -1) 까지의 패킷을 받았다는 응답으로 역시 byte stream number를 사용
- head length : 헤더의 길이
- control flags(U, A, P, R, S, F) : 패킷의 목적에 대한 정보를 각각 1 bit로 나타냄
   - URG : 급한 메시지를 표시. 지금은 사용하지 않음
   - ACK : 요청에 대한 응답임을 표시
   - PSH : 지금은 사용하지 않음
   - RST : 접속을 리셋, seq, ack # 을 리셋함
   - SYN : 접속을 요청
   - FIN : 접속을 종료
- receive window : receiver측의 버퍼 크기를 전송
- checksum : 메세지의 무결성을 확인
- option : 헤더에 추가적인 정보를 넣을 때 사용

### reliable data transfer
- TCP는 unreliable한 네트워크 계층(IP) 위에서 rdt를 제공
- TCP rdt의 특징
  - Pipelined segments
  - in-order byte stream
    - sequence number는 first byte in segment's data를 의미
  - cumulative ACKs
    - GBN의 경우와 약간 다름
      - TCP의 ack 10: seq# 10인 패킷부터 받을 차례임을 의미
      - GBN의 akc 10: seq# 10인 패킷까지 받았음을 의미
  - single retransmission timer
    - -> SR 아니라 GBN의 특성

  
- ![retry1](https://user-images.githubusercontent.com/38183218/54878075-9ed88280-4e6a-11e9-9984-11103d8979f2.JPG)
- ![retry2](https://user-images.githubusercontent.com/38183218/54878076-9f711900-4e6a-11e9-8fa3-1d07d52d3b85.JPG)


> __fast retransmission__
> 
> 동일한 seq#의 ack가 계속 응답된다면 해당 seq#인 패킷이 유실됐음을 인지 가능
> 
> timeout이라는 절대적인 기능이 있지만
> 
> timeout period가 길어 오버헤드가 커지는 경우에 유용
> 
> 보통 3번의 duplicated ack가 기준
> 
> -> 결국 동일한 ack를 4번 받으면 유실됐다고 판단하고 해당 패킷 재전송


### flow control
- recieve buffer의 가용 공간을 고려하여 데이터를 전송하는 단순한 로직

- segment 헤더의 Receive window에 가용 공간의 크기를 담아 전송한다

- 가용공간이 없다는 ack를 받을 경우 sender는 data가 없는 segment를 보내서 가용공간의 유무를 주기적으로 확인해야한다


### connection management
- __TCP 3-way handshake__
  - ![3wayH](https://user-images.githubusercontent.com/38183218/54879397-e74c6c00-4e7b-11e9-9373-3b8e342893ff.JPG)
  - 1) sender가 syn bit 1인 SYN msg를 보내며 initiate. 동시에 초기 seq#을 설정. data는 없거나 쓰레기값
  - 2) receiver가 SYN을 수신하면 SYNACK을 전송하며 receiver의 seq#을 설정
  - 3) 마지막으로 sender의 SYNACK으로 connection이 established 되며 각각의 호스트에 send buffer와 receive buffer가 생성된다
  - 마지막 SYNACK에는 데이터 포함 가능. 즉, 실제로 HTTP request를 보내는 역할을 한다
  
- TCP close: time wait 발생에 유의
  - ![tcpclose](https://user-images.githubusercontent.com/38183218/54879398-e74c6c00-4e7b-11e9-8cb4-315b7aa1c023.JPG)

## TCP congetion control
- TCP의 문제
  - 네트워크에 과부하가 걸려 congestion이 일어날 경우, TCP 프로토콜 하에서는 retransmission 통해 데이터 양을 늘리는 악순환이 일어남
  - 네트워크에 쏟아붓는 데이터 양을 조절하는 congetion control이 필수적
- End-to-End congetion control
  - 네트워크의 직접적인 피드백(network-assisted)이 없더라도 TCP sender(end)와 receiver(end)가 보내는 정보만으로 유추해서 control해야함
- 3 main phases
  - ![3phases](https://user-images.githubusercontent.com/38183218/54879774-3b594f80-4e80-11e9-96b3-23beed1cbef4.JPG)
  - 1) Slow start: 작은 window size로 시작해서 exponentially increase
  - 2) Additive increase: threshold에 다다르면 linear한 increase로 slow down
  - 3) Multiplicative decrease: congetion으로 인한 패킷 loss가 발생한다면 현재의 0.5 수준으로 window size을 확 줄임

- Refinement: TCP Reno
  - congetion의 2가지 case를 구분해 대응
  - ![Reno](https://user-images.githubusercontent.com/38183218/54879882-e585a700-4e81-11e9-9042-cff22c600ee1.JPG)

- TCP Fairness
  - 동일한 bottleneck(bandwidth R)을 이용하는 K개의 TCP 세션들은 접근 순서에 상관 없이 평균적으로 R/K 의 자원을 이용하게 된다
  - ![fairness](https://user-images.githubusercontent.com/38183218/54879973-19ad9780-4e83-11e9-9784-5ac13cda6260.JPG)