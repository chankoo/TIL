#### 19.03.15

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

## EC2 인스턴스에 연결(ubuntu, SSH)
- 인스턴스에 접속할 사용자 이름을 찾아놓자. `ubuntu`이다
- 인스턴스를 생성할때 받은 pem 파일의 경로를 찾는다(`/path/my-key-pair.pem`)
- `chmod 400 /path/exhb-ubuntu.pem` 로 사용자만 접근가능하게한다
- `ssh -i "exhb-ubuntu.pem" ubuntu@ec2-13-124-211-10.ap-northeast-2.compute.amazonaws.com`로 접속한다

## 서버 환경 설정

[참고: How To Serve Flask Applications with uWSGI and Nginx on Ubuntu 16.04](https://www.digitalocean.com/community/tutorials/how-to-serve-flask-applications-with-uwsgi-and-nginx-on-ubuntu-16-04)

파이썬 설치 > 가상환경 설정 > uwsgi 설정 > nginx 설정 순으로 단계별로 진행

-----------------------------------------
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
  [uwsgi 설치]

  `(venv) $ pip install uwsgi`

--------------------------------------------
  [5000번 포트 열고, 실행하기]

  `(venv) $ sudo ufw allow 5000`
  
  `(venv) $ python app.py`

--------------------------------------------
  [WSGI Entry Point 생성하기]

  `(venv) $ nano ~/exhibition-search-web-dev/wsgi.py`

  [wsgi.py 스크립트]

  `from app import app`

  `if __name__=="__main__":`
      `app.run()`

  [설정 테스트]

  `(venv) $ uwsgi --socket 0.0.0.0:5000 --protocol=http -w wsgi:app`

  entry 포인트인 wsgi에 app이 잘 실행되는지 테스트한다


  http://13.124.211.10:5000/ 으로 접속하면 실행이 잘 된다
  
  ![gogole](https://user-images.githubusercontent.com/38183218/54380297-73b59c80-46ce-11e9-8590-8dcf26940bb4.png)

  nohup을 이용해 백그라운드에서 실행하고자하면 아래 명령을 이용한다

  `nohup uwsgi --socket 0.0.0.0:5000 --protocol=http -w wsgi:app&`

  본격적으로 uWSGI 설정파일을 생성해보자

--------------------------------------------
  [uWSGI 설정파일 생성] (아래부터 적용안된 상태)

  deactivate 한 뒤

  `$ vim ~/exhibition-search-web-dev/app.ini`
  
  [app.ini 스크립트]

  `[uwsgi]` // uwsgi 헤더로 시작

  `module = wsgi:app` // wsgi.py 파일을 레퍼런스하여 app을 파일내에서 호출할 수 있다

  //다음으로 실행시 마스터 노드에서 시작하고 1개의  work process를 만들게 한다

  `master=true`

  `processes=1`

  //When we were testing, we exposed uWSGI on a network port. However, we're going to be using Nginx to handle actual client connections, which will then pass requests to uWSGI. Since these components are operating on the same computer, a Unix socket is preferred because it is more secure and faster. We'll call the socket `app.sock` and place it in this directory.

  `socket = app.sock`

  // 소켓에 대한 permission을 바꿔줘야한다  We'll be giving the Nginx group ownership of the uWSGI process later on, so we need to make sure the group owner of the socket can read information from it and write to it. We will also clean up the socket when the process stops by adding the "vacuum" option:

  `chmod-socket = 660`

  `vacuum= true`

  `die-on-term=true` //This can help ensure that the init system and uWSGI have the same assumptions about what each process signal means. Setting this aligns the two system components, implementing the expected behavior:


--------------------------------------------
  [systemd Unit file 생성]

  systemd Unit File을 생성해주면 server 부팅 할 때마다 자동으로 uWSGI와 Flaskapp이 실행된다

  `$ sudo nano /etc/systemd/system/app.service`

  [스크립트]

  `[Unit]`

  `Description=uWSGI instance to serve app`

  `After=network.target`

  `[Service]`

  `User=ubuntu`

  `Group=www-data`

  `WorkingDirectory=/home/ubuntu/exhibition-search-web-dev`

  `Environment="PATH=/home/ubuntu/exhibition-search-web-dev/venv/bin"`

  `ExecStart=/home/ubuntu/exhibition-search-web-dev/venv/bin/uwsgi --ini app.ini`

  `[Install]`

  `WantedBy=multi-user.target`

--------------------------------------------
  [실행 및 활성화]
  
  `$ sudo systemctl start app`
  
  `$ sudo systemctl enable app`

  [Proxy Requests에 Nginx를 설정]

  `sudo nano /etc/nginx/sites-available/app`

  [링크 설정]

  가상 호스트 설정 파일의 경우 기본적으로 `sites-available` 디렉토리에 위치하며 `sites-enabled` 디렉토리에 심볼릭 링크를 걸어줌으로써 nginx에서 사용하게된다

  `$ sudo ln -s /etc/nginx/sites-available/app /etc/nginx/sites-enabled`

  [설정한 파일에 대한 문법 체크]

  `$ sudo nginx -t`

  [웹서버 Nginx 재시작]

  `$ sudo systemctl restart nginx`

  [사용하지 않는 5000포트 닫고, Nginx server접속허용하기]

  `$ sudo ufw delete allow 5000`

  `$ sudo ufw allow 'Nginx Full'`

  [코드 수정 후 재시작]

  `$ sudo systemctl restart app`



## ~~파이참에서 Deploy~~
- Settings -> Build, Execution, Deployment -> Deployment 선택
- '+' 눌러 서버를 추가하자
> ![deploy1](https://user-images.githubusercontent.com/38183218/54301323-1227fc00-4602-11e9-92a5-b3c770dcf8b3.PNG)
- EC2의 경우 SSH Key를 이용해 암호화된 연결하기에 SFTP 타입을 선택
- Connection Tab
  > ![dep1 5](https://user-images.githubusercontent.com/38183218/54301326-12c09280-4602-11e9-9ed8-bf2c2074c787.PNG)

  - SFTP host: DNS나 IP 입력
  - Root Path: 접속시 루트 디렉토리 위치
  - User name: 접속시 사용자 이름(Ubuntu AMI 의 경우 __ubuntu__)
  - Auth type: Key pair 선택하고 다운 받은 pem 파일을 선택
- Mappings Tab
  - ![dep2](https://user-images.githubusercontent.com/38183218/54301324-1227fc00-4602-11e9-9aaa-35fcf5582455.PNG)
  - 동기화할 프로젝트 경로를 지정
  - 두번째 항목인 'Deployment path on server'에 배포할 위치를 적어주자(`/` 인 경우 자동으로 `/home/ubuntu`를 잡아준다)
- 마지막으로 Tools -> Deployment -> Browse Remote Host를 선택하면 우측에 원격 서버의 디렉토리 정보가 나타나게 된다