---
title: context API와 Redux
date: "2022-01-14T22:12:03.284Z"
description: "context API와 Redux"
thumbnail: "./thumbnail.png"
---


### 서론


최근 면접에서 Context API와 Redux의 설명에 관한 질문을 받았는데, 제대로 답변을 하지 못했다.


나: "Context API와 Redux는 둘다 전역 상태 관리에서 사용이 되는데, Context API는 depth가 깊어질수록 계층이 복잡해져 상태 관리가 어렵다는 단점이 있고, Redux는 이러한 단점을 보완하여..."

면접관님 : "Context API는 계층이 없어요."

나: "아하..넵..."


당시 전역 상태 관리를 애매하게 알고 있었고, context api와 redux는 툴 사용 경험만 있을 뿐, 둘의 차이점에 대해서는 제대로 이해를 하지 못했기 때문에 발생한 참사(?)였다...따라서, 이번 포스팅에서는 전역 상태 관리란 도대체 무엇이며, 전역 상태 관리가 필요한 상황에서 주로 사용되는 Context API와 Redux를 알아보고자 한다.


## 전역 상태관리란?


전역 상태 관리에 대해 알아보기 앞서, React에서 말하는 '상태'란 무엇인지부터 알아보자.


'상태'란 컴포넌트의 변경 가능한 데이터 저장소를 의미한다. '컴포넌트'는 속성과 상태가 존재하는 함수이며 그 결과값은 UI의 표현(뷰)이다.

리액트 교과서 (https://thebook.io/006961/part01/ch04/01/)


state는 어떤 컴포넌트에만 한정하여 사용되는 데이터를 포함하며, 해당 데이터는 시간이 지남에 따라 변경될 수 있습니다.

리액트 공식 문서 (https://ko.reactjs.org/docs/react-component.html#state)

정리를 하자면, 상태(state)란 컴포넌트에서 사용되는 변경 가능한 데이터를 의미한다. props와는 차이점이 있는데, props는 컴포넌트간 전달되는 데이터이지만, 상태는 컴포넌트 내에서 관리되며 시간이 지남에 따라 변경이 되는 동적인 데이터이다.


리액트의 상태는 '범위'에 따라 지역 상태(국소적)와 전역 상태(전체적)로 나뉠 수 있다. 지역 상태는 몇몇 컴포넌트에만 영향이 있지만, 전역 상태는 여러개의 혹은 전체 컴포넌트에 영향을 줄 수 있다. 주로 지역 상태는 'useState'를 통해 특정 컴포넌트에 한정되어 관리되거나 props로 전달해줄 수 있다. 하지만, 전역 상태의 경우 useState로만 관리된다면 props를 여러 개의 컴포넌트에 전달해주어야 하는 문제점이 생긴다.


예를 들어, 전역 상태가 적용되는 UI 테마를 생각해보자. 만약 따로 전역 상태 관리 라이브러리를 사용하지 않고, useState로만 관리를 해준다면 다음과 같은 문제점이 발생할 수 있다.


1) 부모 컴포넌트 App에서 UI 테마를 변경

1-1) 자식 컴포넌트 Component1에 변경된 UI 테마 props를 전달

1-2) 자식 컴포넌트 Component2에 변경된 UI 테마 props를 전달

1-3) 자식 컴포넌트 Component3에 변경된 UI 테마 props를 전달...


부모 컴포넌트에서 변경된 특정 props를 자식 컴포넌트로 내려주는 과정을 여러번 진행해야 한다. 앱의 규모가 작다면 상관이 없지만, 규모가 커질 수록 props 전달 과정이 복잡해져서 상태 관리가 어려워질 수 있다. 이 같은 문제를 props drilling 문제라고 하는데, 컴포넌트간 일일이 props를 전달하는 것을 말한다. context API와 Redux는 이런 props drilling 문제와 컴포넌트 간 의존성 문제를 해결에 유용하게 사용된다.



## Context API


리액트에서만 사용되는 전역 상태 관리를 위한 내장 라이브러리


