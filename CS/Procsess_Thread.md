# CS - Process, Thread

## 설명

### 1. 하드웨어(Hard Ware)

![](https://images.velog.io/images/vi2920va/post/a89b910d-08b7-48f5-80a4-4d235763752e/%EB%8B%A4%EC%9A%B4%EB%A1%9C%EB%93%9C%20(1).jpg)

- CPU, 메모리(ROM, RAM), 메인 보드 등 물리적인 부품을 **하드 웨어**라고 한다.
- 입력, 연산, 기억, 출력, 제어 5가지 기능을 구현하기 위해 컴퓨터는 여러가지 부품들로 구성되어 있다.

### 2. 소프트웨어(Soft Ware)

> 💡 여기서 **프로그램**은 실행한 가능한 파일(코드) 말한다.

- 컴퓨터에게 동작 방법을 지시하는 **명령어의 집합**이다.
- 프로그램 소프웨어는 컴퓨터 하드웨어에게 직접 명령어를 주거나
- 다른 소프트웨어 입력을 제공함으로써 명령어의 기능을 수행한다.

### 3. 운영체제(OS)

![](https://images.velog.io/images/vi2920va/post/6465d313-b338-4a7a-bf5b-fc1c8a2abe5d/Operating_system_placement_kor.png)
- 하드웨어를 제어하고 응용 프로그램을 실행하는 기본 프로그램

### 4. 프로세스(Process)

![](https://images.velog.io/images/vi2920va/post/156b3ef9-583f-43f3-ba1a-61a10ce8742e/process.png)

- 소프트웨어에서 언급한 프로그램이 운영 체제에 의해 메모리 영역을 할당 받고 실행 중인 것을 말한다.
  - Code : PC(다음 번에 실행될 명령어의 주소를 갖고 있는 레지스터, 코드 저장)
  - Data  : global variables, static variables 저장
  - Heap  : 메모리 관리
  - Stack : 프로세스가 할당된 자원을 이용하는 실행의 단위, 임시 데이터 저장(local variables, return address)저장
  
### 5. 멀티 프로세스(Multi Process)

- 멀티 프로세스는 2개 이상의 프로세스가 동시에 실행되는 것을 말한다.
- 동시에라는 말은 동시성(concurrency)과 병렬성(parallelism)두 가지를 가르킨다.
  - 동시성 : CPU Core가 1개일 때, 여러 프로세스를 짧은 시간동안 번갈아 가면서 연산을 하게되는 시분할 시스템(time sharing system)되는 것을 말한다.
  - 병렬성 : CPU Core가 여러 개 일 때, 각각의 Core가 각각의 프로세스를 연산함으로써 process가 동시에 실행되는 것을 말한다.
  
## 6. 스레드(Thread)

![](https://images.velog.io/images/vi2920va/post/da158f5f-5b54-4ede-9624-21bfffdd80f8/thread.png)

- 어떠한 프로그램 내에서, 특히 프로세스 내에서 실행되는 흐름의 단위를 말한다.

## 요약

- 프로세스는 메모리 영역을 할당 받고 실행중인 것을 말한다.
- 스레드는 프로그램 내에서 실행되는 흐름의 단위를 말한다.

## 참고

- 하드웨어 [→(SITE)](http://www.terms.co.kr/hardware.htm)
- 소프트웨어 [→(WIKI)](https://ko.wikipedia.org/wiki/%EC%86%8C%ED%94%84%ED%8A%B8%EC%9B%A8%EC%96%B4)
- 운영체제 [→(WIKI)](https://ko.wikipedia.org/wiki/%EC%9A%B4%EC%98%81_%EC%B2%B4%EC%A0%9C)
- 브라우저에 URL을 입력하면? CS 기본부터 공부하기[→(VIDEO)](https://www.youtube.com/watch?v=T2WqQcqssoE&ab_channel=%EA%B0%80%EC%9E%A5%EC%89%AC%EC%9A%B4%EC%9B%B9%EA%B0%9C%EB%B0%9CwithBoaz)