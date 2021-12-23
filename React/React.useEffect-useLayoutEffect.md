# React - useEffect vs useLayoutEffect

## 설명

useEffect와 useLayoutEffect의 차이에 대한 내용입니다.
useEffect와 useLayoutEffect가 궁극적으로 하는 역할은 같습니다.
다만 상황에 따라 useLayoutEffect가 useEffect보다 더 적합한 경우가 있는데 이 경우를 알아보겠습니다.

### 1. useEffect

함수형 컴포넌트와 Hooks의 조합에서 클래스형 컴포넌트에서의 componentDidMount, componentDidUpdate 와 componentWillUnmount의 역할을 제공 해줍니다.

물론 100% 완벽하게 똑같지는 않습니다. componentDidMount, componentDidUpdate와는 다르게 useEffect에 전달된 함수는 화면에 렌더링이 완료된 후에 수행되게 됩니다. 이는 대부분의 작업이 브라우저에서 화면을 업데이트하는 것을 차단해서는 안되기 때문입니다. 실제로 이러한 동작 방식은 웬만한 경우에 더 효율적으로 작용할 것입니다.

### 2. useLayoutEffect

사용자에게 노출되는 DOM을 변경하는 경우에는 화면이 렌더링 되기 전에 동기화를 시켜주어야 합니다.
그렇지 않으면 화면에 렌더링이 되고난 후에 변경이 되기 때문에 사용자가 보는 화면이 깜빡일 것이기 때문입니다.

이렇게 DOM을 변경시키는 작업을 할 때는 useEffect보다 useLayoutEffect를 사용하는 것이 더 효율적입니다.

useLayoutEffect는 앞서 언급한 바와 같이 DOM이 변경되고나서 동기적으로 실행이 됩니다. 즉 브라우저가 화면을 그리기 전에 실행이됩니다. 그렇기 때문에 useLayoutEffect는 스크롤 위치를 얻어오거나 다른 DOM 엘리먼트의 스타일을 조작할 때 사용할 때 효율적입니다. 이는 클래스형 컴포넌트에서 componentDidMount와 componentDidUpdate와 동일한 역할을 제공합니다.

정리하자면, useLayoutEffect내의 함수는 DOM이 업데이트 되면 바로 동기적으로 실행이 되고 이러한 과정은 브라우저가 화면에 렌더링하기 이전에 수행됩니다. 따라서 사용자는 업데이트 되기 전의 화면을 보지 않게 됩니다.

## 요약

useLayoutEffect는 사용자에게 노출되는 DOM을 변형시킬 때 사용하면 더 효율적입니다.

useEffect는 DOM을 변형시키지 않는 대부분의 경우에 사용합니다.

## 참고

- [kentcdodds's blog](https://kentcdodds.com/blog/useeffect-vs-uselayouteffect)
- [blog](https://gingerkang.tistory.com/108)
