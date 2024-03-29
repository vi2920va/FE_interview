# JavaScript - Generator-Async&Await

## 설명

### 1. 제너레이터

- ES6에서 도입된 제너레이터는 코드 블록의 실행을 일시 중지했다가 필요한 시점에 재개할 수 있는 특수한 함수입니다.
- 제너레이터 함수는 function* 키워드로 선언하고 하나 이상의 yield 표현식을 포함합니다.
- 제너레이터 함수는 화살표 함수로 정의할 수 없고 new 연산자와 함께 생성자 함수로 호출할 수 없습니다.

### 2. 제너레이터의 일시 중지와 재개

- 제너레이터는 yield 키워드와 next 메서드를 통해 실행을 일시 중지했다가 필요한 시점에 다시 재개할 수 있습니다.
  - 일반 함수처럼 한 번에 코드 블록의 모든 코드를 일괄 실행하는 것이 아니라 yield 표현식까지만 실행합니다.
  - 제너레이터 객체의 next 메서드를 호출하면 yield 표현식까지 실행되고 일시 중지합니다.
  - 필요한 시점에 호출자가 또다시 next 메서드를 호출하면 일시 중지된 코드로부터 실행을 재개하기 시작하여 다음 yield 표현식까지 실행되고 또 다시 일시 중지됩니다.
  - 이때 제너레이터 객체의 next 메서드는 value, done 프로퍼티를 갖는 이터레이터 리절트 객체를 반환하고 next 메서드가 반환한 이터레이터 리절트 객체의 value 프로퍼티에는 yield 표현식에서 yield 된 값(yield 키워드 뒤의 값)이 할당되고 done 프로퍼티에는 제너레이터 함수가 끝까지 실행되었는지를 나타내는 불리언 값이 할당됩니다.
- 제너레이터의 특성을 활용하면 비동기 처리를 동기 처리처럼 구현할 수 있습니다.

```js
// 제너레이터 함수
function* genFunc(){
  yield 1;
  yield 2;
  yield 3;
}

const generator = genFunc();

console.log(generator.next()); // {value: 1, done: false}
console.log(generator.next()); // {value: 2, done: false}
console.log(generator.next()); // {value: 3, done: false}
console.log(generator.next()); // {value: undefined, done: true}
```

### 3. async/await

- ES8에서 제너레이터보다 간단하고 가독성 좋게 비동기 처리를 동기 처리처럼 동작하도록 구현할 수 있는 async/await가 도입되었습니다.
- `async/await`에서 에러 처리는 `try...catch` 문을 사용할 수 있습니다.

### 4. async 함수

- async 함수는 async 키워드를 사용해 정의하며 언제나 프로미스를 반환합니다.
- async 함수 내에서 catch 문을 사용해서 에러 처리를 하지 않으면 async 함수는 발생한 에러를 reject하는 프로미스를 반환합니다.

### 5. await 함수

- await 키워드는 프로미스가 settled 상태(비동기 처리가 수행된 상태)가 될 때까지 대기하다 settled 상태가 되면 프로미스가 resolve한 처리 결과를 반환합니다.

### 6. async/await과 promise의 차이점

- Promise를 활용할 시에는 .catch() 문을 통해 에러 핸들링이 가능하지만, async/await은 에러 핸들링 할 수 있는 기능이 없어 try-catch() 문을 활용해야 합니다.
- 코드가 길어지면 길어줄수록 async/await를 활용한 코드가 가독성이 좋고 코드 흐름을 이해하기 용이합니다.

## 요약

제너레이터는 코드 블록의 실행을 일시 중지했다가 필요한 시점에 재개할 수 있는 특수한 함수로 프로미스를 사용한 비동기 처리를 동기 처리처럼 구현할 수 있습니다.
async/await은 제너레이터보다 간단하고 가독성 좋게 비동기 처리를 동기 처럼 구현할 수 있는 키워드입니다.
