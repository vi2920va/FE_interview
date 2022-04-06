# React - Version 18 Summary

## 설명

이번 React 18의 정식 릴리즈는 앞으로 몇 달 뒤로 예정되어 있습니다. 하지만, 리액트 사용자 입장에서도 이번 React 18은 미리 알아두면 도움이 많이 됩니다. 지금까지의 웹 프론트엔드 개발에서 한 단계 진보하여 동시성 제어가 가능해지기 때문입니다.

### 1. Version 18로 업그레이드 하는 법

React 18부터 react-dom의 render 함수는 deprecated 됩니다.
따라서 아래 코드로만 변경하면 버전 18을 사용할 수 있습니다.

```js
// ~React 17 (기존 코드)
ReactDOM.render(<App />, container);
```

```js
// React 18
const root = ReactDOM.createRoot(container);
root.render(<App />);
```

### 2. version 18 features

#### 2.1. 자동 배치 Automatic Batching

배치란, 리액트가 더 나은 성능을 위해 여러 개의 상태 업데이트를 한 번의 리렌더링(re-render)으로 묶는 작업을 뜻합니다.

```js
function App() {
  const [count, setCount] = useState(0);
  const [flag, setFlag] = useState(false);

  function handleClick() {
    setCount((c) => c + 1); // 아직 리렌더링 되지 않습니다.
    setFlag((f) => !f); // 아직 리렌더링 되지 않습니다.
    // 리액트는 오직 마지막에만 리렌더링을 한 번 수행합니다. (배치 적용)
  }

  return (
    <div>
      <button onClick={handleClick}>Next</button>
      <h1 style={{ color: flag ? "blue" : "black" }}>{count}</h1>
    </div>
  );
}
```

<br />
위의 handleClick 이벤트 핸들러 함수에서는 상태 업데이트를 두 번 (setCount, setFlag) 수행했습니다.
하지만, 리액트는 언제나 배치를 수행하여 이 두 번의 상태 업데이트를 한 번의 리렌더링으로 처리합니다.

이를 통해 불필요한 여러 번의 리렌더링을 방지하고 의도치 않은 버그를 예방할 수 있습니다.

하지만 배치가 수행되지 않는 예외가 존재합니다.

```js
function App() {
  const [count, setCount] = useState(0);
  const [flag, setFlag] = useState(false);

  function handleClick() {
    fetchSomething().then(() => {
      // 리액트 17 및 그 이전 버전에서는 배치가 수행되지 않습니다. 왜냐하면
      // 이 코드들은 이벤트 이후의 콜백에서 실행되기 때문입니다.
      setCount((c) => c + 1); // 리렌더링
      setFlag((f) => !f); // 리렌더링
    });
  }

  return (
    <div>
      <button onClick={handleClick}>Next</button>
      <h1 style={{ color: flag ? "blue" : "black" }}>{count}</h1>
    </div>
  );
}
```

React 17 이전의 기존 리액트에서는 위와 같이 이벤트 핸들러 함수 내에서 실행되는 상태 업데이트가 아닌 경우 배치가 동작하지 않았습니다.

하지만 React 18부터 자동 배치(Automatic Batching)라는 것이 추가되었습니다.

자동 배치란, 위와 같이 일반적인 이벤트 핸들러 함수 스코프에서 상태 업데이트가 발생하지 않더라도 자동으로 배치를 적용해주는 것을 뜻합니다.

```js
setTimeout(() => {
  setCount((c) => c + 1);
  setFlag((f) => !f);
  // 리액트는 오직 마지막에만 리렌더링을 한 번 수행합니다. (배치 적용)
}, 1000);

fetch(/*...*/).then(() => {
  setCount((c) => c + 1);
  setFlag((f) => !f);
  // 리액트는 오직 마지막에만 리렌더링을 한 번 수행합니다. (배치 적용)
});

elm.addEventListener("click", () => {
  setCount((c) => c + 1);
  setFlag((f) => !f);
  // 리액트는 오직 마지막에만 리렌더링을 한 번 수행합니다. (배치 적용)
});
```

#### 2.2. Transitions

#### 2.3. New Suspense Features

#### 2.4. New Client and Server Rendering APIs

#### 2.5. New Strict Mode Behaviors

<br />

