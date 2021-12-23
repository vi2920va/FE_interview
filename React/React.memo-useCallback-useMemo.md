# React - React.memo, useMemo, useCallback

## 설명

### 1. React.memo()

- 상위 컴포넌트에서 변경이 일어났는데 하위컴포넌트는 상위 컴포넌트에서 받는 `props` 변경이 전혀일어나지 않는 경우 이 기능을 입하면 하위컴포넌트는 재실행되지 않습니다. 그러면 그 하위컴포넌트 아래의 트리구조에 달려있는 컴포넌트들도 재실행이 되지 않습니다.
- `React.memo`는 분명히 props를 비교하는 비용이 생겨납니다. 최 하위 컴포넌트에서의 과한 `React.memo`의 사용은 의미가 없습니다.
- `React.memo`는 컴포넌트를 반환하는 함수입니다. 따라서 컴포넌트를 wrapping하는 형태로 사용합니다.

```jsx
export default React.memo(Demo);
```

### 2. useCallback

- 컴포넌트가 비교를 위해 재실행 될때 함수는 참조값이라서 `React.memo`가 불가능하다면 함수자체를 저장(보존)해놨다가 비교가 가능하게 만들면 되지 않을까? 이러한 의문에서 탄생한 것이 useCallback입니다.
- 이 훅을 이용해서 컴포넌트가 재실행될때 함수객체를 다시 생성하지않고 이전에 보존된 함수를 다시 불러올수 있습니다.
- 이러한 개념을 이용해서 함수를 React 구성요소 어딘가에 저장해놨다가(클로저) 함수가 재실행될때 저장해놓은 요소를 가르키게 함으로서 함수를 보존합니다.
- `React.memo` 기능은 `props`로 넘어가는 함수객체의 비교를 할수 없기에 이를 보조하는 기능으로 함수객체를 보존함으로서 `React.memo`로 인한 컴포넌트 재실행 방지 기능을 활성화 시키기 위해 `useCallback`을 사용합니다.

```jsx
const func = useCallback(callback, [...dependencyArr]);
```

### 3. useMemo

- `useMemo`는 저장하려는 모든 종류의 데이터를 저장할 수 있습니다.
- `prop`이 고정되는 static 데이터나 잘 변하지 않는 종류의 데이터라면 `useMemo`를 사용한다면 컴포넌트가 재실행되도 이전값과 동일하기 때문에 `React.memo`에 의해 자식 컴포넌트가 재실행되지 않습니다.

```jsx
const func = useMemo(callback, [...dependencyArr]);
```

## 요약

- React.memo : 컴포넌트의 props를 이전 값과 비교해서 변경되지 않으면 컴포넌트를 재실행하지 않습니다.
- useCallback : 콜백함수를 저장해서 종속성배열의 값들이 변하지 않는 경우 이전 함수를 그대로 호출해서 함수가 재생성되어 props로 넘어갈때 컴포넌트가 재실행되는 것을 방지합니다. 즉, React.memo의 기능을 보조합니다.
- useMemo : 콜백함수가 반환하는 값을 저장해서 종속성배열의 값들이 변하지 않는경우 이전 값을 그대로 불러와 계속해서 값이 재생성되어 props로 넘어갈때 컴포넌트가 재실행되는것을 방지합니다. 즉, React.memo의 기능을 보조합니다.

## 참고

- [Udemy 강의](http://https://www.udemy.com/course/react-the-complete-guide-incl-redux/learn/lecture/25599684#content)
