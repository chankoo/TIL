#### 19.03.12

## API(Application Programming Interface)

- 응용프로그램에서 사용할 수있도록 운영체제나 프로그래밍 언어가 제공하는 기능을 제어할 수 있게 만든 인터페이스

  - 전문지식을 갖지 않고도 해당 기능을 사용가능하게 해주는 일종의 연결통로
  - 자동차 브레이크가 그 동작 구조와 동작방식을 몰라도 브레이크를 밟아 차를 멈추는 기능을 제공하는 것과 같음

- 최근의 API는 웹 API와 같은 의미로 쓰이나 엄밀히 말하면 API 개념은 더 포괄적임(ex. 운영체제가 제공하는 API)

## API 디자인 패턴
### REST(Representational State Transfer)
- URL과 Method 통해 정보를 가져오는 기능이 핵심인 아키텍쳐
- 구성요소
  - 정보의 자원: URL
  - 행위: HTTP Method
  - 표현
- URI은 정보의 자원만을 표현해야 한다. 즉 행위는 method에만 표현되어야 한다
    - 즉, `GET /members/delete/1` 이 아니라
    - `DELETE /members/1`이 맞는 표현

![rest](https://user-images.githubusercontent.com/38183218/54192780-179b1e80-44fc-11e9-8ac0-8b0f234ce801.PNG)

<br>

![method](https://user-images.githubusercontent.com/38183218/54192782-179b1e80-44fc-11e9-9856-550487668034.PNG)

> [더 알아보기: REST API 설계시 주의할 점](https://meetup.toast.com/posts/92)


### SOAP
- HTTP, HTTPS, SMTP 등의 프로토콜 통해 XML 기반 메시지 주고받음
- REST에 비해 어렵고 무거움
- 무조건 XML 데이터 형식 사용
- 현재는 잘 안씀

### Graphql
- Facebook에서 만든 API 디자인 패턴
- REST를 사용하며 생긴 불편함을 해결하고자 등장
  - URL이 너무 많아지는 문제
  - 사용자별로 필요한 정보가 조금씩 다른 문제
    - Over-fetching (필요 이상의 데이터가 들어옴)
    - Under-fetching (필요한 정보 위해서 여러번 요청해야함)
- 하나 혹은 소수의 Endpoint(URL) 사용해 요청 데이터에 따라 응답결과를 다르게함


