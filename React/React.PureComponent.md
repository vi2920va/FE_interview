# React - React.PureComponent

## 설명

### 1. React.Component

- (공통점)ES6 class를 사용하여 React 컴포넌트를 정의할 때 기초가 되는 class이다.
- shouldComponentUpdate를 따로 설정하지 않을 시 항상 true를 반환한다
  - setState 실행 시 무조건적으로 컴포넌트가 업데이트된다

### 2. React.PureComponent

- (공통점)ES6 class를 사용하여 React 컴포넌트를 정의할 때 기초가 되는 class이다.
- shouldComponentUpdate를 구현하지는 않지만, props와 state를 이용한 얕은 비교(shallow compare)을 시행한다
  - 얕은 비교에서 변경사항이 있을 시 : true => rendering 발생
  - 얕은 비교에서 변경사항이 없을 시 : false == rendering이 발생하지 않는다
- 결국, setState가 실행되어도 얕은 비교 끝에 변경사항이 없다고 판단되면 컴포넌트가 업데이트되지 않는다.

**얕은 비교**

- 변수 / 문자열은 값을 비교한다
- 객체에서는 reference 값을 비교한다
  객체 내부 데이터의 변경과는 관계 없이 동일한 레퍼런스를 가지고 있는 객체는 같다고 취급한다.

## 요약

React.Component만 이용하게 되면 컴포넌트가 업데이트될 때마다 관련된 컴포넌튼들의 리렌더링이 발생하고, 컴포넌트 라이프 사이클 메소드가 실행된다.
그 때 데이터 변경이 없는 컴포넌트들도 함께 렌더링되기 때문에 성능 악화를 가져올 수 있다.

따라서 React.PureComponent를 이용한 '얕은 비교'를 통해 렌더링 관리를 수행하는 것이 좋다.

그러나 복잡한 객체의 변화가 발생한 경우는 shouldComponentUpdate() 등 다른 방식으로 렌더링을 최적화시켜주는 것이 좋다.
'얕은 비교'에서 잡아내지 못한 범위들을 커스터마이징해서 잡아낼 수 있기 때문이다.

## 참고

-[React 공식문서](https://ko.reactjs.org/docs/react-api.html#reactcomponent)
