# React - useState VS useReducer

## 설명

useState와 useReducer모두 React에서 상태값을 관리하는 방법(훅)입니다.
둘의 차이점과 상황에 따라서 어떤 방법(훅)을 쓸지 확인해보겠습니다.

### 1. useState

- useState는 기본 Hook(꼭 알아야 하는 hook) 중 하나로 기존의 class형 컴포넌트에서만 가질 수 있었던 state를 함수형 컴포넌트에서도 가질 수 있게 해주는 훅입니다.

- useState는 상태 유지 값과 그 값을 갱신하는 함수를 반환하며, 아래와 같은 형태입니다.

```jsx
const [state, setState] = useState(initialState);
```

```jsx
// 기본적인 카운터를 useState로 구현한 코드입니다.
function Counter({initialCount}) {
  const [count, setCount] = useState(initialCount);
  return (
    <div>
      Count: {count}
      <button onClick={() => setCount(initialCount)}>Reset</button>
      <button onClick={() => setCount(prevCount => prevCount - 1)}>-</button>
      <button onClick={() => setCount(prevCount => prevCount + 1)}>+</button>
    </div>
  );
}
```

### 2. useReducer

useReducer는 useState의 대체 함수입니다. (state, action) => newState의 형태로 reducer를 받고 dispatch 메서드와 짝의 형태로 현재 state를 반환합니다.

```jsx
// 가장 기본 형태입니다.
const [state, dispatch] = useReducer(reducer, initialArg, init);
```

```jsx
기본적인 카운터를 useReducer로 구현한 코드입니다.

const initialState = {count: 0};

function reducer(state, action) {
  switch (action.type) {
    case 'increment':
      return {count: state.count + 1};
    case 'decrement':
      return {count: state.count - 1};
    default:
      throw new Error();
  }
}

function Counter() {
  const [state, dispatch] = useReducer(reducer, initialState);
  return (
    <div>
      Count: {state.count}
      <button onClick={() => setState((prev)=>{prev-1})}>-</button>
      <button onClick={() => setState((prev)=>{prev+1})}>+</button>
    </div>
  );
```

### 3. useState가 더 간결해보이는데 useReducer를 쓰는 이유

- 위의 예시 카운터 코드만 보면 useState가 useReducer보다 더 가독성이 높고 간결하여 useReducer를 사용할 이유가 전혀 없어 보입니다.
- 실제로 위의 카운터 예시처럼 단독적으로 (이 경우 카운트의 값은 카운터에만 사용되어짐) 사용되어지는 상태값을 관리하는 경우 useState를 쓰는 것이 더 효율 적일 것입니다.

- 코드 복잡도가 높아지고 가독성이 상대적으로 떨어지는 useReducer는 다수의 하윗값을 포함하는 복잡한 정적 로직을 만드는 경우나 다음 state가 이전 state에 의존적인 경우에 보통 `useState`보다 `useReducer`가 많이 사용되어집니다. 또한 `useReducer`는 자세한 업데이트를 트리거 하는 컴포넌트의 성능을 최적화할 수 있게 하는데, 이것은 [콜백 대신 `dispatch`를 전달](https://ko.reactjs.org/docs/hooks-faq.html#how-to-avoid-passing-callbacks-down) 할 수 있기 때문입니다.

## 요약

- 둘 중 어떤 상태관리를 사용해도 문제가 되지는 않습니다.
- 독립된 부분의 상태값을 관리하고 싶을 때는 useState를,
- 다른 부분의 값에 관련된 상태를 관리하고자 할 때는 useReducer를 쓰는 것이 추천되어집니다.

## 참고

- [React.js(useState)](https://ko.reactjs.org/docs/hooks-reference.html#usestate)
- [React.js(useReducer)](https://ko.reactjs.org/docs/hooks-reference.html#usereducer)
- [Kentcdodds' Blog](https://kentcdodds.com/blog/should-i-usestate-or-usereducer)
