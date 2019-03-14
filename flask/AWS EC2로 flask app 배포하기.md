#### 19.03.13

## Keywords

가상환경, pip freeze, git-lfs, AWS EC2


## 파이썬 가상환경

- 기존에 설치된 파이썬과 격리된 환경을 제공함으로써 기존 라이브러리의 의존성없이 개발 환경을 설정
- 종류
  - venv
  - virtualenv
  - conda
  - pyenv
- ~~파이참이 생성한 flask 프로젝트는 [venv](https://docs.python.org/ko/3/library/venv.html#module-venv) 모듈을 사용(?)~~


### 예제 프로젝트의 디렉토리 구조(윈도우)

- exhibition-search-web-dev
  - | -- app.py
  - | -- static
  - | -- templates
  - | -- venv
    - |-- Include
    - |-- Lib
    - |-- Scripts
  
- `venv\Scripts\activate.bat` 으로 가상환경을 실행한다
- Unix나 Mac에서는 `source venv/bin/activate`

> __pip-freeze__
> 
> 가상환경에서 pip freeze는 설치된 패키지의 목록을 만든다
> 일반적으로 `(venv) $pip freeze > requirements.txt` 로 파일에 입력한다


## git-lfs
- 우선 로컬에서 개발한 flask 프로젝트를 github에 업로드 해보자
- 해당 프로젝트의 경우 100MB 이상 파일을 github에 push해야 했다(파일 db). 이 경우 일반적인 방법으로는 push가 안된다
- commit 과정에서 지정한 파일을 작게 조각내는 git extension인 [git-lfs](https://git-lfs.github.com) 를 설치하자
- 적용하려는 repo 경로에서 `git lfs install`
- 그다음 용량이 큰 파일을 git-lfs의 관리 대상으로 등록해준다
- `git lfs track "*.db"`
- `git commit -m "Large file included"`
- 이제는 push 가능하다
- 그런데 기존에 100MB 이상의 파일을 Commit한 적이 있다면 여전히 100MB 이상의 파일을 올릴 수 없다는 경고 메시지를 보게 된다
- 그럴 땐 [BFG Repo-Cleaner](https://rtyley.github.io/bfg-repo-cleaner/) 를 이용해 기존 커밋에서 100mb보다 큰 파일의 로그를 강제로 없애주자
- 공식 사이트에서 bfq-x.x.x.jar를 받고, 대상이 되는 Repository에서 `java -jar bfg-x.x.x.jar --strip-blobs-bigger-than 100M` 명령 실행
- 그런 후에 push를 다시 시도 하면 성공한다


## AWS EC2 인스턴스 생성
1. AMI는 Ubuntu 16.04 LTS
2. type은 12micro
3. free-tier는 storage 30gb까지 제공한다 30gb로 설정
4. Tag
   - 인스턴스의 역할과 관리자 등을 입력 가능

5. Security Group
   - 내부 방화벽과 비슷한 보안 관련 설정
   - 현재 설정된 값이 없으므로 '새 보안 그룹 생성' 선택
   - 차례로 해당 security group의 이름과 설명을 입력하면 된다
   - 아래의 테이블이 직접적인 네트워크 접근에 대한 설정을 하는 부분
   - 인스턴스를 웹서버로 이용하기 위해서 SSH를 허용하고, HTTP에 대해서 80번 포트를 열어주자
   - Source는 모든 타입의 외부 접근을 허용하는 Anywhere를 사용

6. 키 페어 선택 또는 생성
  - EC2 인스턴스 접속을 위한 키 쌍
  - '새 키 페어 생성'에서 임의로 이름을 주고 키 페어를 다운로드 받는다




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