#### 19.05.12

## 폼(form)이란
일반적으로 HTML form tag를 의미한다. 이는 웹에서 사용자 입력을 서버로 전송하는 데 이용된다.

기본적인 동작은 아래와 같다

![스크린샷, 2019-05-12 16-44-56](https://user-images.githubusercontent.com/38183218/57579333-73226200-74d5-11e9-89e5-11eec73a81a6.png)

1. HTML form에 내용을 입력
2. form 안의 데이터를 서버로 전송
3. 서버는 웹 프로그램으로 데이터를 넘겨줌
4. 웹 프로그램이 form 데이터를 처리
5. 처리 결과에 따른 새로운 html 페이지를 서버에 전송
6. 서버 역시 브라우저에 html 페이지를 전송
7. 브라우저의 rendering

## 폼 태그 속성

- action : 폼을 전송할 서버 쪽 스크립트 파일을 지정한다

- name : 폼을 식별하기 위한 이름을 지정한다

- accept-charset : 폼 전송에 사용할 문자 인코딩을 지정한다

- target : action에서 지정한 스크립트 파일을 현재 창이 아닌 다른 위치에 열도록 한다

- method : 폼을 서버에 전송할 http 메소드를 정한다 (GET or POST)
    - GET: 폼 데이터를 URL 끝에 붙여서 쿼리스트링으로 보낸다. 보안이 필요하지 않으면서 자원을 읽을 경우에 사용한다

    - POST: 내부적(body 안에)으로 숨겨서 폼 데이터를 보낸다. 데이터를 처리하는 경우인 create, post, delete에 사용한다


### Reference

[HTML: 폼(form) 이해](http://www.nextree.co.kr/p8428/)

