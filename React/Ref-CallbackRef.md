# React - Ref, CallbackRef

## 설명

### 1. Ref란?

- Ref는 DOM노드에 직접 접근하기 위해 사용합니다.
- 가령 input 엘리먼트에 focus이벤트를 주고싶다면 state만으로 해결하기 어려우며, 노드에 직접 접근해 `inputElement.focus()`를 호출해야 할 것입니다.
- HTML엘리먼트에 ref속성을 추가해 직접 접근하고 조작하기
- 클래스 컴포넌트에 ref속성을 추가해, 마운트된 컴포넌트에 인스턴스 내용 받아오기
(함수 컴포넌트는 인스턴스가 없으므로 ref 속성은 클래스 컴포넌트에만 속성으로 추가할 수 있다.  `useRef`를 사용하는 것과는 별개의 문제임.)

### 2. Ref  생성 방법

class컴포넌트에서는 `createRef`를 이용해서 생성하며, 함수 컴포넌트에서는 `useRef`를 이용해서 생성할 수 있다.

#### 2-1. **클래스 컴포넌트**

```jsx
class MyComponent extends React.Component {
  constructor(props) {
    super(props);
    this.myRef = React.createRef();
  }
  render() {
    return <div ref={this.myRef} />;
  }
}
```

### 2-2. **함수 컴포넌트**

```jsx
function MyComponent () {
  const myRef = useRef(null);
  
  return <div ref={myRef}></div>;
}
```

### 3. Ref  동작

- **컴포넌트가 mount될 때 ref의 `current` 속성에 DOM노드를 대입, 컴포넌트가 unmount될 때 `current` 속성을 다시 null**로 돌려놓습니다.
- `componentDidMount` 또는 `componentDidUpdate` 생명주기 메서드가 호출되기 전에 ref의 수정된 내용이 업데이트 됩니다.
- **ref 내용의 변경은 생명주기 함수를 호출시키지 않는다**. 이것이 state로 관리하는 것과의 차이점이기도 합니다.

### 4. Callback Ref

- React에서 ref가 설정되고 해제되는 부분을 직접 관리하고 싶다면 callback Ref를 사용할 수 있습니다. 
- `React.createRef()`를 사용하는 대신 직접 설정한 함수를 전달하면 된다. 이렇게 하면 꼭 unmount 호출 시점이 아니더라도 ref 속성을 제거하는 것이 가능해집니다.

```jsx
class CustomTextInput extends React.Component {
  constructor(props) {
    super(props);

    this.textInput = null;

    this.setTextInputRef = element => {
      // 여기서 ref가 업데이트될 때는 React의 생명주기 함수가 호출되지 않음.
      this.textInput = element; 
    };
  }

  render() {
    return (
      <div>
        <input
          type="text"
          ref={this.setTextInputRef}
        />
        <input
          type="button"
          value="Focus the text input"
        />
      </div>
    );
  }
}
```

### 5. useRef

- 함수 컴포넌트에서 ref 속성을 사용하기 위해서는 React Hook 중의 하나이 `useRef`를 사용하면 됩니다.
- `useRef`를 사용할때도 callback ref가 사용하고 싶다면 `useCallback`과 함께 사용하면 됩니다.

```jsx
function MeasureExample() {
  const [height, setHeight] = useState(0);

  const measuredRef = useCallback(node => {
    if (node !== null) {
      setHeight(node.getBoundingClientRect().height);
    }
  }, []); // 종속성 배열로 빈 배열을 사용했으므로 ref 콜백이 리렌더링 시 불필요하게 호출되지 않습니다.

  return (
    <>
      <h1 ref={measuredRef}>Hello, world</h1>
      <h2>The above header is {Math.round(height)}px tall</h2>
    </>
  );
}
```

### 6. Forwarding Refs

- `ref`를 컴포넌트를 통해 자식 컴포넌트 중 하나에 전달하는 기법을 의미합니다.
- `forwardRef`함수로 둘러싼 컴포넌트에 `ref`를 전달하면, `forwardRef`의 자식 컴포넌트에 `ref`가 전달됩니다.

```jsx
// App.js
import { createRef } from "react";
import FancyButton from "./FancyButton";

const ref = createRef();

export default function App() {
  return (
    <div className="App">
      <FancyButton ref={ref}>Click me!</FancyButton>
    </div>
  );
}

```

```jsx
//FancyButton.js
import React from "react";

const FancyButton = React.forwardRef((props, ref) => {
  return <button ref={ref}>{props.children}</button>;
});

export default FancyButton;

```

- 위 예시에서는 `forwardRef`함수안의 `<button>` 태그 안에 ref가 전달됩니다.
- 그리고 `forwardRef` 안의 함수에 함수인자로 `props`와 `ref`가 있는데, `ref`인자는 `forwardRef`함수를 사용할 때만 존재합니다.

## 요약

- ref는 DOM노드나 자식 React 컴포넌트에 직접 접근하기 위해 사용합니다.
- ref는 컴포넌트가 mount될 때 current 속성값으로 지정되며 한번 설정된 값은 변경되지 않습니다.
- ref값의 변화는 React 생명주기 함수를 호출하지 않습니다.
- ref가 설정되고 해제되는 부분을 직접 관리하고 싶다면 callback ref를 이용할 수 있습니다.

## 참고

- [React 공식문서 - Ref](https://ko.reactjs.org/docs/refs-and-the-dom.html)
- [React 공식문서 - useRef](https://ko.reactjs.org/docs/hooks-reference.html#useref)