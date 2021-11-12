# JavaScript - Promise와 Callback

## 설명

**프로미스(Promise)**는 비동기 처리에서 사용되는 객체로 비동기적으로 실행하는 작업의 결과를 객체로 표시되어 나타냅니다. 비동기 작업 처리시 최종 결과를 반환하지 않고, 대신 프로미스를 반환해서 어떠한 결과를 제공합니다. 프로미스는 다음 중 하나의 상태를 가집니다.

- 대기(Pending) : 비동기 처리가 아직 완료되지 않은 상태.
- 이행(Fulfilled) : 비동기 처리가 완료되어 프로스미스 결과 값을 반환한 상태.
- 거부(Rejected) : 비동기 처리가 실패하여 오류가 발생한 상태.

### 특징

- 프로미스의 특징으로는 여러 개의 프로미스를 연결하여 사용할 수 있습니다.
- 실제 서비스를 구현하다 보면 네트워크 연결, 서버 문제 등으로 인해 오류가 발생할 에러 처리를 할 수 있습니다

## 요약

- 콜백 함수(CallBack Function)란?
  - 함수의 안에서 특정한 시점에 호출되는 함수를 말합니다.
  - 콜백 함수는 비동기 처리 중 발생하는 에러의 오류 처리를 할 수 없습니다.
  - 여러 개의 비동기 처리를 한꺼번에 하는 부분도 한계가 있습니다.
- 콜백 지옥(Callback Hell)이란?
  - 콜백 함수를 많이 사용하는 경우, 콜백 함수가 깊어지는 현상을 말합니다.
- Promise와 Callback의 차이
  - 콜백 함수를 사용하게 되면 가독성이 떨어지게 되고, 콜백 지옥 현상을 피할 수 있습니다.
  - 콜백의 단점들을 보완해 Promise의 사용하여 `.then()`을 통해 가독성이 높여주고 더욱 간결하게 사용할 수 있습니다.

## 참고

- [Promise 쉽게 이해하기](https://joshua1988.github.io/web-development/javascript/promise-for-beginners/)
- [MDN Promise](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Promise)
