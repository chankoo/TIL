#### 19.03.25

## Flask-RESTful
- RESTful API 작성을 위한 프레임워크로 Flask를 확장해서 사용
- class based
- 이용하려는 자원을 클래스(ex. UserList)로 만들어 이용하고 HTTP method 이름의 메소드(ex.get, post, put, delete)를 정의

> [공식문서: User's Guide](https://flask-restful.readthedocs.io/en/latest/)

## RESTful API 작성 예제
유저정보(email과 pwd)가 있는 클래스 UserList를 자원으로 하는 api의 CRUD를 구현해보자

![스크린샷, 2019-03-26 00-05-26](https://user-images.githubusercontent.com/38183218/54930860-649ade00-4f5b-11e9-9d42-5d2cc56fc39e.png)

우선 main에 해당하는 `api.py`에 UserList 클래스를 import 하였다(실제 api 처리 로직을 userlist에 분리하였다)

[line 6] `api = Api(app)` 명령으로 flask app을 확장하여 이용한다

[line 8] `add_resource(UserList, 'api/users')` 메소드로 api 객체에 리소스인 UserList와 URI 정보를 전달한다

현재 flask app은 로컬 호스트 5000번 포트에서 돌고있다

![스크린샷, 2019-03-26 00-05-57](https://user-images.githubusercontent.com/38183218/54930861-65337480-4f5b-11e9-8ba6-f94dd15fc4f1.png)

Resource 클래스를 상속받은 UserList를 정의한다

HTTP 메소드의 이름과 같은 get, post, put, delete 메소드 구현이 핵심이다

get 메소드는 user 자원을 조회하고

`$ curl -X GET 0.0.0.0:5000/api/users` 로 request 한다

post 메소드는 user 자원을 생성하고

`curl -X POST -H "Content-Type: application/json" -d '{"email":"someone@gmail.com","password":"hello"}' 0.0.0.0:5000/api/users` 

형식으로 옵션을 통해 헤더와 데이터(json)을 전송한다

데이터는 user정보로 이메일과 비밀번호를 담고있다

비밀번호의 경우 평문 저장시 보안문제가 있으므로 `bcrypt`를 이용하여 hash된 형태로 저장한다

![스크린샷, 2019-03-26 00-08-35](https://user-images.githubusercontent.com/38183218/54930864-65337480-4f5b-11e9-9cc8-01f82f9a8a7b.png)

POST로 생성된 유저정보는 위와같이 json 파일로 저장된다

![스크린샷, 2019-03-26 00-06-28](https://user-images.githubusercontent.com/38183218/54930863-65337480-4f5b-11e9-8ee4-0f45e126fcdf.png)

put 메소드는 email을 key로 하여 비밀번호를 수정한다

`curl -X PUT -H "Content-Type: application/json" -d '{"email": "ckbaek1125@gmail.com", "password":"hello2"}'  0.0.0.0:5000/api/users` 형태로 request한다

역시 비밀번호는 hash하여 저장한다

마지막으로 delete 메소드는 email을 key로 하여 유저정보를 삭제한다

