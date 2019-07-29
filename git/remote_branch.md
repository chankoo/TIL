#### 19.07.29

## 개요
git을 이용해 협업하다보면 remote 브랜치를 내 로컬 저장소에 가져오는 경우가 있다. repo를 clone하는 것만으로는 브랜치들을 같이 가져올 수 없기 때문에 remote 브랜치를 관리해보자


### git remote update
- 우선 `git remote update` 명령으로 remote 저장소를 갱신해준다


### remote Refs
remote Refs는 원격 저장소에 있는 레퍼런스이다

![Screen Shot 2019-07-29 at 3 01 55 PM](https://user-images.githubusercontent.com/38183218/62025305-c1ace600-b212-11e9-9b72-da058b45e421.png)
- `git ls-remote [remote]` 명령으로 리모트 저장소에 저장된 모든 remote Refs를 조회 가능하다

![Screen Shot 2019-07-29 at 3 01 35 PM](https://user-images.githubusercontent.com/38183218/62025306-c1ace600-b212-11e9-8ff7-03586b4cf500.png)
- `git remote show [remote]` 명령으로 모든 리모트 브랜치와 정보를 확인 가능하다

### 리모트 트래킹 브랜치
![origin/master](https://git-scm.com/book/en/v2/images/remote-branches-1.png)

우리의 목표는 리모트 브랜치를 로컬에 가져오는 것이다. 위의 예제에서 가져오고자 하는 리모트 브랜치의 이름은 'master'이다. 이를 로컬에 가져오는 순간 원래의 리모트 브랜치를 트래킹하는 '리모트 트래킹 브랜치'가 'origin/master'라는 이름으로 생성된다. 반면 로컬 브랜치의 이름이 'master'가 되어서 리모트 트래킹 브랜치와 구분한다

리모트 트래킹 브랜치는 로컬에 존재하지만 임의로 움직일 수 없다. 리모트 서버에 연결할 때마다 자동으로 갱신될 뿐이다. 따라서 리모트 트래킹 브랜치는 일종의 북마크에 불과하다

### 리모트 브랜치 확인
그럼 본격적으로 리모트 브랜치를 확인하고 가져와보자

- `git branch -r` 명령으로 원격 저장소의 브랜치 리스트를 볼 수 있다
- `git branch -a` 명령으로 원격과 로컬의 모든 브랜치 리스트를 볼 수 있다

### 리모트 브랜치 가져오기
- `git checkout -t origin/<branch name>` 명령으로 로컬에 브랜치를 생성함과 동시에 리모트 브랜치가 가리키는 내용을 가져온다
- -t 옵션은 `--track' 옵션과 동일하다
- `git checkout -b <생성할 branch 이름> <원격 저장소의 branch 이름>` 명령은 리모트 브랜치의 이름을 변경하여 가져오게 한다

### refs
- [3.5 Git 브랜치 - 리모트 브랜치](https://git-scm.com/book/ko/v2/Git-%EB%B8%8C%EB%9E%9C%EC%B9%98-%EB%A6%AC%EB%AA%A8%ED%8A%B8-%EB%B8%8C%EB%9E%9C%EC%B9%98)

- [Git remote branch 가져오기](https://cjh5414.github.io/get-git-remote-branch/)