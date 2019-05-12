#### 19.03.12
#### 19.05.12

## 웹의 구성
- HTML
- URL
- 웹 브라우저, 웹서버
- HTTP

## HTTP(Hyper Text Transfe Protocol)
- 웹에서 클라이언트와 서버가 컨텐츠(html, 이미지, 오디오, css, javascript 파일등)을 주고 받기 위해 사용하는 통신규칙
  - 클라이언트와 서버는 HTTP 메시지로 통신
- W3C 에서 표준화 작업을 계속 진행
- TCP/IP 기반의 프로토콜

## HTTP Header
### Request Header
<br>

![requestH](https://user-images.githubusercontent.com/38183218/54200222-386c6f80-450e-11e9-8e5d-57c4f0bb470e.png)

- request line: method/ 요청 정보/ 브라우저의 HTTP버젼
- Host: 웹 서버 주소: 포트
- Accept: 웹 브라우저가 처리가능한 데이터 형식, 좌측일수록 선호
- Accept-Encoding: 웹 브라우저가 지원하는 압축(인코딩) 방식
- User-Agent: 웹 브라우저를 의미
- If-Modified-Since: 웹 브라우저가 가지고 있는 최신의 파일 버전을 명시

### Response Header
<br>

![responseH](https://user-images.githubusercontent.com/38183218/54200223-386c6f80-450e-11e9-865e-1045bd9f302b.png)

- Status line: 브라우저의 HTTP 버전/ status code/ phrase
- Content-Encoding: 응답의 압축(인코딩) 방식
- Content-Length: 응답의 사이즈
- Content-Type: 응답의 타입

## HTTPS
- HTTPS 혹은 SSL
- HTTP를 암호화

## HTTP Cache
- 이미 다운로드한 파일을 저장해 웹 브라우징 속도를 높임
- cache-control pragma

## 쿠키와 세션

웹 환경의 정보를 클라이언트에 파일로 남기는 것을 __쿠키__, 서버에 별도로 저장하는 것을 __세션__ 이라고 한다.

__비연결성(Connectionless)__ 과 __비상태성(Stateless)__ 은 HTTP의 특징이다. 그렇기에 HTTP는 클라이언트와 서버 간 한번의 통신을 완료하면 연결이 끊기게 된다. 이 때 요청과 응답의 상태가 저장되지 않는 문제가 생긴다

![스크린샷, 2019-05-12 22-11-35](https://user-images.githubusercontent.com/38183218/57582714-fad29580-7502-11e9-9cc6-637fb68b15ac.png)

Connectionless, Stateless한 HTTP의 특성은 시스템 구축을 쉽게 만들고 통신의 비용을 절감하는 장점도 있지만, 인증 (authentication) 기능과 같이 클라이언트와 서버의 상태 값을 유지해야하는 경우가 있기에 쿠키와 세션을 이용한다

### 쿠키 vs 세션

![스크린샷, 2019-05-12 22-53-40](https://user-images.githubusercontent.com/38183218/57583173-d37ec700-7508-11e9-9bab-8db54cb957dd.png)


#### 쿠키의 특징

- 웹 환경의 데이터를 클라이언트의 특정 경로(PC)에 파일로 저장하는 방식

- 요청 때마다 쿠키 정보를 request Header에 함께 전송

- 주로 1) 세션관리, 2) 개인화, 3) 사용자 트래킹 에 이용

- 서버가 클라이언트의 상태를 내부적으로 저장하지 않기 때문에 서버의 부하가 적음

- 클라이언트의 저장장치에 파일로 저장되기 때문에 보안에 취약

#### 세션의 특징
- 인증된 클라이언트 정보를 서버에서 내부적으로 저장하고 관리하는 방식
- 서버의 자원이 남아있으면 제한없이 정보를 저장 가능
- 객체로 관리되기에 다양한 정보 저장 가능

- 세션 ID만 클라이언트에게 넘겨주기 때문에 쿠키보다 안전
- 쿠키를 저장수단 아니라 전달수단으로 사용 (즉, 세션 ID를 쿠키통해 전달)
- 세션 개수가 커지면 서버에 부담을 주고 세션의 종류에 따라 파기시점이 다르기에 주의 필요

#### 세션의 동작

![스크린샷, 2019-05-12 23-19-28](https://user-images.githubusercontent.com/38183218/57583554-6c631180-750c-11e9-9722-39034a32a421.png)

1) 클라이언트가 서버에게 Login을 요청한다. 이 때, 인증에 필요한 정보를 파라미터로 전달한다

2) 서버에서는 전달된 정보가 맞는지 확인 작업을 수행한다. 정보가 올바르다면 세션 객체를 생성하고 세션 ID를 Set-cookie 를 통하여 클라이언트에게 전달한다. 세션 객체는 서버에 저장되며 일반적으로 메모리나 Cache DB에 저장한다

3) 클라이언트가 서버에게 작업을 요청할 때 HTTP Request 헤더에 세션 ID를 같이 전달한다

4) 서버에서는 클라이언트의 세션정보에 세션 ID가 있다면, 세션 스토리지에서 세션 객체를 검색한다. 정보가 있으면 요청한 작업에 대해 응답해주고 통신이 종료된다

## proxy
- 클라이언트와 서버 사이에서 HTTP 메시지를 정리하는 __중개서버__
- __서버인 동시에 클라이언트__
  - 웹 캐시
  - 보안 방화벽
  - 문서 접근 제어
  - 콘텐츠 라우터
  - 사용자 요청 분산
  - ...


### Reference

- [개발자 노트: 세션과 쿠키에 대하여 알아보자](https://kim1124.tistory.com/1)

- [쿠키(Cookie)와 세션(Session) & 로그인 동작 방법](https://cjh5414.github.io/cookie-and-session/)