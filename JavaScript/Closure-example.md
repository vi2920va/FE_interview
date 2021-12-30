# JavaScript - Closure example

## 설명

클로저를 활용하는 예시들을 더 구체적으로 알아봅니다.

### 1. 콜백 함수 내부에서 외부 데이터를 사용하고자 할 때

대표적인 콜백 함수 중 하나인 이벤트 리스너 예시를 통해 알아보겠습니다.

```javascript{ .numberLines}
// 출처: 코어 자바스크립트

var fruits = ["apple", "banana", "peach"];
var $ul = document.createElement("ul");

fruits.forEach(function (fruit) { // (A)
    var $li = document.createElement("li");
    $li.innerText = fruit;
    $li.addEventListener("click", function(){ // (B)
        alert("your choice is "+ fruit);
    });
    $ul.appendChild($li);
});

document.body.appendChild($ul);
```

위 코드에서는 addEventListener에 넘겨준 콜백함수(B)가 fruit이라는 외부변수를 참조하고 있으므로 클로저가 존재합니다.

### 2. 접근 권한 제어(정보 은닉)

자바스크립트는 기본적으로 변수 자체에 접근 권한을 직접 부여하도록 설계되어 있지 않습니다. 하지만 클로저를 이용하면 함수 차원에서 public한 값과 private한 값을 구분하는 것이 가능합니다.

#### 2.1 정보 은닉역할로서 사용되는 클로저 예시

정보 은닉 역할로서 사용되는 클로저의 대표적인 예로는 리액트의 useState 훅이 있습니다.

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

클로저를 이용하여 useState함수에서 리턴한 state값을 통해서만 \_val 변수에 접근 가능하게 하고, useState함수에서 리턴한 setState값을 통해서만 \_val 변수를 변경할 수 있습니다(클로저를 이용하여 \_val 변수 은닉).

### 3. 부분 적용 함수

부분 적용 함수란 n개의 인자를 받는 함수에 미리 m개의 인자만 넘겨 기억시켰다가, 나중에 (n-m)개의 인자를 넘기면 비로소 원래 함수의 실행 결과를 얻을 수 있게끔 하는 함수입니다.

부분 적용 함수의 대표적인 예로 디바운스가 있습니다.

디바운스는 짧은 시간 동안 동일한 이벤트가 많이 발생할 경우 이를 전부 처리하지 않고 처음 또는 마지막에 발생한 이벤트에 대해 한 번만 처리하는 것으로, 프론트엔드 성능 최적화에 큰 도움을 주는 기능 중 하나입니다. scroll, wheel, mousemove, resize, 검색창 기능 등에 적용하기 좋습니다.

#### 3.1 부분 적용 함수 예시: 디바운스

```javascript{ .numberLines}
// 출처: 코어 자바스크립트

var debounce = funtion(eventName, func, wait){
    var timeoutId = null;
    return function(event){
        var self = this;
        console.log(eventName, 'event 발생');
        clearTimeout(timeoutId);
        timeoutId = setTimeout(this.bind(self, event), wait);
    };
};

var moveHandler = function(e){
    console.log('move event 처리');
}

var wheelHandler = function(e){
    console.log('wheel event 처리');
}

document.body.addEventListener("mousemove", debounce("move", moveHandler, 500));
document.body.addEventListener("mousewheel", debounce("wheel", wheelHandler, 700));

```

위의 코드에서 최초 event가 발생하면 9번째 줄에 의해 timeout의 대기열에 'wait 시간 뒤에 func를 실행할 것'이라는 내용이 담깁니다. 그런데 wait 시간이 경과하기 이전에 다시 동일한 이벤트가 발생하면 이번에는 8번째 줄에 의해 앞서 저장했던 대기열을 초기화하고 다시 9번째 줄에서 새로운 대기열을 등록합니다. 결국 각 이벤트가 바로 이전 이벤트로부터 wait 시간 이내에 발생하는 한 마지막에 발생한 이벤트만이 초기화되지 않고 무사히 실행될 것입니다.

참고로 위의 예제에서 클로저로 처리되는 변수에는 eventName, func, wait, timeoutId가 있습니다.

### 4. 커링 함수

커링 함수란 여러 개의 인자를 받는 함수를 하나의 인자만 받는 함수로 나눠서 순차적으로 호출될 수 있게 체인 형태로 구성한 것을 말합니다.

```javascript{ .numberLines}
// 변환 전
f(a, b, c)


// 변환 후
f(a)(b)(c)
```

커링 함수는 마지막 인자가 들어올 때까지 함수를 실행하지 않습니다. 이를 함수형 프로그래밍에서는 지연실행이라고 칭합니다.

위의 부분적용 함수와는 아래와 같은 차이점이 있습니다.

1. 커링은 한 번에 하나의 인자를 전달하는 것을 원칙으로 합니다.
2. 중간 과정상의 함수를 실행한 결과는 그 다음 인자를 받기 위해 대기만 할 뿐이므로, 마지막 인자가 전달되기 전까지는 원본 함수가 실행되지 않습니다
   (부분 적용 함수는 여러 개의 인자를 전달 할 수 있고, 실행 결과를 재실행할 때는 원본 함수가 무조건 실행됩니다).

#### 4.1 커링 함수 사용 예시

##### 예시 1: sum

```javascript{ .numberLines}
// 출처: https://ko.javascript.info

// 커링 변환을 하는 curry(f) 함수
function curry(f) {
  return function(a) {
    return function(b) {
      return f(a, b);
    };
  };
}

// 사용 예시
function sum(a, b) {
  return a + b;
}

let curriedSum = curry(sum);

alert( curriedSum(1)(2) ); // 3
```

##### 예시 2: log

정보를 형식화하고 출력하는 로그 함수 log (date, importance, message)가 있다고 가정해 보겠습니다. 실제 프로젝트에서 이러한 함수는 네트워크를 통해 로그를 보내는 것과 같은 많은 유용한 기능을 제공합니다. 아래 예시에서는 alert 창 을 띄우는 것으로 해보겠습니다.

예시2 출처: https://ko.javascript.info

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

이제 date와 로그 타입을 일일이 넘기지 않고 메시지만 넘기는 간단한 방식으로 편리하게 디버깅 로그를 남길 수 있습니다.

## 요약

클로저는 다양한 방식으로 활용될 수 있는 활용도가 높은 컨셉입니다.

## 참고

- [코어자바스크립트](http://www.yes24.com/Product/Goods/78586788)
- [Humanscape Tech Blog](https://medium.com/humanscape-tech/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%ED%81%B4%EB%A1%9C%EC%A0%80%EB%A1%9C-hooks%EA%B5%AC%ED%98%84%ED%95%98%EA%B8%B0-3ba74e11fda7)
- [javacript.info](https://ko.javascript.info/currying-partials)
