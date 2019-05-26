#### 19.03.15
#### 19.05.26

## 개요
AWS EC2 우분투 환경에 Nginx를 웹 서버로, uWSGI를 미들웨어로 쓰는 flask app을 배포해보자

## Keywords

*AWS EC2, SSH, uWSGI, Nginx, nohup*

## AWS EC2 인스턴스 생성
1. AMI는 Ubuntu 16.04 LTS
2. type은 12micro
3. free-tier는 storage 30gb까지 제공한다.
4. Tag
   - 인스턴스의 역할과 관리자 등을 입력 가능
5. Security Group
   - 내부 방화벽과 비슷한 보안 관련 설정
   - 현재 설정된 값이 없으므로 '새 보안 그룹 생성' 선택
   - 차례로 해당 security group의 이름과 설명을 입력하면 된다
   - 아래의 테이블이 직접적인 네트워크 접근에 대한 설정을 하는 부분
   - 인스턴스를 웹서버로 이용하기 위해서 SSH를 허용하고, HTTP에 대해서 80번 포트를 열어주자
   - 또한 flask app을 위해 5000번 포트를 열어준다
   - Source는 모든 타입의 외부 접근을 허용하는 '위치 무관'을 사용

6. 키 페어 선택 또는 생성
  - EC2 인스턴스 접속을 위한 키 쌍
  - '새 키 페어 생성'에서 임의로 이름을 주고 키 페어를 다운로드 받는다

### EC2 인스턴스에 연결(ubuntu, SSH)
- 인스턴스에 접속할 사용자 이름을 찾아놓자. `ubuntu`이다
- 인스턴스를 생성할때 받은 pem 파일의 경로를 찾는다(`/path/my-key-pair.pem`)
- `chmod 400 /path/my-key-pair.pem` 로 사용자만 접근가능하게한다
- `ssh -i "/path/my-key-pair.pem" ubuntu@<EC2_public_ip>`로 접속한다

## 서버 환경 설정

파이썬 설치 > 가상환경 설정 > uwsgi 설정 > nginx 설정 순으로 단계별로 진행

-----------------------------------------
## 파이썬 설치 및 프로젝트 설정

  [리눅스 버전 확인]
  
  `$ cat /etc/issue`

-----------------------------------------
  [파이썬 설치]

  `$ sudo apt-get update`

  `$ sudo apt-get upgrade`

  `$ sudo apt-get install python3-pip python3-dev nginx`

-----------------------------------------
  [가상환경 설정을 위한 virtualenv 설치]
  
  `$ sudo pip3 install virtualenv`

-----------------------------------------
  [프로젝트 clone 및 가상환경 생성]
  
  `$ git clone https://github.com/chankoo/exhibition-search-web-dev.git`
  
  `$ cd exhibition-search-web-dev`
  
  `$ sudo apt-get install openjdk-8-jdk` # jpype 설치를 위한 jdk install

  `$ rm -r venv` # 기존 가상환경 삭제

  `$ virtualenv venv`

  `$ source venv/bin/activate`
  
  `(venv)$ pip install -r requirements.txt` # dependencies 설치

-----------------------------------------
  [파일(db) 업로드]

  새로운 터미널을 열고

  업로드 하고 싶은 파일이 있는 경로에서 아래의 명령을 수행한다

  $ scp -i ["pem파일 경로"] [업로드할 파일 이름] [ec2-user 계정명]@[ec2 instance의 public DNS]:~/[경로 ]

  `$ scp -i "exhb-ubuntu.pem"  ExhbnRec.db ubuntu@ec2-13-124-211-10.ap-northeast-2.compute.amazonaws.com:~/exhibition-search-web-dev`

----------------------------------------
  [웹 서버 확인]

