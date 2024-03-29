# React - useEffect

## 설명

- useEffect Hook은 dependency 배열 내에 지정된 값의 변화가 일어났을 때 이펙트 함수가 실행됩니다.
- 이러한 특성으로 인해 컴포넌트가 마운트(렌더링)될 때 API를 통해 데이터를 가져오거나, state값 또는 props 값이 변경될 때 특정 함수를 실행시키는 등의 작업을 하는 데 사용됩니다.
  - 마운트는 DOM 객체가 생성되고 브라우저에 나타나는 것을 의미합니다.
- useEffect Hook은 매 렌더링 후에 실행되며, 필요에 따라 cleanup 함수를 반환하도록 작성해 이펙트 실행 이전에 실행시킬 수 있습니다.

### 1. cleanup 함수

- cleanup 함수는 useEffect의 뒷처리 함수입니다.
- 컴포넌트가 사라질 때 특정 작업을 수행합니다.
- 언마운트 될 때만 cleanup 함수를 실행하고 싶으면 두번째 파라미터로 빈 배열을 넣습니다(unmount 이전).
- 특정 값이 업데이트 되기 직전에 cleanup 함수를 실행하고 싶을 때에는 deps 배열안에 검사하고 싶은 값을 넣어줍니다(update 이전).

### 2. useEffect() 사용법

- 컴포넌트가 화면에 가장 처음 렌더링 됐을 때만 한번 실행하고 싶을 때, deps 위치에 빈 배열을 넣습니다.
- 만약, 빈 배열을 생략한다면 리렌더링 될 때마다 실행됩니다.
- 특정 값이 업데이트 될 때, 실행하고 싶을 때는 deps 위치의 배열 안에 검사하고 싶은 값을 넣습니다.
- 업데이트 될 때뿐만 아니라 마운트 될 때에도 실행됩니다.

## 요약

- useEffect Hook을 사용하면 함수 컴포넌트에서 side effect를 수행할 수 있습니다.
