# React - Version 18 Summary

## 설명

이번 React 18의 정식 릴리즈는 앞으로 몇 달 뒤로 예정되어 있습니다. 하지만, 리액트 사용자 입장에서도 이번 React 18은 미리 알아두면 도움이 많이 됩니다. 지금까지의 웹 프론트엔드 개발에서 한 단계 진보하여 동시성 제어가 가능해지기 때문입니다. (글 작성 기준: 2022.04)

리액트 18에서 선보이는 가장 큰 특징은 동시성concurrency입니다.
하지만 concurrency라는 단어 하나로 말하고하 하는 바가 잘 와닿지 않습니다.

이를 풀어서 이야기하자면,

**동시적으로 업데이트해야하는 여러 UI 상태 중에 더 urgent(긴급하고 중요한)상태를 지정하여 화면을 더 부드럽고 멈춤없이 표현할 수 있는 것이 이번 리액트 18의 가장 큰 특징입니다.**

### 1. Version 18로 업그레이드 하는 법

React 18부터 react-dom의 render 함수는 deprecated 됩니다.
따라서 **index.js**에서 아래 코드로만 변경하면 버전 18을 사용할 수 있습니다.

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

Transitions은 긴급 업데이트와 긴급하지 않은 업데이트를 구분하기 위한 React의 새로운 개념입니다.

- **Urgent 긴급** 업데이트는 입력, 클릭, 누르기 등과 같은 직접적인 상호 작용을 반영합니다.
- **Transition 전환** 업데이트는 UI를 한 보기에서 다른 보기로 전환합니다.

일반적으로 최상의 사용자 경험을 위해서는 단일 사용자 입력으로 긴급 업데이트와 긴급하지 않은 업데이트가 모두 발생해야 합니다. 입력 이벤트 내에서 startTransition API를 사용하여 어떤 업데이트가 긴급하고 "Transition전환"인지 React에 알릴 수 있습니다.

```js
mport {startTransition} from 'react';

// Urgent: Show what was typed
setInputValue(input);

// Mark any state updates inside as transitions
startTransition(() => {
  // Transition: Show the results
  setSearchQuery(input);
});
```

startTransition에 래핑된 업데이트는 긴급하지 않은 것으로 처리되며 클릭이나 키 누름과 같은 더 긴급한 업데이트가 들어오는 경우 중단됩니다.

전환이 사용자에 의해 중단되면(예: 여러 문자를 연속으로 입력) React는 완료되지 않은 오래된 렌더링 작업을 제거하고 최신 업데이트만 렌더링합니다.

- useTransition: 보류 상태를 추적하는 값을 포함하여 전환을 시작하는 hook.
- startTransition: hook을 사용할 수 없을 때 트랜지션을 시작하는 메소드.
  전환은 동시 렌더링을 선택하여 업데이트를 중단할 수 있습니다. 콘텐츠가 다시 일시 중단되면 전환은 백그라운드에서 전환 콘텐츠를 렌더링하는 동안 현재 콘텐츠를 계속 표시하도록 React에 지시합니다(자세한 내용은 Suspense RFC 참조 ).

#### 2.3. New Suspense Features

Suspense를 사용하면 아직 표시할 준비가 되지 않은 경우 구성 요소 트리의 일부에 대한 로드 상태를 선언적으로 지정할 수 있습니다.

```js
<Suspense fallback={<Spinner />}>
  <Comments />
</Suspense>
```

<br />
Suspense는 버전 18에서 새로 등장한 개념은 아닙니다.
그러나 지원되는 유일한 사용 사례는 React.lazy를 사용한 코드 분할이었고 서버에서 렌더링할 때 전혀 지원되지 않았습니다.

React 18에서는 **서버에서 Suspense에 대한 지원**을 추가하고 **동시 렌더링** 기능을 사용하여 기능을 확장했습니다.

React 18의 Suspense는 **Transition전환 API와 결합될 때 가장 잘 작동합니다.** 전환 중에 일시 중단하면 React는 이미 보이는 콘텐츠가 대체 항목으로 대체되는 것을 방지합니다. 대신 React는 잘못된 로드 상태를 방지하기 위해 충분한 데이터가 로드될 때까지 렌더링을 지연합니다.

#### 2.4. New Client and Server Rendering APIs

React DOM Client와 React DOM Server에 대한 업데이트가 이루어졌습니다.

##### 2.4.1 React DOM Client

- createRoot: 기존의 ReactDOM.render 대신 사용할 메서드입니다.
  리액트 18에서 지원하는 기능들은 해당 메서드를 사용하지 않으면 사용할 수 없습니다.

- hydrateRoot: 기존의 ReactDOM.hydrate 대신 사용할 메서드입니다.

##### 2.4.2 React DOM Server

- renderToPipeableStream: Node 환경에서의 스트리밍을 지원합니다.
- renderToReadableStream: Deno와 Cloudflare와 같은 modern edge runtime environments을 위한 메서드입니다.

기존의 renderToString 는 계속 사용할 수는 있지만 사용을 권장하지 않습니다.

#### 2.5. New Strict Mode Behaviors

