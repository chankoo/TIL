#### 19.05.06

## 키워드
React, Component, props, state, virtual DOM

## React?
- A Javascript library for building UI
- Component 단위 개발
- Virtual DOM을 이용한 high performance
- Unidirectional data flow
- HTML과 javascript를 합친듯한 jsx 문법을 이용


## React Component
- 리액트 사용 이유 첫번째는 __정리정돈__ -> 코드의 복잡도를 낮추는 것
- 클래스 정의해서 하나의 컴포넌트 만든다
- 개념적으로 컴포넌트는 props를 input으로, UI 정의하는 React Element를 output으로 하는 __함수__
- 그렇기에 React를 이용하면 __UI를 독립적인 단위로 쪼개고, 재사용 가능__ 하다
- 컴포넌트를 만들땐 반드시 하나의 최상위 태그로 감싸야함
- 컴포넌트의 계층 존재(without REDUX)
    - 상위에서 하위를 제어할때는 __props__ 로 명령 전달
    - 하위에서 상위를 제어할때는 __이벤트__ 를 이용

### why Component?
- 복잡한 문제를 작게 나누어 개발하기 위해 필요
- 작은 컴포넌트 단위로 개발하면 캡슐화, 확장성, 결합성, 재사용성과 같은 장점
- 관심사의 분리
    - ![스크린샷, 2019-05-06 22-10-08](https://user-images.githubusercontent.com/38183218/57227198-c155e200-704b-11e9-8d77-f88d1ed8b564.png)
    - 기존 개발 방식은 마크업, 디자인, 로직을 분리하는 기술의 분리(separation of technologies)에 불과
    - React는 하나의 개개의 관심사로 분리된 컴포넌트 단위로 작업(separation of concerns)

> #### 컴포넌트 파일로 분리하기
>- 모든 컴포넌트가 App.js에 존재한다면 컴포넌트가 늘어남에따라 복잡해진다
>- 컴포넌트를 파일로 분리해서 관리해보자
>- 예를들면 src/components 에 다음과 같은 포맷으로 TOC.js 파일을 생성한다
    - `import React, { Component } from 'react';`
    - `export default TOC;`
>- App.js에 `import TOC from "./components/TOC";`로 TOC.js의 정보를 받아온다


## props
- 컴포넌트의 어트리뷰트로 해당 컴포넌트 객체로 전달된다
- 상위의 컴포넌트에서부터 one-way flow를 갖는다
- 수정이 안된다(setProps 메소드는 deprecated). 컴포넌트의 수명주기 동안 props는 불변이다


## state vs props
![스크린샷, 2019-05-06 21-49-45](https://user-images.githubusercontent.com/38183218/57226080-147a6580-7049-11e9-8387-acdaba2aa735.png)


공통적으로 render 통해 UI를 제어하는 state와 props

- __state__
    - 컴포넌트 내부의 상태
    - `this.setState`로 수정 가능. 비동기로 동작

- __props__
    - 외부적으로 컴포넌트를 제어(상위 컴포넌트에서 전달)
    - 내부적으로 수정 불가
컴포넌트 실행시 constructor가 제일 먼저 실행되며 초기화를 담당한다

동적으로 생성하는 element는 key 라는 props를 가져야한다

### 상태 저장 컴포넌트
![스크린샷, 2019-05-06 21-47-45](https://user-images.githubusercontent.com/38183218/57226084-16dcbf80-7049-11e9-95c6-0a10b3d3581b.png)

일반적으로 내부 state를 갖는 컴포넌트를 말한다
### 상태 비저장 컴포넌트
![스크린샷, 2019-05-06 21-47-57](https://user-images.githubusercontent.com/38183218/57226086-180dec80-7049-11e9-9dc6-a2af7f9788e4.png)

일반적으로 컴포넌트 내부에 state가 없으며 함수로 구현된다

## Virtual DOM
![스크린샷, 2019-05-06 22-23-49](https://user-images.githubusercontent.com/38183218/57227993-ac7a4e00-704d-11e9-9ea7-3ecd8f88455a.png)

기존 jQuery 방식의 경우 DOM에 변화가 생기면 렌더 트리를 재생성한다. 

이 과정에서 모든 요소들의 스타일을 다시 계산하고, 레이아웃 만들고 페인팅 하는 일련의 과정이 반복된다.

React는 Virtual DOM을 이용해 reflow와 repaint 연산을 줄인다.

![스크린샷, 2019-05-06 22-28-27](https://user-images.githubusercontent.com/38183218/57228249-4d690900-704e-11e9-90ad-0c562b5d09a7.png)

이는 새로운 개념이 아니다. DOM 트리의 변화가 있을때 오프라인(on-memory) DOM에 적용하고, 모든 연산이 끝났을때 변경된 부분을 실제 DOM에 최종적으로 한번만 업데이트 하는 것이다. 

#### reference
- [서강대 WebStudio 2019](https://github.com/sisobus/WebStudio2019/blob/master/9_react/lecture/WebStudio2019_9_react.pdf)
- [생활코딩-React](https://www.youtube.com/playlist?list=PLuHgQVnccGMCRv6f8H9K5Xwsdyg4sFSdi)
- https://medium.com/little-big-programming/react%EC%9D%98-%EA%B8%B0%EB%B3%B8-%EC%BB%B4%ED%8F%AC%EB%84%8C%ED%8A%B8%EB%A5%BC-%EC%95%8C%EC%95%84%EB%B3%B4%EC%9E%90-92c923011818
- https://velog.io/@kyusung/React-%EC%82%B4%ED%8E%B4%EB%B3%B4%EA%B8%B0