#### 2.6. New Hooks

##### 2.6.1. useId

useId 훅은 서버와 클라이언트 모두에서 사용하기에 안전한 고유의 id를 생성하여 데이터 주입 시 에러가 발생하지 않도록 하는 훅입니다.

하지만 해당 아이디를 CSS 셀렉터로 사용하거나 돔 Api 에서 사용할 수 없습니다. (예: querySelectorAll )

```js
// 이렇게 만듭니다.

const id = useId();
```

```js
// 단일 사용의 경우

function Checkbox() {
  const id = useId();

  return (
    <>
      <label htmlFor={id}>Do you like React?</label>
      <input id={id} type="checkbox" name="react" />
    </>
  );
}
```

```js
// 한 컴포넌트에서 여러 번 사용할 경우

function NameFields() {
  const id = useId();
  return (
    <div>
      <label htmlFor={id + "-firstName"}>First Name</label>
      <div>
        <input id={id + "-firstName"} type="text" />
      </div>
      <label htmlFor={id + "-lastName"}>Last Name</label>
      <div>
        <input id={id + "-lastName"} type="text" />
      </div>
    </div>
  );
}
```

##### 2.6.2. useTransition

useTransition 과 startTransition 훅은 해당 스테이트가 urgent한 스테이트 값이 아님을 나타내도록 도와주는 훅입니다. (디폴트는 urgent)

```js
// 이렇게 만듭니다.

const [isPending, startTransition] = useTransition();
```

```js
function App() {
  const [isPending, startTransition] = useTransition();
  const [count, setCount] = useState(0);

  function handleClick() {
    startTransition(() => {
      setCount((c) => c + 1);
    });
  }

  return (
    <div>
      {isPending && <Spinner />}
      <button onClick={handleClick}>{count}</button>
    </div>
  );
}
```

##### 2.6.3. useDeferredValue

##### 2.6.4. useSyncExternalStore

##### 2.6.5. useInsertionEffect

#### 3. 지원 중단하는 기능들

- **react-dom**: ReactDOM.render더 이상 사용되지 않습니다. 그것을 사용하면 경고하고 React 17 모드에서 앱을 실행합니다.
- **react-dom**: ReactDOM.hydrate더 이상 사용되지 않습니다. 그것을 사용하면 경고하고 React 17 모드에서 앱을 실행합니다.
- **react-dom**: ReactDOM.unmountComponentAtNode더 이상 사용되지 않습니다.
- **react-dom**: ReactDOM.renderSubtreeIntoContainer더 이상 사용되지 않습니다.
- **react-dom/server**: ReactDOMServer.renderToNodeStream더 이상 사용되지 않습니다.

## 요약

- React 18의 알파 버전이 출시되었습니다. (정식 릴리즈는 아직입니다.)
- 자동 배치(Automatic Batching)가 도입되어 배치가 개선되었습니다.
- startTransition 이나 선택적(Selective) Hydartion 등의 동시성(Concurrent) 기능이 추가되었습니다.
- 새로운 서버 사이드 렌더링 아키텍처가 도입되었습니다. 이 아키텍처에서는`<Suspense>` 와 React.lazy 를 지원하며, 기존 서버 사이드 렌더링의 근본적인 문제를 해결했습니다.
- ReactDOM.render() 가 Deprecated 되고 새롭게 ReactDOM.createRoot() API가 그 자리를 이어갑니다.

## 참고

- [React 공식 사이트](https//reactjs.org/blog/2022/03/29/react-v18.html)
- [React Conf 2021](https://www.youtube.com/watch?v=FZ0cG47msEk)
- [최철헌님의 미디엄](https://medium.com/naver-place-dev/react-18%EC%9D%84-%EC%A4%80%EB%B9%84%ED%95%98%EC%84%B8%EC%9A%94-8603c36ddb25)
- [노마드코더 유튜브](https://www.youtube.com/watch?v=7mkQi0TlJQo)
- [Academind 유튜브](https://www.youtube.com/watch?v=N0DhCV_-Qbg)
- [Codevolution 유튜브](https://www.youtube.com/watch?v=BN4pVo7ectA)

- [벨로그](https://velog.io/@jay/React-18-%EB%B3%80%EA%B2%BD%EC%A0%90)
- []()
- []()
- []()
