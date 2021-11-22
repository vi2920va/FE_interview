# JavaScript - 이벤트 버블링과 캡쳐, 이벤트 위임

## 설명

[![9935-C9425-AE422-C52-C.png](https://i.postimg.cc/qRLR1cDx/9935-C9425-AE422-C52-C.png)](https://postimg.cc/xJXnCm2X)

### 1. 이벤트 버블링(Event Bubbing)

이벤트 버블링은 특정 요소에서 이벤트가 발생했을 때, 해당 이벤트가 상위 요소들까지 전달되어 특성을 의미합니다.

### 2. 이벤트 위임(Event Delegation)

이벤트 위임은 하위 요소에 각각의 이벤트를 붙이지 않고, 상위 요소에 하위 요소의 이벤트를 제어하는 방식 입니다.

### 3. 이벤트 캡쳐(Event Capture)

이벤트 캡쳐는 이벤트 버블링과 반대 방향으로 진행되는 전파 방식 입니다.

## 요약

JavaScript에서 DOM을 제어하면서 이벤트를 등록하면 이벤트가 진행되는 방식은 이벤트 버블링, 이벤트 캡쳐, 이벤트 위임으로 나뉩니다.

## 참고

- [이벤트 버블링과, 이벤트 캡쳐](https://joshua1988.github.io/web-development/javascript/event-propagation-delegation/#%EC%9D%B4%EB%B2%A4%ED%8A%B8-%EC%BA%A1%EC%B3%90---event-capture)