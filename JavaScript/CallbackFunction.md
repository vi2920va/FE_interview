# JavaScript - Callback Fucntion

## 설명

콜백함수는 다른 코드의 인자로 넘겨주는 함수입니다.

### 제어권

콜백함수는 어떤 함수 X를 호출하면서 '특정 조건일 때 함수 Y를 실행해서 나에게 알려달라'는 요청을 함께 보내는 것입니다.

함수 X가 자체적인 내부로직을 통해 위임받은 코드를 적절한 시점에 실행하는 개념이 콜백함수이므로 콜백함수는 제어권과 관련이 깊습니다.

#### 1. 호출 시점

호출 시점에 대한 예시로는 setInterval을 들 수 있습니다.

```jsx
var count = 0;
var cb = function () {
  console.log(count);
  if (++count > 4) clearInterval(timer);
};
var timer = setInterval(cb, 300);
```

setInterval이라고 하는 '다른 코드'에 첫 번재 인자로서 cb함수를 넘겨주자 제어권을 넘겨받은 setInterval이 스스로의 판단에 따라 적절한 시점에(0.3초마다) 이 익명함수를 실행했습니다.

이처럼 콜백함수의 제어권을 넘겨받은 코드는 콜백함수 호출 시점에 대한 제어권을 가집니다.

#### 2. 인자

콜백함수의 제어권을 넘겨받은 코드는 콜백함수를 호출할 때 인자에 어떤 값들을 어떤 순서로 넘길 것인지에 대한 제어권을 가집니다.

예컨대 map함수를 쓸 경우에 무조건 현재 값, 인덱스 순서로 인자를 받습니다.

```jsx
var newArr = [10, 20, 30].map(function (currentValue, index) {
  console.log(currentValue, index);
  return currentValue + 5;
});

// 실행 결과
// 10 0
// 20 1
// 30 2
// [15, 25, 35]
```

#### 3. this

콜백함수도 함수이기 때문에 기본적으로는 this가 전역객체를 참조하지만, 제어권을 넘겨받을 코드에서 콜백함수에 별도로 this가 될 대상을 지정한 경우에는 그 대상을 참조하게 됩니다.

### 콜백함수는 함수다.

콜백함수로 어떤 객체의 메서드를 전달하더라도 그 메서드는 메서드가 아닌 함수로서 호출됩니다.
따라서 객체의 메서드를 콜백함수로 전달하면 해당 객체를 this로 바라볼 수 없게 됩니다.

### this 바인딩

콜백함수 내부의 this에 다른 값을 바인딩하는 방법에는 여러 방법이 있습니다.
여러 방안 중 최신의 bind를 사용하는 방안을 사용하면 기존에 사용하던 방법의 아쉬운 점들을 보완할 수 있습니다 (번거로움, 메모리 낭비 등).

```jsx
var obj1 = {
  name: "obj1",
  func: function () {
    console.log(this.name);
  },
};

setTimeout(obj1.func.bind(obj1), 1000);

var obj2 = { name: "obj2" };
setTimeout(obj1.func.bind(obj2), 1500);
```

### 콜백 지옥과 비동기 제어

콜백 지옥은 콜백함수를 익명함수로 전달하는 과정이 반복되어 코드의 들여쓰기 수준이 감당하기 어려울 정도로 깊어지는 현상입니다.

비동기적인 코드란 별도의 요청, 실행 대기, 보류 등과 관련한 코드를 의미합니다.

보통 서버 통신이나 이벤트 처리 등의 비동기적인 작업에 콜백함수를 많이 작성하여 콜백 지옥에 빠지기 쉽기 때문에 두 개념을 함께 묶어서 설명하였습니다.

이렇게 비동기적인 작업에서 발생할 수 있는 콜백 지옥을 방지하고자 나온 것이 Promise, Generator, async/await입니다.

## 요약

- 콜백함수는 다른 코드에 인자로 넘겨줌으로써 그 제어권도 함께 위임한 함수입니다.
- 제어권을 넘겨받은 코드는 다음과 같은 제어권을 가집니다.
  - 콜백함수를 호출하는 시점을 스스로 판단해서 실행합니다.
  - 콜백함수를 호출할 때 인자로 넘겨줄 값들 및 그 순서가 정해져 있습니다. 이 순서를 따르지 않고 코드를 작성하면 엉뚱한 결과를 얻게 됩니다.
  - 콜백함수의 this가 무엇을 바라보도록 할지가 정해져 있는 경우도 있습니다. 정하지 않은 경우에는 전역객체를 바라봅니다. 사용자가 임의로 this를 바꾸고 싶은 경우 bind 메서드를 활용하면 됩니다.
- 어떤 함수에 인자로 메서드를 전달하더라도 이는 결국 함수로서 실행됩니다.
- 비동기 제어를 위해 콜백 함수를 사용하다 보면 콜백 지옥에 빠지기 쉽습니다. 최근의 ES에서는 Promise, Generator, async/await 등 콜백 지옥에서 벗어날 수 있는 훌륭한 방법들이 속속 등장하고 있습니다.

## 참고

- CallBack Function [→ (MDN)](https://developer.mozilla.org/en-US/docs/Glossary/Callback_function)
- 코어자바스크립트 책 [→ (SITE)](http://www.yes24.com/Product/Goods/78586788)
- 코어자바스크립트 인프런 강의 [→ (SITE)](https://www.inflearn.com/course/%ED%95%B5%EC%8B%AC%EA%B0%9C%EB%85%90-javascript-flow/dashboard)