> ![스크린샷, 2019-03-15 01-40-35](https://user-images.githubusercontent.com/38183218/54374899-6a730280-46c3-11e9-852b-60dd503e24a6.png)
>
> 현재 nginx 웹 서버가 돌고 있다
> 
> nginx와 flask app을 잇는 미들웨어인 uWSGI를 이용해 flask app을 서비스해보자
>
> [WSGI]: Flask Is Not Your Production Server](https://vsupalov.com/flask-web-server-in-production/) 

--------------------------------------------
### uWSGI 설치 및 설정

[uwsgi 설치]

  `pip install uwsgi`

--------------------------------------------
  [5000번 포트 열고, 실행하기]

  `sudo ufw allow 5000`
  
  `python app.py`

--------------------------------------------
  [WSGI Entry Point 생성하기]

  `nano ~/exhibition-search-web-dev/wsgi.py`

  [wsgi.py 스크립트]

  `from app import app`

  `if __name__=="__main__":`
      `app.run()`

  [설정 테스트]

  `uwsgi --socket 0.0.0.0:5000 --protocol=http -w wsgi:app`

  entry 포인트인 wsgi에 app이 잘 실행되는지 테스트한다


  http://13.124.211.10:5000/ 으로 접속하면 실행이 잘 된다
  
  ![gogole](https://user-images.githubusercontent.com/38183218/54380297-73b59c80-46ce-11e9-8590-8dcf26940bb4.png)

  nohup을 이용해 백그라운드에서 실행하고자하면 아래 명령을 이용한다

  `nohup uwsgi --socket 0.0.0.0:5000 --protocol=http -w wsgi:app&`

  본격적으로 uWSGI 설정파일을 생성해보자

--------------------------------------------
  [uWSGI 설정파일 생성]

  deactivate 한 뒤 .ini 파일을 만들어주자

  `vim ~/exhibition-search-web-dev/app.ini`
  
  [app.ini 스크립트]

  ```sh
  [uwsgi] # uwsgi 헤더로 시작

  http = :5000 # uWSGI가 직접 5000번 통신을 한다. Nginx를 이용할때는 웹 요청을 Nginx가 담당하기에 .ini 파일에서 http 파라미터를 지우자

  module = wsgi:app # wsgi.py 파일을 레퍼런스하여 app을 파일내에서 호출할 수 있다

  #다음으로 실행시 마스터 노드에서 시작하고 1개의  work process를 만들게 한다

  master=true
  processes=1

  # 다음은 UNIX socket 파일의 위치이다. socket file의 위치를 잡아줘도 되며 localhost와 port를 명시해줘도 된다

  socket = app.sock

  chmod-socket = 660 # 소켓에 대한 permission을 바꿔줘야한다 


  vacuum= true # uWSGI를 통해서 생성된 파일들은 삭제하는 옵션

  daemonize=/path/uwsgi.log # 백그라운드로 돌리기 위한 설정이며 log파일을 남길 경로를 지정

  die-on-term=true # This can help ensure that the init system and uWSGI have the same assumptions about what each process signal means. Setting this aligns the two system components, implementing the expected behavior:

  
  # pidfile=/tmp/linku_backend.pid # process pid

  # newrelic settings

  # enable-threads = true # thread 사용을 앱(uWSGI) 내에서 가능하게 해줍니다.

  # single-interpreter = true # 단일한 python interpreter를 사용하게 하는 옵션입니다.

  # lazy-apps = true # master말고 각각의 worker에(master에서 spawn한 자식들) 앱을 로드하는 설정입니다.

  ```
  

--------------------------------------------
  [systemd Unit file 생성]

  systemd Unit File을 생성해주면 server 부팅 할 때마다 자동으로 uWSGI와 Flaskapp이 실행된다

  `sudo nano /etc/systemd/system/app.service`

  ```sh
  [Unit]

  Description=uWSGI instance to serve app

  After=network.target

  [Service]

  User=ubuntu

  Group=www-data

  # <project_path> = /home/ubuntu/react-flask-todo/backend

  WorkingDirectory=/<project_path>

  Environment="PATH=/<project_path>/venv/bin"

  ExecStart=/<project_path>/venv/bin/uwsgi --ini app.ini

  [Install]

  WantedBy=multi-user.target
  ```

--------------------------------------------
  [실행 및 활성화]
  
  `sudo systemctl start app`
  
  `sudo systemctl enable app`


  ------------------------------------------
  ### [socket을 이용한 uWSGI와 Nginx 연결]
  uwsgi 만으로도 서비스는 동작한다. 다만 nginx를 이용하면 1) 보안기능을 확보, 2) static 리소스(css, js, 이미지 파일들)접근이 안정적, 3) 여러 다양한 기능(gzip 등)을 이용할 수 있다(reverse proxy 를 사용한다고 한다)

  uWSGI와 Nginx를 연결하기 위해 Unix socket을 이용한다. 포트보다 socket을 이용하는 것이 더 안전하고 오버헤드가 적다

  [Proxy Requests에 Nginx를 설정]

  `nano /etc/nginx/sites-available/default`

  ```sh
    server {
          listen  80;
          server_name <ip or domain>;

          location /myproject {
                  include uwsgi_params;
                  uwsgi_pass unix:/home/user/myproject/myproject.sock;
          }
    }
  ```

  [링크 설정]

  가상 호스트 설정 파일의 경우 기본적으로 `sites-available` 디렉토리에 위치하며 `sites-enabled` 디렉토리에 심볼릭 링크를 걸어줌으로써 nginx에서 사용하게된다

  `sudo ln -s /etc/nginx/sites-available/app /etc/nginx/sites-enabled`

  [설정한 파일에 대한 문법 체크]

  `sudo nginx -t`

  [웹서버 Nginx 재시작]

  `sudo systemctl restart nginx`

  [사용하지 않는 5000포트 닫고, Nginx server접속허용하기]

  `sudo ufw delete allow 5000`

  `sudo ufw allow 'Nginx Full'`

  [코드 수정 후 재시작]

  `sudo systemctl restart app`



### References

- [How To Serve Flask Applications with uWSGI and Nginx on Ubuntu 16.04](https://www.digitalocean.com/community/tutorials/how-to-serve-flask-applications-with-uwsgi-and-nginx-on-ubuntu-16-04)

- [uWSGI를 이용하여 Nginx에 Flask 연결하기](https://cjh5414.github.io/flask-uwsgi-nginx/)

- [Nginx의 개요](https://whatisthenext.tistory.com/123)