# React - React-rendering optimization

## 설명

### 클래스형 컴포넌트

일반적인 리액트 '클래스 컴포넌트'는 React.Component를 이용하여 정의한다.
그러나 React.Component는 setState가 호출될 때 마다 필요없는 리렌더링을 발생시키기도 한다.
이 때 비효율적인 렌더링을 제어하기 위해

- 컴포넌트의 업데이트 여부를 주시하는 함수 (shouldComponentUpdate()) 혹은
- React.PureComponent를 이용할 수 있다.

- 리액트 클래스 컴포넌트에서 사용하는 기본형인 React.Component와 React.PureComponent의 공통점과 차이점에 대해 배우고
- 리액트의 비효율적인 렌더링을 제어하기 위한 방법에 대해서 알아보려고 한다.

**1. React.Component**

- (공통점)ES6 class를 사용하여 React 컴포넌트를 정의할 때 기초가 되는 class이다.
- shouldComponentUpdate를 따로 설정하지 않을 시 항상 true를 반환한다
  - setState 실행 시 무조건적으로 컴포넌트가 업데이트된다

**2. React.PureComponent**

- (공통점)ES6 class를 사용하여 React 컴포넌트를 정의할 때 기초가 되는 class이다.
- shouldComponentUpdate를 구현하지는 않지만, props와 state를 이용한 얕은 비교(shallow compare)을 시행한다
  - 얕은 비교에서 변경사항이 있을 시 : true => rendering 발생
  - 얕은 비교에서 변경사항이 없을 시 : false == rendering이 발생하지 않는다
- 결국, setState가 실행되어도 얕은 비교 끝에 변경사항이 없다고 판단되면 컴포넌트가 업데이트되지 않는다.

```
class SomeComponent extends PureComponent {
   render(){
      return(
         <h1>PureComponent 이용하기🤍</h1>
      )
   }
}

```

- 얕은 비교

* 변수 / 문자열은 값을 비교한다
* 객체에서는 reference 값을 비교한다
  객체 내부 데이터의 변경과는 관계 없이 동일한 레퍼런스를 가지고 있는 객체는 같다고 취급한다.

**3. shouldComponentUpdate()**

- 클래스형 컴포넌트에서 리렌더링 여부를 결정할 수 있는 메소드.

* React.memo() 또한 사용할 수 있다. 함수형 컴포넌트에서 자세히 설명한다

**결론**
React.Component만 이용하게 되면 컴포넌트가 업데이트될 때마다 관련된 컴포넌튼들의 리렌더링이 발생하고, 컴포넌트 라이프 사이클 메소드가 실행된다.
그 때 데이터 변경이 없는 컴포넌트들도 함께 렌더링되기 때문에 성능 악화를 가져올 수 있다.

따라서 class 컴포넌트에서는 React.PureComponent를 이용한 '얕은 비교'를 통해 렌더링 관리를 수행하는 것이 좋다.

그러나 복잡한 객체의 변화가 발생한 경우는 shouldComponentUpdate() 등 다른 방식으로 렌더링을 최적화시켜주는 것이 좋다.
'얕은 비교'에서 잡아내지 못한 범위들을 커스터마이징해서 잡아낼 수 있기 때문이다.

### 함수형 컴포넌트

1. useMemo()
   함수의 결과값 기억
   - 함수를 계산한 값을 메모리에 저장하고, 함수의 호출이 발생했을 시 메모리에 저장된 값과 새로이 계산된 값을 비교한다.
   - 함수의 결과값이 같을 때 전에 저장된 값을 재사용하기 때문에 불필요한 계산을 야기하지 않는다.

```
const count = useMemo(() => addNumber(num), [num])
```

2. useCallback()
   함수 기억
   - useMemo()와 유사하다
   - 하지만 useMemo()가 함수의 결과값을 재사용할 때 사용하지만 useCallback은 특정 '함수 자체'를 재사용하고 싶을 때 사용한다
   - 첫번째 인자에 함수, 두번 째 인자에 props에서 사용하는 배열을 넣는다.
   - 해당 컴포넌트가 렌더링되더라도 함수가 의존하는 값 (예시에서는 x,y)이 바뀌지 않으면 기존 함수를 계속 제사용한다.
   - 값이 변했을 경우 새로운 함수가 생성되어 변수(예시에서는 add)에 할당된다.

```
const add = useCallback(() => x+y, [x,y])
```

3. React.memo()
   컴포넌트 기억
   - 컴포넌트를 React.memo로 감싸면 pureComponent로 만들어 준다.
   - 자주 렌더링되는 컴포넌트에 사용해야 이점이 있다.
   - 똑같은 prop이 계속 렌더링되고 UI가 큰 사이즈의 컴포넌트일 때 사용하면 좋다.

방법 1

```
import {memo} from "react";

const ShowToDoSet = memo(() => {
    const [atomGoals, setAtomGoals] = useRecoilState(myProgress);
     const onClick = (e: React.MouseEvent<HTMLDivElement>) => {
         .
         .
         .

```

방법 2

```
export default React.memo(ShowToDoSet);

```

**결론**
memoization이 일어나길 바라는 부분 (함수 / 함수의 값 / 컴포넌트)에 맞춰 최적화를 시행할 수 있다.

## 요약

React를 class로 사용할 때와 함수형으로 사용할 때 다른 최적화 방안을 적절히 도입해야 한다.
그러나 모든 부분에서 최적화를 도입하는 것은 옳지 않다.

1. 최적화를 위해 쓰이는 memoization 기법은 최적화가 필요 없거나 계속 다른 값으로 바뀌는 컴포넌트(또는 함수)에 사용했을 때 불필요한 정보를 계속해서 비교하게 된다. 이는 경제적이지 않다
2. 얕은 비교(shallow compare)이 일어난다는 것을 잘 알고 있어야 한다.
   - 복잡한 객체의 변화를 잡아내지 못할 수도 있다(reference를 비교하기 때문에)

따라서 얕은 비교로 잡아내지 못할 때는 shouldComponentUpdate등을 잘 활용해 주어야 한다

## 참고

- [React 공식문서](https://ko.reactjs.org/docs/react-api.html#reactcomponent)
- [React Hooks: useCallback 사용법](https://www.daleseo.com/react-hooks-use-callback/)
- [18. useCallback 을 사용하여 함수 재사용하기](https://react.vlpt.us/basic/18-useCallback.html)
- [React.memo() 적절하게 사용하기](https://pottatt0.tistory.com/73)
