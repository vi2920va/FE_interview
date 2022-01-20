# React - props.children

## 설명

- React는 강력한 합성 모델을 가지고 있으며, 상속 대신에 합성을 사용하여 컴포넌트 간에 코드를 재사용하는 것이 좋습니다.
- `props.children`은 컴포넌트의 **여는 태그와 닫는 태그 사이의 내용을**  포함한다.
- 즉, 태그와 태그 사이에 담을 내용을 `props.children`이라는 값으로 접근할 수 있습니다.

```jsx
// App.jsx
import Dialog from "./Dialog";

const App = () => {
  return (
    <div>
      <Dialog title="welcome" message="react props message!" />
      <button>button</button>
    </div>
  );
};
export default App;
```

```jsx
// Dialog.jsx
import React from "react";

const Dialog = (props) => {
  return (
    <div color="blue">
      <h1 className="dialog-title">{props.title}</h1>
      <p className="dialog-message">{props.message}</p>
      {props.children}
    </div>
  );
};
export default Dialog;

```

## 요약

- `props.children`은 컴포넌트에서 다른 컴포넌트를 담을 때 사용할 수 있습니다.
- 특수한 경우에는 컴포넌트 사용을 고려해야 하는 경우, 구체적인 컴포넌트가 일반적인 컴포넌트를 렌더링하고 `props`  를 통해 전달할 수 있습니다.
- 컴포넌트에 `props.children`를 여러 번 사용해서도 쓸 수 있습니다.

## 참고

- [props.children(예제코드)](https://codesandbox.io/s/children-hx7sz?file=/src/App.js)
- [[React] this.props.children](https://velog.io/@hanei100/React-this.props.children)
- [props.children](https://ko.reactjs.org/docs/glossary.html#propschildren)