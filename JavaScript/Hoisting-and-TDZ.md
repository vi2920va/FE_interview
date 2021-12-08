# JavaScript - Hoisting, TDZ

## 설명

호이스팅이란 자바스크립트 엔진 동작을 이해하기 쉽게 하기 위해 만들어진 개념으로, 변수 및 함수 선언이 유효 스코프 최상단으로 끌어올려진 후에 코드를 실행하는 것으로 생각하는 것 입니다.

### 1. Hoisting

- 인터프리터가 변수와 함수의 메모리 공간을 선언 전에 미리 할당하는 것을 의미합니다.
- var로 선언한 변수의 경우 호이스팅 시 undefined로 변수를 초기화합니다.
- let과 const로 선언한 변수의 경우 초기화 전에 접근을 시도하면 ReferenceError가 발생합니다(var의 경우 선언 전에 접근할 시 undefined입니다).

### 2. TDZ

- 초기화되지 않은 변수가 있는 곳을 Temporal Dead Zone이라고 합니다.
- 변수가 초기화되는 순간 TDZ에서 나오게 되며 사용할 수 있게 됩니다.
- TDZ에 영향을 받는 구문은 크게

  - const 변수
  - let 변수
  - class 구문
  - constructor() 내부의 super()
  - 기본 함수 매개변수 가 있습니다.

- 예시 1

```
// Does not work!
count; // throws `ReferenceError`
let count;

count = 10;
```

- 예시 2

```
// 다시, let 변수도 선언 이후에 사용해야 한다.


let count;

// Works!
count; // => undefined
count = 10;

// Works!
count; // => 10
```

## 요약

호이스팅이란 자바스크립트 엔진 동작을 이해하기 쉽게 하기 위해 만들어진 개념으로, 변수 및 함수 선언이 유효 스코프 최상단으로 끌어올려진 후에 코드를 실행하는 것으로 생각하는 것입니다.

var , let , const 키워드로 선언된 변수는 선언 부분만 끌어올려진다고 생각할 수 있습니다. 따라서 var 의 경우에는 변수 선언전에도 참조할 수 있지만 할당을 하지 않았기에 undefined 입니다. 그러나 let 과 const 의 경우 식별자가 호이스팅 후 실제 코드에서 선언되기 전까지 TDZ에 있기 때문에 let , const 선언 코드가 있는 곳 이전에는 해당 변수에 참조할 수 없게 됩니다.

## 참고

- [MDN](https://developer.mozilla.org/ko/docs/Glossary/Hoisting)
- [MDN](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Statements/let)
- [TOAST UI](https://ui.toast.com/weekly-pick/ko_20191014)
- [velog](https://velog.io/@open_h/Hoisting-and-TDZ#hoisting%ED%98%B8%EC%9D%B4%EC%8A%A4%ED%8C%85)
- [blog](https://noah0316.github.io/JavaScript/2020-11-04-temporal-dead-zone,-hoisting%EC%97%90-%EA%B4%80%ED%95%98%EC%97%AC/)
