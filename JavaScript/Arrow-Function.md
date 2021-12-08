# JavaScript - 화살표 함수(Arrow Function)

## 설명

- 화살표 함수(Arrow Function)는 함수의(Function)키워드를 간단히 사용할 수 있게 ES6+에서 추가된 문법입니다.

### 1. 화살표 함수의 this

> 일반 함수와 화살표 함수의 차이점 `this`키워드 입니다.

- 일반 함수에서 `this`는 함수의 호출 방식에서 달라집니다.
- 화살표 함수에서 `this`는 정의되지 않기 때문에 `this`에 접근하려고 하는 경우에 스코프  페인 가장 가까운 `this`에 접근하게 된다.

```javascript
const person = {
  name: 'Print', sayHi: () => console.log(`Hi ${this.name}`)              
}; 

person.sayHi(); // Hi undefined
```

### 2. 화살표 쓰면 안되는 경우

- 객체의 메서드(method)로 화살표 함수를 사용하게 되면 가까운 `this`를 찾기 때문에 `undefined`가 출력된다.

- protype에 할당할 경우에 위와 같은 이유로 `undefined`를 출력하게 된다.

- 생성자 함수(constructor function)에서 `this` 새롭게 만들어질 객체를 가리키지만, 화살표 함수를 생성자 함수로 사용하게 되면 `TypeError`가 발생하게 된다.

- `addEventListener`의 콜백함수에서 화살표 함수를 사용하면 `this`는 전역 객체 `window`를 가리키게 된다.

## 요약

즉, 화살표 함수는 짧은 함수를 작성하거나, *컨텍스트**가 없는 짧은 코드를 담을 용도로 만들어졌다.

## 참고
[자바스크립트 화살표 함수 도입](https://ui.toast.com/weekly-pick/ko_20160912)
[화살표함수(MDN)](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Functions/Arrow_functions)
