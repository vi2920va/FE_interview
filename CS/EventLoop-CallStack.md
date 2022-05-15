# Event Loop 와 call stack은 어떻게 작동하나 ?

## 설명

### 1. JavaScript Engine - V8

- JavaScript 엔진의 대표적인 예는 Google V8 엔진이 있다.
- V8은 Chrome과 Node.js에서 사용된다.
- 엔진은 주요 구성요소는 메모리 힙(Memory Heap), 콜 스택(Call Stack) 두 가지로 구성하게 된다
  - 메모리 힙(Memory Heap) : 메모리 할당이 일어나는 곳
  - 콜 스택(Call Stack) : 코드에 따라 호출 스택이 쌓이는 곳

### 2. Call Stack

- JavaScript는 기본적으로 싱글 쓰레드 기반의 언어로 한 번에 한 작업만 처리할 수 있다.
- 호출 스택은 기본적으로 프로그램 상에서 어디 있는지를 기록하는 자료구조 이다.
- 함수를 실행하면, 해당 함수는 호출 스택의 가장 상단에 위치하게 되고, 해당 함수의 실행이 끝날 때, 호출 스택에서 제거 하는게 스택의 역할이다.

### 3. Memory Heap

- JavaScript에서 선언 함수는 모두 메모리 힙에서 저장된다.

### 4. Web API

- Web API는 브라우저에서 제공하는 API들이다.
- 대표적인 예로는 `setTimeOut` 함수가 있다.

### 5. Callback Queue

- 함수는 저장하는 자료구조로, 콜 스택과는 다르게 가장 먼저 들어온 함수를 가장 먼저 처리한다.
- 특정 이벤트에 따른 콜백 함수를 정의하게 되면 콜백함수는 Callback Que 저장된다.

### 6. Event Loop

- 호출 스택이 다 비워지면 콜백 큐에 존재하는 함수를 하나씩 호출 스택으로 옮기는 역할은 한다.

## 요약

