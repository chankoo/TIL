#### 19.04.01
#### 19.04.08

## 브랜치 관리
![스크린샷, 2019-04-01 14-22-58](https://user-images.githubusercontent.com/38183218/55305093-bbd40d80-5489-11e9-80c7-ab5e0b6816b2.png)

현재 git branch들의 상황이다. master를 포함한 4개 브랜치가 있는데 `git branch -v` 명령으로 마지막 커밋 메시지를 보여준다. 현재 로컬의 소스코드는 master 브랜치를 따르고있다

### 브랜치 삭제
우선 불필요한 브랜치를 삭제해보자. `git branch --merged`의 브랜치 중 `*` 기호가 붙지 않은 3-practice 브랜치는 삭제해도 된다는 뜻이다. 이미 다른 브랜치와 merge한 상태이기에 삭제해도 정보를 잃지 않는다. 

반면 `git branch --no-merged`의 브랜치는 아직 merge 하지 않은 커밋을 담고있기에 강제로 삭제해야한다. 2-practice는 필요없는 내용이기에 `git branch -D` 로 삭제한다. 

### 작업 브랜치의 PR
`git checkout 4_flask_restful_chankoo` 명령으로 작업 브랜치로 이동한다. 이때 커밋안된 master 브랜치의 작업 내용을 커밋하거나, 삭제해야 checkout이 가능함을 유의하자. (파이참에서 진행했기에 `git checkout -- .idea/workspace.xml` 명령으로 불필요한 변경사항을 버렸다)

그런후에 `git push --set-upstream origin 4_flask_restful_chankoo`
명령으로 origin master에 push한다. 이후 github의 해당 repo로 가보면 pull&request 가능하다

### master에 merge
`git checkout master` 으로 로컬의 마스터 브랜치로 이동한다

`git merge 4_flask_restful_chankoo`로 
작업 완료한 4번 브랜치를 병합한다

`git branch --merged`로 4번 브랜치가 삭제가능한 브랜치임을 확인한다

`git branch -d 4_flask_restful_chankoo`
마지막으로 브랜치를 삭제한다

![스크린샷, 2019-04-01 15-35-01](https://user-images.githubusercontent.com/38183218/55307862-c4c9dc80-5493-11e9-9c6f-c965e2e3d018.png)

그결과 4번 브랜치의 커밋 히스토리가 마스터 브랜치에 병합되었다. 현재 커밋 오브젝트는 병합이 일어났음을 명시한다

### origin master에 push
지금까지의 작업은 origin master 와 관계없이 이루어졌다. `git push origin master` 를 통해 작업내용을 전송한다

### 과거의 커밋 히스토리에서 새로 분기하는 브랜치 만들기
![스크린샷, 2019-04-08 13-56-53](https://user-images.githubusercontent.com/38183218/55699569-5098b700-5a06-11e9-879d-6d2d53dd8f0b.png)

현재 상황: `5-practice-chankoo` 브랜치를 upstream master에 PR하려는데 불필요한 커밋이 쌓인상황. 즉, PR하려는 커밋은 Apr 04의 'ORM 적용' 이지만 선형적으로 쌓이는 커밋 객체의 특성상 지난 작업들이 모두 PR되는 상황이다

하려는 것: `upstream/master` 가 마지막으로 커밋된 시점으로 돌아가 필요한 파일만 커밋하는 새로운 브랜치 `5_1-practice-chankoo`를 만드려고 한다

과정:

1. ![스크린샷, 2019-04-08 14-05-53](https://user-images.githubusercontent.com/38183218/55699792-75415e80-5a07-11e9-9d0a-ead72b290cd4.png)
`git fetch upstream master`로 'FETCH_HEAD'를 `upstream/master` 브랜치의 마지막 커밋 오브젝트로 옮긴다

2. ![스크린샷, 2019-04-08 14-08-48](https://user-images.githubusercontent.com/38183218/55699906-de28d680-5a07-11e9-9ea6-febaecf7755c.png)
위와같이 현재 커밋 시점 이후의 커밋 히스토리가 untracked 되어있다

3. `git checkout -b 5_1-practice-chankoo`로 'HEAD가 분리된' 상태의 커밋을 유지하는 브랜치를 만든다

4. ![스크린샷, 2019-04-08 14-11-03](https://user-images.githubusercontent.com/38183218/55699954-2d6f0700-5a08-11e9-89bd-638483bf8ab8.png)
필요한 작업을 진행하고 커밋한다

5. ![스크린샷, 2019-04-08 14-13-28](https://user-images.githubusercontent.com/38183218/55700008-86d73600-5a08-11e9-96e1-9950b40680cd.png)
브랜치가 성공적으로 분기되었다

6. remote 레포지토리 origin에 push하고 upstream에 PR하면 끝