● context 리액트 컴포넌트 트리 내에서 전역적 데이터를 공유할 수 있도록 고안된 방법으로, context 그 자체가 상태 관리라고는 볼 수 없다. 정확히 말하자면, useState와 useReducer로 상태 관리가 이루어지고, context는 이를 위한 수단으로 사용된다.

● createContext context 객체를 생성함

● Provider context를 구독하는 컴포넌트들에게 context의 변화를 알림. Provider 컴포넌트는 부모 컴포넌트에서 자식 컴포넌트로 전역 데이터를 전달할 때 사용

● Consumer context의 변화를 구독하는 컴포넌트

● useConsumer 해당 컴포넌트의 가장 가까이에 있는 Provider의 value 값 (context의 현재 값)을 반환함


● 사용 방법

```javascript
const themes = {
  light: {
    foreground: "#000000",
    background: "#eeeeee"
  },
  dark: {
    foreground: "#ffffff",
    background: "#222222"
  }
};


/* 전역 변수 themes에 대한 context 객체를 선언 */
const ThemeContext = React.createContext(themes.light);

/* Provider를 활용하여 themeContext를 구독하는 컴포넌트들에게 context의 변화를 알리도록 설정 */
function App() {
  return (
    <ThemeContext.Provider value={themes.dark}>
      <Toolbar />
    </ThemeContext.Provider>
  );
}

function Toolbar(props) {
  return (
    <div>
      <ThemedButton />
    </div>
  );
}

function ThemedButton() {
  const theme = useContext(ThemeContext); //useContext를 활용하여 themeContext에서 전달하고 있는 현재 값을 반환
  return (
    <button style={{ background: theme.background, color: theme.foreground }}>
      I am styled by theme context!
    </button>
  );
}
```

## Redux

리액트, 앵귤러, 뷰 등 다양한 환경에서 사용 가능한 전역 상태 관리 라이브러리

Dan Abramov에 의해 React + Flux의 구조에 reducer를 결합한 형태인 Redux 등장

Flux에서 처리하기 힘든 hot reloading과 time travel debugging을 해결함


* dan abramov의 강연 메모 참고


[강연 메모] Dan Abramov - Live React: Hot Reloading with Time Travel at react-europe 2015
Status Quo(the status in which) for JavaScript If render method or data transformation (etc) ch...

blog.naver.com

● reducer action을 받아서 새로운 state를 반환하는 순수 함수로, action을 통해 앱의 state를 어떻게 업데이트하는지 작성한다.

● store 전역 state를 관리하는 객체로 아래의 메소드를 가지고 있다. 리덕스 앱에는 오직 한 개의 store만 정의 가능하다.

- 사전에 정의한 reducer를 활용하여 createStore(reducer)로 redux store를 생성한다.

- getState를 통해 현재 state를 가져올 수 있다.

- dispatch(action)으로 store의 reducer에 action을 전달한다.

- subscribe(listener)로 이벤트 리스너 등록을 할 수 있다.


● 사용 방법


▶ reducer를 정의

```javascript
/* 초기 상태를 설정한다. */
const initialState = { value: 0 }

/* state와 action을 인자로 받아서 새로운 상태를 반환하는 reducer를 작성한다. */
function counterReducer(state = initialState, action) {
  /* 사전에 정의된 action의 타입이 다음과 같다면 새로운 상태를 반환한다. */
  if (action.type === 'counter/incremented') {
    return {
      ...state,
      value: state.value + 1
    }
  }
  // 그렇지 않다면, 기존의 state를 반환한다.
  return state
}
내용을 입력하세요.
▶ store를 정의


import { createStore } from 'redux'
import counterReducer from './reducer'

const store = createStore(counterReducer)
내용을 입력하세요.
리액트 앱의 경우, store 객체를 react-redux의 Provider에 전달하여 전역 스토어를 정의함.


import { Provider } from 'react-redux'

ReactDOM.render(
  <Provider store={store}>
    <App />
  </Provider>,
  document.getElementById('root')
)
```
▶ store 메소드의 활용 예시

```javascript
console.log('Initial state: ', store.getState()) // {value : 0}

store.dispatch({ type: 'counter/incremented' })
console.log(store.getState()) // {value : 1}
```
* 이번 포스팅에서는 리덕스의 사용 방법보다는, context api와 redux의 차이에 초점이 맞춰져 있다. 더 자세한 내용은 리덕스 공식 홈페이지를 참고하는 것을 추천한다.


