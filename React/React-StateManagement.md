# React - state management

## 설명

React의 상태관리의 필요성에 대해 알고
React와 결합할 수 있는 상태관리 라이브러리의 종류를 비교한다.

### 1. 상태관리의 필요성

React는 부모가 자식 컴포넌트에게 props를 전달해줄 수 있다는 특징이 있다.
만약 모든 컴포넌트가 사용해야 하는 상태가 있다면 모든 컴포넌트에 props를 전달해야 한다(상태 끌어올리기)
더 나아가서
컴포넌트의 자식, 자식, 자식 그 이상의 자식에게까지 전달해야 하는 상태가 있다면?
실제로 상태를 사용하지 않는 컴포넌트까지 props가 지나쳐서 그 자식에게로 전달해야 하기 때문에 비효율적이다.
따라서 이런 상태를 관리해주는 상태관리가 필요하다.

#### 0.상태?

- 웹을 렌더하는데 있어 영향을 미칠 수 있는 **동적인 값**
- 따라서 useState또한 상태관리를 하는 방법이다.

**지역상태**

- 특정 컴포넌트 안에서만 관리된다
- 다른 컴포넌트와 데이터를 공유하지 않는다
- 보통 form이 해당된다

**컴포넌트 간 상태**

- 여러 컴포넌트에서 관리되는 상태
- 일반적으로 prop drilling 방식을 사용한다(위에서 말한 계속 전달`)

**전역 상태**

- 프로젝트 전체에 영향을 미치는 상태

### 1.React의 상태관리

상태 관리 라이브러리를 이용하는 방법 외에도 React가 자체적으로 가지고 있는 상태 끌어올리기(Lifting State Up)을 이용해 자체적 해결이 가능하다

#### 컴포넌트 합성

> React는 강력한 합성 모델을 가지고 있으며, 상속 대신 합성을 사용하여 컴포넌트 간에 코드를 재사용하는 것이 좋습니다

**children prop 사용하기**

<details>
<summary>코드 보기</summary>

```
function WelcomeDialog() {
   return (
  <FancyBorder color="blue"> // ✨ 1
    <h1 className="Dialog-title">
     Welcome
    </h1>
    <p className="Dialog-message"> Thank you for visiting our spacecraft! </p>
  </FancyBorder>
   );
    }

```

```

function FancyBorder(props) {
return (

<div className={'FancyBorder FancyBorder-' + props.color}>
{props.children} // ✨ 1
</div>
);
}

```

</details>

**특수화**

<details>
<summary>코드 보기</summary>

```

function Dialog(props) {
return (
<FancyBorder color="blue">

<h1 className="Dialog-title">
{props.title} // ✨ 1
</h1>
<p className="Dialog-message">
{props.message} // ✨ 2
</p>
</FancyBorder>
);
}

function WelcomeDialog() {
return (

<Dialog
title="Welcome" // ✨ 1
message="Thank you for visiting our spacecraft!" /> // ✨ 2
);
}

```

</details>

#### Context

<details>
<summary>코드 보기</summary>

`<theme-context.js>`

```

export const themes = {
light: {
foreground: '#000000',
background: '#eeeeee',
},
dark: {
foreground: '#ffffff',
background: '#222222',
},
};

export const ThemeContext = React.createContext(
themes.dark // 기본값
);

```

`<themed-button.js>`

```

import {ThemeContext} from './theme-context';

class ThemedButton extends React.Component {
render() {
let props = this.props;
let theme = this.context;
return (
<button
{...props}
style={{backgroundColor: theme.background}}
/>
);
}
}
ThemedButton.contextType = ThemeContext;

export default ThemedButton;

```

</details>

> context를 이용하면 단계마다 일일이 props를 넘겨주지 않고도 컴포넌트 트리 전체에 데이터를 제공할 수 있습니다.

### 2.상태관리 라이브러리의 필요성

Context API와 컴포넌트 합성을 이용해서 상태관리를 할 수 있지만 Context API는 값이 변할 때 Context를 구독하는 모든 컴포넌트의 리렌더링이 발생한다는 문제가 있다
이러한 문제는 useReducer이나 memorization을 사용하면 제어가 가능하지만 번거로운 과정이다.

### 3.상태관리 라이브러리

#### Redux

<img src="https://user-images.githubusercontent.com/87933367/164347085-ddac292c-3a41-4987-a93b-bd0c01eec42a.jpg">

1. 상태를 한 곳에서 관리하고 변경사항을 예측, 추적할 수 있다
2. 변경 사항을 쉽게 파악할 수 있다
3. React-Redux의 대규모 커뮤니티에서 도움을 구할 수 있다 : React 생태계에서 가장 사용률이 높다
4. **성능 최적화를 보장하여 필요할 시 재렌더링한다**
5. Recoil storage에 상태를 유지하고 복원할 수 있다
6. SSR이 가능하여 서버 요청에 대한 응답과 함께 상태를 서버로 전송할 수 있다
7. 유지보수성 : 컴포넌트 트리에서 비즈니스 로직 분리가 가능하다
8. 디버깅이 간단하다
9. 미들웨어(Middleware)가 존재해서 액션 객체가 리듀서에서 처리되기 전에 우리가 원하는 작업을 수행시킬 수 있다. : 비동기 작업에 좋음

#### Recoil

<img src="https://user-images.githubusercontent.com/87933367/164347102-cf65f44a-e79f-4828-9dd4-dfe4247a2335.jpg">

1. 쉬운 러닝 커브 : React useState의 글로벌 버전처럼 사용할 수 있다
2. Boilerplate-free API
3. atom이라는 상태 저장 공간을 두어 상태를 쉽게 참조할 수 있다
4. 상태를 기반으로 하는 파생 데이터를 계산할 수 있는 selector 함수를 이용하여 효율적으로 계산할 수 있다 : 쓸모없는 상태의 보존 방지
5. 간편한 비동기 처리 : Redux에서는 추가적인 api였던 비동기 처리나 캐싱을 자체적으로 selector에서 지원한다
6. 렌더링 최적화

## 요약

React에서 상태 관리의 필요성을 느낄 때 이용할 수 있는 여러 가지 방법들에 대해 알아봤다.
이 외에도 상태 관리 라이브러리는 많이 존재하지만 중요한 점은 **굳이** 이 상태 관리 방법을 사용해야 하는 이유에 대해서 잘 아는 것이다.

## 참고

- [벨로퍼드와 함께하는 리덕스](https://react.vlpt.us/redux/)
- [[FE] 리액트 상태관리 1부.](https://velog.io/@longroadhome/FE-%EB%A6%AC%EC%95%A1%ED%8A%B8-%EC%83%81%ED%83%9C%EA%B4%80%EB%A6%AC-1%EB%B6%80)
- [2021년 React 상태 관리 라이브러리 전쟁 : Hooks, Redux, Recoil](https://mmsesang.tistory.com/entry/2021%EB%85%84-React-%EC%83%81%ED%83%9C-%EA%B4%80%EB%A6%AC-%EB%9D%BC%EC%9D%B4%EB%B8%8C%EB%9F%AC%EB%A6%AC-%EC%A0%84%EC%9F%81-Hooks-Redux-Recoil)
- [합성 (Composition) vs 상속 (Inheritance)](https://ko.reactjs.org/docs/composition-vs-inheritance.html#gatsby-focus-wrapper)
- [React에서 상태관리하기 (자체적 방법과 라이브러리 비교하기 Recoil, Redux)](https://pottatt0.tistory.com/82)
