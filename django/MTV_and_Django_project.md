#### 19.03.30

## MTV 패턴

![mvc](https://user-images.githubusercontent.com/38183218/55274059-1305a100-5317-11e9-8ae0-8e105e07b1e8.png)

웹 개발의 일반적인 형태인 MVC 패턴은 Model-View-Controller를 의미하며 데이터(model), 사용자 인터페이스(view), 데이터를 처리하는 로직(controller)를 분리하여 개발하는 방식이다. MVC는 각 요소들을 독립적으로 개발할 수 있기에 각 요소를 담당하는 UI 디자이너, 백엔드 개발자 등의 업무 효율을 높인다 

![mtv](https://user-images.githubusercontent.com/38183218/55274060-139e3780-5317-11e9-9089-531eabc6e49c.png)

장고는 MTV(Model-Template-View) 패턴을 이용하는데 MTV는 MVC와 유사한 로직이다. 다만 사용자 인터페이스 요소를 Template, 데이터 처리 로직을 View라 이름지엇기에 용어의 혼동을 조심하자

각 요소의 기능을 약술하자면 Model은 DB에 저장될 데이터의 포맷을 클래스 형태로 구현해 ORM을 통한 DB 관리를 가능하게 한다. Template은 클라이언트에게 보여지는 부분으로 디자인과 테마를 적용해 페이지를 렌더링한다. View는 실질적으로 프로그램이 동작하여 데이터를 가져와서 처리한 결과를 템플릿에 전달하는 역할을 수행한다. 동작 과정을 정리하자면

- 클라이언트의 요청(HTTP Request)이 있으면 URL conf 모듈을 이용해 URL을 분석한다
- URL을 처리할 뷰를 결정한다
- (DB의 데이터가 필요하다면) 해당 뷰는 모델을 통해 데이터를 넘겨받는다
- 뷰는 자신의 로직이 끝나면 템플릿을 이용하여 클라이언트에 전송할 HTML 파일을 생성한다
- 마지막으로 뷰는 HTML을 HTTP Response 객체로 클라이언트에게 보낸다

> ### URLconf
> 
> 장고의 URL 처리는 보통 `urls.py`에 URL과 URL 처리함수(View)의 매핑을 정의하는 방식으로 이뤄진다
> 
> ![urlconf](https://user-images.githubusercontent.com/38183218/55274449-d2f4ed00-531b-11e9-834b-f2f2ed355db7.png)
> 
> 이는 개발자에게 유연성을 제공하는데 URL 자체에 처리 함수나 처리 로직이 담긴 스크립트 파일 이름이 들어가면 변경하기 힘들기 때문이다. URLconf를 이용하면 매핑부분만 수정하면 되기에 간편하다
> 
> - `setting.py` 의 ROOT_URLCONF 항목을 읽어 `url.py`의 경로를 알아낸다
> - URLconf 모듈을 로딩하여 urlpatterns 변수에 지정된 URL 리스트를 검사한다
> - 매칭된 URL의 뷰를 호출한다. HttpRequest 객체와, 매칭시 추출된 단어들을 뷰의 인자로 넘긴다



## 프로젝트 뼈대 만들기

### project와 app의 구분
- 장고에서 프로젝트란 개발 대상이 되는 전체 프로그램을 의미
- 프로젝트 하위의 서브 프로그램을 애플리케이션이라 함
- 즉, app의 집합인 project
- 여러개의 서브 project들을 합쳐 큰 project를 만드는 계층적인 개발이 가능 

![스크린샷, 2019-03-30 18-53-15](https://user-images.githubusercontent.com/38183218/55274605-2451ac00-531d-11e9-963e-16ff63075fb5.png)

위는 django_test 프로젝트의 뼈대이다. `django_test`라는 최상위 디렉토리가 존재하고 이는 보통 `settings.py`의 BASE_DIR 항목으로 지정된다. 이 디렉토리에 db와 프로젝트 관리를 위한 `manage.py`가 보통 위치한다

그 아래 `django_test`는 프로젝트명으로 만들어진 디렉토리이다.
`django-admin startproject django_test` 명령으로 시작한다.

`myapp`과 `myapp2`는 프로젝트의 어플리케이션들이다.
각각 `django-admin startapp myapp`, `python manage.py startapp myapp2` 명령으로 생성되었다

### db
장고는 디폴트로 SQLite3 엔진을 이용한다. `python manage.py migrate` 명령으로 db변경사항을 반영해준다