그래서, Context API와 Redux 중 무엇을 사용할까?


context API와 Redux는 둘다 전역 상태 관리를 위한 도구이며, Redux는 context API를 가지고 만든 라이브러리이다. 따라서 상태 관리 그 자체의 방법에 따른 차이가 있다기 보다, 둘의 성능과 사용이 권장되는 상황 등에 따른 차이가 있다고 볼 수 있다.


1. 성능 상의 차이


Redux는 어떤 컴포넌트가 전역 상태의 특정 값을 의존하게 될 때, 해당 값이 바뀔 때만 리렌더링이 되도록 최적화 되어있다. 반면, Context API는 이런 성능 최적화가 이루어지지 않았기 때문에 컴포넌트가 특정 값에 의존하게 되는 경우, 해당 값과 상관없는 값이 바뀌게 되더라도 컴포넌트 리렌더링이 발생할 수 있다. 따라서, context API는 관심사의 분리가 상당히 중요하며, 상태용과 업데이트용 context 분리도 필요하기 때문에 작업해야 하는 일들이 많다. 전역 상태가 고도화 / 다양화 되는 경우에는 context API의 사용이 적합하지 않을 수도 있다.


RIDI에서 제공하는 아티클에 따르면, context API의 관심사 분리 상황에 대해 더 자세히 설명하고 있으니 읽어보는 것을 추천한다.


리덕스 잘 쓰고 계시나요? - 리디주식회사 RIDI Corporation
리덕스에는 정해진 규칙이 없습니다. 개발자들 모두 자신만의 방식으로 사용 하고 있어요. 하지만 제대로 사용하지 않으면 굉장히 불편할 수 있고, 유지 보수에 있어서 오히려 독이 될 수도 있어요. 그래서 리덕스를 사용할 때 고려하면 유용한 정보를 모았습니다.

ridicorp.com


2. 권장 사항



대표사진 삭제
https://blog.isquaredsoftware.com/2021/01/context-redux-differences/


- 기본적으로 context API는 리액트 내부 라이브러리이기 때문에 리액트에서만 사용되며, Redux는 UI와 독립적이기 때문에 별도로 사용할 수 있다.

- 단순히 prop-drilling 문제 해결을 위함이라면 context API를 사용한다. context + useReducer의 조합으로 적당히 복잡한 상태 관리의 구현이 가능하다.

- 만약, 앱 자체가 high-complex하거나 상태 관리 이외에 디버깅 툴, 테스트 코드의 유연한 작성, 미들웨어 등 추가적인 기능이 필요하다면 Redux를 사용한다.


References



리액트 상태 관리 가이드이미지 썸네일 삭제
리액트 상태 관리 가이드
Stevy의 개발 블로그 입니다

www.stevy.dev


[React] Context API 와 Redux 비교하기
[React] Context API 와 Redux 비교하기 리액트에서 전역 상태를 관리할 때 많이 쓰는 Context API 와 Redux를 사용법과 장단점 위주로 비교해 보겠습니다. 개요 전역 상태 관리는 언제 할까? Context API의 특징..

egg-programmer.tistory.com


리덕스 잘 쓰고 계시나요? - 리디주식회사 RIDI Corporation
리덕스에는 정해진 규칙이 없습니다. 개발자들 모두 자신만의 방식으로 사용 하고 있어요. 하지만 제대로 사용하지 않으면 굉장히 불편할 수 있고, 유지 보수에 있어서 오히려 독이 될 수도 있어요. 그래서 리덕스를 사용할 때 고려하면 유용한 정보를 모았습니다.

ridicorp.com


Blogged Answers: Why React Context is Not a "State Management" Tool (and Why It Doesn't Replace Redux)이미지 썸네일 삭제
Blogged Answers: Why React Context is Not a "State Management" Tool (and Why It Doesn't Replace Redux)
Definitive answers and clarification on the purpose and use cases for Context and Redux

blog.isquaredsoftware.com


﻿