구성 요소가 여러 번 마운트하고 언마운트하는 부분에 문제 없이 작동하는 지 확인할 수 있도록 Strict Mode를 업데이트 하였습니다.

이는 추후에 reusable state 관련 기능을 업데이트할 때 화면이 원하는 대로 작동하는지를 체크하는데 필요할 것이라고 보았기 때문에 업데이트 되었습니다.

##### 2.5.1 버전 18 이전

구성 요소를 마운트하고 효과를 생성합니다.

```
* React mounts the component.
  * Layout effects are created.
  * Effects are created.
```

##### 2.5.2 버전 18 이후

구성 요소의 마운트 **해제 및 다시 마운트**를 시뮬레이션합니다.

```
* React mounts the component.
  * Layout effects are created.
  * Effects are created.
  // 여기까지는 동일합니다.
* React simulates unmounting the component.
  * Layout effects are destroyed.
  * Effects are destroyed.
* React simulates mounting the component with the previous state.
  * Layout effects are created.
  * Effects are created.
```

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

useTransition 과 startTransition 훅은 해당 스테이트가 긴급(urgent)한 스테이트 값이 아님을 나타내도록 도와주는 훅입니다. (디폴트는 urgent)

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

useDeferredValue는 트리의 긴급하지 않은 부분을 다시 렌더링하는 것을 연기할 수 있습니다.

디바운싱과 비슷하지만 그에 비해 몇 가지 장점이 있습니다. 고정된 시간 지연이 없으므로 React는 첫 번째 렌더링이 화면에 반영된 직후 지연된 렌더링을 시도합니다. 지연된 렌더링은 중단 가능하며 사용자 입력을 차단하지 않습니다.

```js
// 이렇게 만듭니다.

const deferredValue = useDeferredValue(value);
```

useDeferredValue전달한 값만 연기합니다. 긴급 업데이트 중에 자식 구성 요소가 다시 렌더링되는 것을 방지하려면 해당 구성 요소를 React.memo또는 로 메모화해야 합니다

```js
function Typeahead() {
  const query = useSearchQuery("");
  const deferredQuery = useDeferredValue(query);

  // Memoizing tells React to only re-render when deferredQuery changes,
  // not when query changes.
  const suggestions = useMemo(
    () => <SearchSuggestions query={deferredQuery} />,
    [deferredQuery]
  );

  return (
    <>
      <SearchInput query={query} />
      <Suspense fallback="Loading results...">{suggestions}</Suspense>
    </>
  );
}
```

##### 2.6.4. useSyncExternalStore

useSyncExternalStore는 외부 저장소가 저장소에 대한 업데이트를 동기식으로 강제하여 동시 읽기를 지원할 수 있도록 하는 새로운 hook입니다. 외부 데이터 소스에 대한 구독을 구현할 때 useEffect가 필요하지 않으며 React 외부의 상태와 통합되는 모든 라이브러리에 권장됩니다.
<br />
useDeferredValue 훅은 리액트 라이브러리 저자들을 위해 만들어졌습니다.
일반적인 어플리케이션 코드 작성에서는 사용을 지양하는 것이 좋습니다.

```js
// 이렇게 만듭니다.

const state = useSyncExternalStore(subscribe, getSnapshot[, getServerSnapshot]);
```

##### 2.6.5. useInsertionEffect

DOM mutations전에 동기적으로 동작한다는 점만 제외하고는 useEffect와 같은 역할을 한다고 보면 됩니다.

useLayoutEffect를 실행하기 전에 스타일을 돔에 주입하는 용도로 사용하면 됩니다.
다만 스코프가 제한된 훅이기 때문에 ref에 대한 접근과 스케쥴 업데이트가 불가능합니다.

<br />
useDeferredValue 훅은 css-in-js 라이브러리 저자들을 위해 만들어졌습니다.
일반적인 어플리케이션 코드 작성에서는 사용을 지양하는 것이 좋습니다.

<br />
<br />


```js
// 이렇게 만듭니다.

useInsertionEffect(didUpdate);
```

#### 3. 지원을 중단하는 기능들

- **react-dom**: ReactDOM.render를 더 이상 사용하지 않습니다. 그것을 사용하면 경고하고 React 17 모드에서 앱을 실행합니다.
- **react-dom**: ReactDOM.hydrate를 더 이상 사용하지 않습니다. 그것을 사용하면 경고하고 React 17 모드에서 앱을 실행합니다.
- **react-dom**: ReactDOM.unmountComponentAtNode를 더 이상 사용하지 않습니다.
- **react-dom**: ReactDOM.renderSubtreeIntoContainer를 더 이상 사용하지 않습니다.
- **react-dom/server**: ReactDOMServer.renderToNodeStream를 더 이상 사용하지 않습니다.

## 요약

동시적으로 업데이트해야하는 여러 UI 상태 중에 더 urgent(긴급하고 중요한)상태를 지정하여 화면을 더 부드럽고 멈춤없이 표현할 수 있는 것이 이번 리액트 18의 가장 큰 특징입니다.

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
- [useTransition() vs useDeferredValue | React 18](https://www.youtube.com/watch?v=lDukIAymutM)

