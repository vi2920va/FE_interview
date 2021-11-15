# React - Lifecycle

## 설명
모든 컴포넌트는 다양한 lifecycle method 를 가지며, 이를 오버라이딩하여 특정 시점에 코드 실행을 설정할 수 있음

![summary](https://p165.p3.n0.cdn.getcloudapp.com/items/12uPzbK1/6a18a046-9241-4e8a-a544-15e3072ff16a.jpg?source=viewer&v=25ec87188938b7e644b32a49fd7724d1)

* v16.3 기준 deprecated 된 함수는 다루지 않음

## 요약
### Component API
#### render
```js
constructor(props)

- 컴포넌트 생성 시점에 호출
- method binding 혹은 state 초기화 작업 시 사용
```
```js
static getDerivedStateFromProps(props, state)

- props, state 변경 시 render 직전 호출
- props에 state가 의존하는 경우 사용
```
```js
shouldComponentUpdate(nextProps, nextState)

- props, state 변경 시 render 직전 호출
- props, state 변경 시 렌더링 여부 결정하는데 사용
```
```js
render()
```

#### pre-commit
```js
getSnapshotBeforeUpdate(prevProps, prevState)

- 렌더링 결과가 DOM 에 반영되었을때 호출
```

#### commit
```js
componentDidMount()

- 컴포넌트 마운트 직후(paint) 호출
```
```js
componentDidUpdate(prevProps, prevState, snapshot)

- 컴포넌트 업데이트 직후 호출
```
```js
componentDidUpdate(prevProps, prevState, snapshot)

- 컴포넌트 마운트 제거 직전 호출
- event, subscribe, network request 등 해제하는데 사용
```

### Hooks
```js
useEffect()

- 컴포넌들이 render, paint 된 후 실행 (async)
- componentDidMount, componentDidUpdate, componentWillUnmount 의 기능 가능
```
```js
useLayoutEffect()

- 컴포넌트들이 render 된 후 실행, 이후에 paint (sync)
```
## 참고
- [State and Lifecycle](https://ko.reactjs.org/docs/state-and-lifecycle.html)
- [React.Component](https://ko.reactjs.org/docs/react-component.html)
