#### 19.05.13

## OAuth 2.0

![slack_oauth_flow_diagram@2x](https://user-images.githubusercontent.com/38183218/57584648-c4a01080-7518-11e9-8711-bede1c43ad4c.png)

유저(__Resource Owner__) 우리의 서비스(__Client__), 그리고 그들의 서비스(__Resource Server__ & Authorization Server) 간 상호작용이다

## 진행과정 요약

1. 우리가 유저에게 허락을 구하고 그들에게 우리의 신원을 증명함

2. 그들이 AccessToken을 발급하여 우리에게 전달함

3. 우리가 AccessToken을 이용하여 유저에 대한 그들의 기능(API)을 제한적으로 사용

## AccessToken 발급 과정

### 등록

client가 Resource Server의 허락(Credentials)을 받는 것이다

![스크린샷, 2019-05-13 01-00-48](https://user-images.githubusercontent.com/38183218/57584813-9c191600-751a-11e9-8dba-9a2e4659fc09.png)

서비스마다 등록방법 다르지만 공통적으로 `client ID`(app 식별자) )와 `client secret`(app 보안), `Authorized Redirect URI`(Authorized code를 해당 주소로 전달)을 발급받는다

### Resource Owner의 승인

client가 ResourceSever의 B,C 기능을 사용하기 위해 ResourceOwner의 승인을 받는 과정이다. 앞서 등록 과정을 거쳤기에 ResourceSever는 client를 인식한 상황이다

--------------------------
![스크린샷, 2019-05-13 01-09-20](https://user-images.githubusercontent.com/38183218/57584908-c61f0800-751b-11e9-9d1f-426a1cac16e8.png)

1. ResourceOwner가 client에 접속하면 우선 client는 ResourceOwner에게 ResourceSever 로의 로그인을 요구한다 

2. 로그인에 ResourceOwner가 동의한다

--------------------
![스크린샷, 2019-05-13 01-11-58](https://user-images.githubusercontent.com/38183218/57584939-388fe800-751c-11e9-8524-55e7ed15d5b0.png)

3. ResourceOwner가 ResourceSever에 접속하여 로그인한다

4. ResourceSever는 client의 정보와 대조하여 redirect_url이 일치하는지 확인한다

5. ResourceSever는 ResourceOwner에게 "지금 client 가 scope B, C 에 해당하는 작업을 요구하고 있다"고 알려주고 컨펌 받으면 user id와 scope 정보를 서버에 저장한다


### Resource Sever의 승인

![스크린샷, 2019-05-13 01-30-26](https://user-images.githubusercontent.com/38183218/57585134-e8feeb80-751e-11e9-84cc-2773a3016812.png)

6. ResourceSever 는 Authorization code를 ResourceOwner에게 전달하며 Authorization code에 해당하는 주소로 ResourceOwner를 redirect 시킨다

------------------------------------------
![스크린샷, 2019-05-13 01-31-26](https://user-images.githubusercontent.com/38183218/57585133-e69c9180-751e-11e9-8f68-fd082963aa36.png)

7. redirection 시 client에 Authorization code가 저장된다

-----------------------------
![스크린샷, 2019-05-13 01-37-12](https://user-images.githubusercontent.com/38183218/57585198-b0abdd00-751f-11e9-975a-bc99a14ffbc7.png)

8. client는 ResourceSever에 가진 정보를 전송해 신원을 증명하고 ResourceSever의 승인을 얻는다

----------------------------------
### Access Token의 발급

![스크린샷, 2019-05-13 01-44-12](https://user-images.githubusercontent.com/38183218/57585261-a5a57c80-7520-11e9-8fbb-de8f76d1dc3a.png)

9. ResourceSever가 해당 ResourceOwner에 대한 Access Token을 client에 발급

## API 호출
![스크린샷, 2019-05-13 01-50-09](https://user-images.githubusercontent.com/38183218/57585314-7c392080-7521-11e9-8bc3-c3b3e502b316.png)

google의 경우 1) 헤더로 AccessToken을 넘기거나 2) 쿼리 스트링으로 AccessToken을 넘기는 두가지 호출 방법을 제공한다

## Refresh token
토큰의 수명이 끝난 경우 AccessToken을 다시 발급 받아야하는데 이전의 복잡한 과정을 다시 거치지 않고도 재발급을 가능케하는 방법

AccessToken 발급시에 Refresh token 역시 발급되는 경우 많다

Invalid Token Error 발생시 client는 보관하고있던 Refresh token을 ResourceSever에 전달하며 AccessToken을 다시 발급 받는다


### Reference
- [생활코딩: WEB2 - OAuth 2.0](https://opentutorials.org/course/3405)


