#### 19.05.27

## 개요
docker-compose를 이용하여 nginx와 flask 컨테이너로 구성된 애플리케이션 빌드하기


## 서버 구성

![스크린샷, 2019-05-27 02-23-55](https://user-images.githubusercontent.com/38183218/58385134-86314800-8026-11e9-8a31-d499e297a17b.png)

`react-flask-todo` 라는 프로젝트 디렉토리 하위에 `backend` 서버가 존재하는데 여기에서 `flask`와 `nginx` 컨테이너를 연결할 것이다

### flask

```Dockerfile
# ./flask/Dockerfile

FROM python:3.7.2-stretch

WORKDIR /app

ADD . /app

RUN pip install -r requirements.txt

CMD ["uwsgi", "--ini", "uwsgi.ini"]

```
베이스 이미지로 `python:3.7.2-stretch`를 이용하며 하위의 app 디렉토리에서 앱을 구성한다

엔트리 포인트로 `wsgi.py`를 이용하며 `uwsgi.ini` 스크립트로 uwsgi 설정한다 


### nginx
```Dockerfile
# ./nginx/Dockerfile

FROM nginx

RUN rm /etc/nginx/conf.d/default.conf

COPY nginx.conf /etc/nginx/conf.d/
```

`nginx` 베이스 이미지를 이용하며 `nginx.conf`의 설정을 컨테이너에 copy하는 역할을 맡는다. flask 컨테이너의 `app.socket`에 연결한다


```sh
# ./nginx/nginx.conf

server {
    listen 80;
    server_name localhost;
    location / {
            include uwsgi_params;
            uwsgi_pass flask:5001; # 현재 flask 컨테이너가 5001번 통신 중
            # uwsgi_pass unix:/home/ubuntu/react-flask-todo/backend/flask/app/app.sock; # unix socket 연결하는 경우 이용
    }
}

```

### docker-compose.yml
```yml
# .backend/docker-compose.yml
version: "3.7"

services:
  flask:
    build: ./flask
    container_name: flask
    restart: always
    environment:
      - APP_NAME=MyFlaskApp
    expose:
      - 5001

  nginx:
    build: ./nginx
    container_name: nginx
    restart: always
    ports:
      - "80:80"
```



### docker-compose build
docker-compose를 이용하여 빌드해보자

flask 이미지와 nginx 이미지가 차례로 만들어진다

![스크린샷, 2019-05-27 02-01-54](https://user-images.githubusercontent.com/38183218/58385004-52a1ee00-8025-11e9-81ef-1a0da960bf27.png)

![스크린샷, 2019-05-27 02-13-02](https://user-images.githubusercontent.com/38183218/58385006-52a1ee00-8025-11e9-9d80-1562f325d654.png)

![스크린샷, 2019-05-27 02-03-40](https://user-images.githubusercontent.com/38183218/58385005-52a1ee00-8025-11e9-8fa1-ac5b4590c61d.png)





### References

- [Julian Nash: Building a Flask app with Docker](https://pythonise.com/feed/flask/building-a-flask-app-with-docker-compose)