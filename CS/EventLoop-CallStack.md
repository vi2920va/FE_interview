# Event Loop 와 call stack은 어떻게 작동하나 ?

## 설명

### 1. JavaScript Engine

[![image.png](https://i.postimg.cc/BZY7Xcdb/image.png)](https://postimg.cc/t76NcPRG)

- 위의 예제 그림에서 확인할 수 있듯이 **메모리 힙(Memory Heap)**과 **콜 스택(Call Stack)**으로 구성되어 있습니다.
- **Memory Heap**
    - 메모리 할당이 이루어지는 곳입니다.
- **Call Stack**
    - 코드가 실행될 때 호출 스택이 쌓이는 공간 입니다.
    - 즉, 호출된 함수는 Call Stack에 push 됩니다.

### 2. Web APIs

- JavaScript는 **싱글 스레드(Single Thread)**기반으로 동작하는 언어이지만, 실행 환경인 브라우저에서 JavaScript 엔진 또는 Web APIs가 함께 동작하여 비동기 적으로 처리할 수 있습니다.

### 3. Callback Queue

- 비동기적으로 이벤트 발생시 실행해야 할 `callback` 함수가 **Callback Queue**에 추가됩니다.

### 4. Event Loop

- 이벤트 루프는 비동기 함수의 스케쥴링 담당한다. 일단 콜 스택에서 현재 실행중인 작업을 확인하고 태스크 큐의 대기 중인 함수가 있는지 반복해서 확인한다.
- 그 후에 콜 스택이 비어있다면, 태스크 큐에 대기중인 함수가 있다면 첫 번째로 콜스택에 밀어준다.

- **Event Loop**는 Call Stack과 Callback Queue의 상태를 체크하여, CallStack이 빈 상태가 되면, Callback Queue의 첫 번째 콜백 함수를 Call Stat으로 밀어넣어줍니다.
- 이러한 반복적인 행동을 **틱(tick)**이라고 부릅니다.

## 요약

- JavaScript 싱글 쓰레드 기반의 언어이지만 실행 환경인 브라우저에서 비동기적 작업 처리를 할 수 있게한다.


