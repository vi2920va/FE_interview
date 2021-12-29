# JavaScript - Closure example

## 설명

클로저를 활용하는 예시들을 더 구체적으로 알아봅니다.

### 1. 콜백 함수 내부에서 외부 데이터를 사용하고자 할 때

### 2. 접근 권한 제어(정보 은닉)

자바스크립트는 기본적으로 변수 자체에 접근 권한을 직접 부여하도록 설계되어 있지 않습니다. 하지만 클로저를 이용하면 함수 차원에서 public한 값과 private한 값을 구분하는 것이 가능합니다.

#### 2.1 정보 은닉역할로서 사용되는 클로저 예시

정보 은닉역할로서 사용되는 클로저의 대표적인 예로는 리액트의 useState 훅이 있습니다.

```javascript{ .numberLines}
// 출처: Humanscape Tech Blog

function useState(initVal) {
  let _val = initVal;
  const state = () => _val;
  const setState = newVal => {
    _val = newVal;
  };

  return [state, setState];
}

const [count, setCount] = useState(1);
console.log(count()); // 1

setCount(2);
console.log(count()); // 2

```

useState함수에서 리턴한 state값을 통해서만 \_val 변수에 접근 가능하게 하고, useState함수에서 리턴한 setState값을 통해서만 \_val 변수를 변경할 수 있습니다.

### 3. 부분 적용 함수

부분 적용 함수란 n개의 인자를 받는 함수에 미리 m개의 인자만 넘겨 기억시켰다가, 나중에 (n-m)개의 인자를 넘기면 비로소 원래 함수의 실행 결과를 얻을 수 있게끔 하는 함수입니다.

부분 적용 함수의 대표적인 예로 디바운스가 있습니다.
디바운스는 짧은 시간 동안 동일한 이벤트가 많이 발생할 경우 이를 전부 처리하지 않고 처음 또는 마지막에 발생한 이벤트에 대해 한 번만 처리하는 것으로, 프론트엔드 성능 최적화에 큰 도움을 주는 기능 중 하나입니다. scroll, wheel, mousemove, resize 등에 적용하기 좋습니다.

### 4. 커링 함수

커링 함수란 여러 개의 인자를 받는 함수를 하나의 인자만 받는 함수로 나눠서 순차적으로 호출될 수 있게 체인 형태로 구성한 것을 말합니다.

```javascript{ .numberLines}
// 변환 전
f(a, b, c)


// 변환 후
f(a)(b)(c)
```

커링 함수는 마지막 인자가 들어올 때까지 함수를 실행하지 않습니다. 이를 함수형 프로그래밍에서는 지연실행이라고 칭합니다.

```javascript{ .numberLines}
// 출처: https://ko.javascript.info

function curry(f) { // 커링 변환을 하는 curry(f) 함수
  return function(a) {
    return function(b) {
      return f(a, b);
    };
  };
}

// usage
function sum(a, b) {
  return a + b;
}

let curriedSum = curry(sum);

alert( curriedSum(1)(2) ); // 3
```

#### 4.1 커링 함수 사용 예시

정보를 형식화하고 출력하는 로그 함수 log (date, importance, message)가 있다고 가정해 보겠습니다. 실제 프로젝트에서 이러한 함수는 네트워크를 통해 로그를 보내는 것과 같은 많은 유용한 기능을 제공합니다. 여기서는 alert 창 을 띄우는 것으로 해보겠습니다.

예시 출처: https://ko.javascript.info

```javascript{ .numberLines}
function log(date, importance, message) {
  alert(`[${date.getHours()}:${date.getMinutes()}] [${importance}] ${message}`);
}
```

커링을 적용해보겠습니다.

```javascript{ .numberLines}
// lodash 라이브러리의 _.carry 사용

log = _.curry(log);
```

위 로그를 아래처럼 현재 시간으로 로그를 출력하는데 편리하도록 log 함수를 작성해서 사용할 수 있습니다.

```javascript{ .numberLines}
// logNow 는 log 의 첫 번째 인수가 고정된 partial이 될 것입니다.
let logNow = log(new Date());

// use it
logNow("INFO", "message"); // [HH:mm] INFO message
```

이제 logNow 는 log의 첫 번째 인수를 고정한 것입니다. 다른 말로 하면 “partially 적용된 함수” 또는 짧게 하면 “partial” 입니다.

이제 더 나아가서 디버깅 로그를 편리하게 하는 함수를 만들어 보겠습니다.

```javascript{ .numberLines}
let debugNow = logNow("DEBUG");

debugNow("message"); // [HH:mm] DEBUG 메세지
```

<!-- ```javascript{ .numberLines}

``` -->

## 요약

## 참고

- [코어자바스크립트](http://www.yes24.com/Product/Goods/78586788)
- [Humanscape Tech Blog](https://medium.com/humanscape-tech/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%ED%81%B4%EB%A1%9C%EC%A0%80%EB%A1%9C-hooks%EA%B5%AC%ED%98%84%ED%95%98%EA%B8%B0-3ba74e11fda7)
- [javacript.info](https://ko.javascript.info/currying-partials)
- []()
- []()
