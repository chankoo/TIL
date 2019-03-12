#### 19.03.12

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

## 쿠키
- 쿠키를 이용해 사용자를 식별
- web storage

## proxy
- 클라이언트와 서버 사이에서 HTTP 메시지를 정리하는 __중개서버__
- __서버인 동시에 클라이언트__
  - 웹 캐시
  - 보안 방화벽
  - 문서 접근 제어
  - 콘텐츠 라우터
  - 사용자 요청 분산
  - ...