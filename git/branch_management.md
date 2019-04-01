#### 19.04.01

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