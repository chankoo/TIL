#### 19.03.13

## Keywords

가상환경, pip freeze, git-lfs

## 파이썬 가상환경

- 기존에 설치된 파이썬과 격리된 환경을 제공함으로써 기존 라이브러리의 의존성없이 개발 환경을 설정한다
- 종류
  - venv
  - virtualenv
  - conda
  - pyenv
- ~~파이참이 생성한 flask 프로젝트는 [venv](https://docs.python.org/ko/3/library/venv.html#module-venv) 모듈을 사용(?)~~


> ### 예제 프로젝트의 디렉토리 구조(ubuntu)

  - exhibition-search-web-dev
    - | -- app.py
    - | -- static
    - | -- templates
    - | -- venv
      - |-- include
      - |-- bin
      - |-- lib
  
- Unix나 Mac에서는 `source venv/bin/activate`
- 윈도우에서는 `venv\Scripts\activate.bat` 으로 가상환경을 실행한다


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
- 다만 git-lfs가 관리하는 .db 파일을 다시 database로 이용하려면 recovery가 필요해보임. 다시 확인 필요