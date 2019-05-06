#### 19.05.06

## 이벤트
이벤트로 인해 App.js의 state가 바뀌고 그에따라 하위 컴포넌트의 props에 영향을 미치는 형태

props나 state 값이 바뀌면 해당되는 컴포넌트의 render함수가 다시 호출(화면이 다시 그려짐)된다

이벤트 실행시(함수 호출시) 함수의 첫번째 파라미터로 이벤트 객체를 넘겨준다

`e.preventDefault();` 로 해당 태그의 주된 동작을 못하게 막을 수 있다

이벤트 실행으로 컴포넌트의 state 바꾸려면 

- 1) .bind(this)로 컴포넌트 가리키는 this 객체 바인딩
- 2) this.setState() 호출 

### bind
bind로 인해 해당 함수 내의 `this`는 bind의 파라미터가 된다

실제동작은 bind로 원래 함수를 복제한 새로운 함수가 만들어지며, bind의 파라미터가 새로운 함수의 `this`로 지정된다

### setState
동적으로 state 값을 바꿀때는 반드시 setState 메소드 이용한다

constructor에서 처럼 `this.state.mode = 'welcome';` 으로 직접 바꾸면 react는 이를 인식하지 못하고 render가 갱신되지 않는다

## 컴포넌트 이벤트 만들기
특정 컴포넌트의 props로 `onChangePage` 라는 이벤트(함수)를 정의한다

이어서 해당 컴포넌트를 정의하는 부분에 `onClick` 속성으로 함수를 넣어 `this.props.onChangePage();`를 호출하면 우리가 정의한 컴포넌트 이벤트가 호출된다

`data` 접두사가 붙은 변수는 이벤트의 타겟이 되는 태그의 `dataset` 속성에 저장된다. 

즉 `data-id = {data[i].id}`로 i번째 컨텐츠의 id를 받아왔다면 `e.target.dataset.id`로 그 id를 꺼내 쓸 수 있다

다른 방법은 `bind`의 인자로 `data[i].id`를 넘겨주는 것도 있다


#### reference
- [생활코딩-React](https://www.youtube.com/playlist?list=PLuHgQVnccGMCRv6f8H9K5Xwsdyg4sFSdi)
