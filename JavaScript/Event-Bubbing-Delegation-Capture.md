# JavaScript - 이벤트 버블링과 캡쳐, 이벤트 위임

## 설명

[![9935-C9425-AE422-C52-C.png](https://i.postimg.cc/qRLR1cDx/9935-C9425-AE422-C52-C.png)](https://postimg.cc/xJXnCm2X)

### 1. 이벤트 버블링(Event Bubbing)

이벤트 버블링은 특정 요소에서 이벤트가 발생했을 때, 해당 이벤트가 상위 요소들까지 전달되어 특성을 의미합니다.

### 2. 이벤트 위임(Event Delegation)

이벤트 위임은 하위 요소에 각각의 이벤트를 붙이지 않고, 상위 요소에 하위 요소의 이벤트를 제어하는 방식 입니다.

#### 이벤트 위임 예제 코드

로그인을 하려면 ID, PW을 입력 후에 값을 서버에게 전송해야 됩니다. 아래의 경우처럼 마크업 되어있다 가정하에 각각의 요소에 이벤트를 걸어주는 것 보다, **ID, PW, Btn을 전체적으로 감싸고 있는 부모 form에 이벤트 걸어주는 게 효율적**입니다. 그 이유는 각각의 이벤트에 걸어주게 되면 메모리 사용량을 줄게하고, 각각의 요소들에 굳이 불필요하게 이벤트 걸어주지도 않기 때문입니다.

```html
<form class="login__form">
  <!-- iput id -->
  <div class="login__form-id__wrapper">
    <label for="userID">ID: </label>
    <input type="text" class="input__id" id="userID" />
  </div>
  <!-- iput pw -->
  <div class="login__form-pwd__wrapper">
    <label for="userPWD">PW: </label>
    <input type="password" class="input__pw" id="userPW" />
  </div>
  <!-- form button -->
  <div class="login__form-btn-group">
    <button type="button">로그인</button>
    <button type="reset">비밀번호</button>
  </div>
</form>
```

### 3. 이벤트 캡쳐(Event Capture)

이벤트 캡쳐는 이벤트 버블링과 반대 방향으로 진행되는 전파 방식 입니다.

### 4. 이벤트 전파를 막는 방법

- `event.preventDefault()`: 현재 이벤트의 기본 동작을 막는 방법 입니다.
- `event.stopPropagation()`: 현재 이벤트가 버블링 또는 캡쳐링 되는 것을 막습니다.
- `event.stopImmediatePropagation()`: 이벤트 버블링 혹은 캡처링 뿐만 아니라, 해당 객체 걸린 현재 이벤트를 제외한 다른 이벤트 동작까지 막습니다.

## 요약

JavaScript에서 DOM을 제어하면서 이벤트를 등록하면 이벤트가 진행되는 방식은 이벤트 버블링, 이벤트 캡쳐, 이벤트 위임으로 나뉩니다.

## 참고

- [이벤트 버블링과, 이벤트 캡쳐](https://joshua1988.github.io/web-development/javascript/event-propagation-delegation/#%EC%9D%B4%EB%B2%A4%ED%8A%B8-%EC%BA%A1%EC%B3%90---event-capture)
