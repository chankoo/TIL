#### 19.08.02


## 개요
브랜치 관리를 위해 reset, revert, 그리고 rebase까지 정확히 이해해보자

## reset vs revert
몹쓸 커밋이 만든 문제를 해결하는 방법은 보통 reset과 revert 두가지이다. 흔히 __reset__ 은  '시계를 과거로 돌리는' 것에 __revert__ 는 '문제를 없던 일로 만드는' 것에 비유된다. 즉, 몹쓸 커밋을 없애버리는 reset이다. 반면, revert는 몹쓸 커밋에 더해진 코드를 삭제하고 삭제된 코드를 더하는 새로운 커밋을 만든다

### reset
3가지 옵션이 있고 아래의 형태로 쓰인다

- `$ git reset <옵션> <돌아가려는 커밋>`

1. hard
: 돌아가려는 커밋 이후의 모든 내용을 지운다.
![hard](https://user-images.githubusercontent.com/38183218/62362982-250f7e80-b559-11e9-90cd-a69b72a88e71.png)

2. soft
: 돌아가려는 커밋으로 돌아가지만, 이후 내용은 파일에서 지워지지 않고 git이 인덱싱한 상태로 남아있다. 바로 다시 커밋 가능하다
![soft](https://user-images.githubusercontent.com/38183218/62362980-250f7e80-b559-11e9-80f1-3eeb0ba5314b.png)

3. mixed(default)
: 역시 커밋으로 돌아가고, 인덱싱이 지워진다. 즉, add가 필요한 상태이다
![mix](https://user-images.githubusercontent.com/38183218/62362981-250f7e80-b559-11e9-8636-99057c043f44.png)

### revert
- `$ git revert <돌아가려는 커밋>`
- `$ git revert <돌아가려는 커밋>.. <현재의 바로 이전 커밋>`

돌아가려는 커밋의 반대로 작용하는 새로운 커밋을 만든다. 조심할 것은 현재 커밋과 돌아가려는 커밋 사이에 다른 커밋들이 있다면 충돌할 가능성이 높다는 것. 그렇기 되돌릴 커밋의 범위를 지정해주어 순차적으로 되돌리게끔 만든다.

### 다시 reset vs revert
- 이미 push한 상태라면 무조건 revert를 사용하자
- reset은 돌아간 커밋 이후의 내역을 모두 지우기에 좋지 못하다

## 브랜치의 분기와 rebase
그렇다면 checkout으로 돌아가면 어떨까? 물론 이미 push한 상황이라면 좋지 않다.

![Screen Shot 2019-08-02 at 7 16 39 PM](https://user-images.githubusercontent.com/38183218/62363328-17a6c400-b55a-11e9-9444-4658a819c411.png)

위와 같이 checkout으로 돌아간 커밋에서 내용을 수정하면 브랜치의 분기가 발생한다.

![mer](https://user-images.githubusercontent.com/38183218/62363817-0316fb80-b55b-11e9-92bb-c7218d3829ba.png)

이때 마스터 브랜치에서 `git merge experiment` 명령으로 간단히 머지할 수 있다. 이는 조상을 2개 갖는 커밋을 만들어낸다.

![rebase](https://user-images.githubusercontent.com/38183218/62363818-0316fb80-b55b-11e9-9fef-2a19e183995c.png)

다른 방식으로 experiment(C4)의 수정사항을 master(C3)에 반영하여 새로운 (하지만 조상은 하나인)커밋을 만들어 낼 수도 있다. 이를 'rebase'라 하며 명령은 다음과 같다.

- `git checkout experiment`
- `git rebase master`

"실제로 일어나는 일을 설명하자면 일단 두 브랜치가 나뉘기 전인 공통 커밋으로 이동하고 나서 그 커밋부터 지금 Checkout 한 브랜치가 가리키는 커밋까지 diff를 차례로 만들어 어딘가에 임시로 저장해 놓는다. Rebase 할 브랜치(역주 - experiment)가 합칠 브랜치(역주 - master)가 가리키는 커밋을 가리키게 하고 아까 저장해 놓았던 변경사항을 차례대로 적용한다."

![ㄱ](https://user-images.githubusercontent.com/38183218/62363819-0316fb80-b55b-11e9-862b-01bba126e0ae.png)

이후 master 브랜치를 가장 최근 커밋인 C4'와 동기화 시켜야 한다. 이를 'fast-forward'라 하며
- `git checkout master`
- `git merge experiment` 로 가능하다

결과적으로 브랜치 log가 선형적으로 관리되며 깔끔해진다. merge와 최종 결과물은 같지만 커밋 히스토리 관리에 이점이 생기는 것이다.

하지만 rebase는 기존의 커밋을 그대로 이용하는 것이 아니라 내용은 같지만 다른 커밋을 새로 만든다. 그렇기 때문에 이미 push된 커밋을 rebase로 바꿔 버리면 협업 상황에서 복잡하게 꼬이는 문제가 있으니 조심하자

### refs
-[git-BackToTheFuture](https://www.devpools.kr/2017/02/05/%EC%B4%88%EB%B3%B4%EC%9A%A9-git-%EB%90%98%EB%8F%8C%EB%A6%AC%EA%B8%B0-reset-revert/)

- [gitBook](https://git-scm.com/book/ko/v2/Git-%EB%8F%84%EA%B5%AC-Reset-%EB%AA%85%ED%99%95%ED%9E%88-%EC%95%8C%EA%B3%A0-%EA%B0%80%EA%B8%B00)