#### 19.03.18

## 개요
fork한 remote repo, 원래의 remote repo, 그리고 local repo의 싱크 맞추기

## Keywords
fork, clone, origin, upstream

## 현재 상황
original repo(남에꺼) 를 내 remote repo(내꺼)에 fork한 후, local(내꺼) 에 clone한 상태.

이후 original repo에서 새 커밋 발생해서 싱크 안맞는 상황 

![sync](https://user-images.githubusercontent.com/38183218/54526043-e23d7780-49b8-11e9-9642-87f74f1c4efa.png)

## origin과 upstream의 구분
clone한 로컬에서 origin은 해당 local repo의 remote repo(내꺼)를 의미.

upstream은 original(남에꺼)를 의미

##
fork & clone 후에는 일반적으로 아래와 같이 origin만 등록되어있다

> chankoo@chankoo-gram:~/GitHub/WebStudio2019$ `git remote -v`

> origin	https://github.com/chankoo/WebStudio2019.git (fetch)

> origin	https://github.com/chankoo/WebStudio2019.git (push)

'upstream' 이라는 이름으로 원래의 repo(남에꺼)를 새로운 원격 저장소로 추가해주자(이름은 사실 상관없다)

> `git remote add upstream https://github.com/sisobus/WebStudio2019.git` 

그럼 fetch와 merge 명령으로 upstream의 내용을 가져와 내 로컬에 병합할 수 있다

> `git fetch upstream`

자신의 브랜치가 master임을 확인(checkout master)하고

> `git checkout master`

머지한다

> `git merge upstream/master` 

이제 origin repo(내꺼)와 싱크를 맞추기위해 push 한다

> `git push origin/master`


