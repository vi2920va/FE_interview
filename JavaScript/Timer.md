# JavaScript - Timer

## 설명

### 1. 호출 스케일링

- 호출 스케일링이란 함수를 명시적으로 호출하지 않고 일정 시간이 경과된 이후에 호출되도록 함수 호출을 예약하기 위해 타이머 함수를 사용하는 것을 말합니다.
- 자바스크립트는 타이머를 생성할 수 있는 타이머 함수 setTimeout과 setInterval, 타이머를 제거할 수 있는 타이머 함수 clearTimeout과 clearInterval을 제공합니다.
- 타이머 setTimeout과 setInterval은 모두 일정 시간이 경과된 이후 콜백 함수가 호출되도록 타이머를 생성합니다.
- 자바스크립트 엔진은 단 하나의 실행 컨텍스트 스택을 갖기 때문에 두 가지 이상의 태스크를 동시에 실행할 수 없습니다.
- 이런 이유로 타이머 함수 setTimeout과 setInterval은 비동기 처리 방식으로 동작합니다.

### 2. 타이머 함수

- setTimeout
  - setTimeout 함수는 두 번째 인수로 전달받은 시간(ms, 1/1000초)으로 단 한 번 동작하는 타이머를 생성합니다.
  - 이후 타이머가 만료되면 첫 번째 인수로 전달받은 콜백 함수가 호출됩니다.
  - 즉, setTimeout 함수의 콜백 함수는 두 번째 인수로 전달받은 시간 이후 단 한 번 실행되도록 호출 스케일링됩니다.
  
```js
  // 1초 후 타이머가 만료되면 콜백 함수가 호출됩니다.
  // 이때 콜백 함수에 'Lee'가 인수로 전달됩니다.
  setTimeout(name=>console.log(`Hi! ${name}.`),1000,'Lee');
  // 두 번째 인수를 생략하면 기본값 0이 지정됩니다.
  setTimeout(()=>console.log('Hello!'));
```

- setTimeout 함수는 생성된 타이머를 식별할 수 있는 고유한 타이머 id를 반환합니다.
- setTimeout 함수가 반환한 타이머 id를 clearTimeout 함수의 인수로 전달하여 타이머를 취소할 수 있습니다.
  
```js
  // setTimeout 함수는 생성된 타이머를 식별할 수 있는 고유한 타이머 id를 반환합니다.
  const timerId = setTimeout(()=>console.log('Hi!'),1000);

  // setTimeout 함수가 반환한 타이머 id를 clearTimeout 함수의 인수로 전달하여 타이머를 취소합니다.
  clearTimeout(timerId);
```

- setInterval
  - setInterval 함수는 두 번째 인수로 전달받은 시간으로 반복 동작하는 타이머를 생성합니다.
  - 이후 타이머가 만료될 때마다 첫 번째 인수로 전달받은 콜백 함수가 반복 호출됩니다.
  - 이는 타이머가 취소될 때까지 계속됩니다.
  - setInterval 함수에 전달한 인수는 setTimeout 함수와 동일합니다.
  - setInterval 함수는 생성된 타이머를 식별할 수 있는 고유한 타이머 id를 반환합니다.
  - setInterval 함수가 반환한 타이머 id를 clearInterval 함수의 인수로 전달하여 타이머를 취소할 수 있습니다.

### 3. 디바운스와 스로틀

- 디바운스와 스로틀은 짧은 시간 간격으로 연속해서 발생하는 이벤트를 그룹화해서 과도한 이벤트 핸들러의 호출을 방지하는 프로그램입니다.
- 디바운스와 스로틀의 구현에는 타이머 함수가 사용됩니다.

### 4. 디바운스

- 디바운스는 짧은 시간 간격으로 이벤트가 연속해서 발생하면 이벤트 핸들러를 호출하지 않다가 일정 시간이 경과한 이후에 이벤트 핸들러가 한 번만 호출되도록 합니다.
- 즉, 디바운스는 짧은 시간 간격으로 발생하는 이벤트를 그룹화해서 마지막에 한 번만 이벤트 핸들러가 호출되도록 합니다.
  
```html
<!DOCTYPE html>
<html>
<body>
    <input type="text">
    <div class="msg"></div>
    <script>
        const $input = document.querySelector('input');
        const $msg = document.querySelector('.msg');

        const debounce = (callback,delay)=>{
            let timerId;
            // debounce 함수는 timerId를 기억하는 클로저를 반환합니다.
            return event => {
                // delay가 경과하기 이전에 이벤트가 발생하면 이전 타이머를 취소하고 새로운 타이머를 재설정합니다.
                // 따라서 delay보다 짧은 간격으로 이벤트가 발생하면 callback은 호출되지 않습니다.
                if(timerId) clearTimeout(timerId);
                timerId = setTimeout(callback, delay, event);
            }
        }

        // debounce 함수가 반환하는 클로저가 이벤트 핸들러로 등록됩니다.
        // 300ms보다 짧은 간격으로 input 이벤트가 발생하면 debounce 함수의 콜백 함수는
        // 호출되지 않다가 300ms 동안 input 이벤트가 더 이상 발생하지 않으면 한 번만 호출됩니다.
        $input.oninput = debounce(e=>{
            $msg.textContent = e.target.value;
        }, 300);
    </script>
</body>
</html>
```

- delay보다 짧은 간격으로 이벤트가 연속해서 발생하면 debounce 함수의 첫 번째 인수로 전달한 콜백 함수는 호출되지 않다가 delay 동안 input 이벤트가 더 이상 발생하지 않으면 한 번만 호출됩니다.

### 5. 스로틀

- 스로틀은 짧은 시간 간격으로 이벤트가 연속해서 발생하더라도 일정 시간 간격으로 이벤트 핸들러가 최대 한 번만 호출되도록 합니다.
- 즉, 스로틀은 짧은 시간 간격으로 연속해서 발생하는 이벤트를 그룹화해서 일정 시간 단위로 이벤트 핸들러가 호출되도록 호출 주기를 만듭니다.

```js
const $container = document.querySelector('.container');
const $throttleCount = document.querySelector('.throttle-count');

const throttle = (callback,delay)=>{
    let timerId;
    // throttle 함수는 timerId를 기억하는 클로저를 반환합니다.
    return event => {
        // delay가 경과하기 이전에 이벤트가 발생하면 아무것도 하지 않다가
        // delay가 경과했을 때 이벤트가 발생하면 새로운 타이머를 재설정합니다.
        // 따라서 delay 간격으로 callback이 호출됩니다.
        if(timerId) return;
        timerId = setTimeout(()=>{
            callback(event);
            timerId = null;
        }, delay, event);
    };
};

let throttleCount = 0;
// throttle 함수가 반환하는 클로저가 이벤트 핸들러로 등록됩니다.
$container.addEventListener('scroll', throttle(()=>{
    $throttleCount.textContent = ++ throttleCount;
},100));
```
- throttle 함수가 반환한 함수는 throttle 함수에 두 번째 인수로 전달한 시간이 경과하기 이전에 이벤트가 발생하면 아무것도 하지 않다가 delay 시간이 경과했을 때 이벤트가 발생하면 콜백 함수를 호출하고 새로운 타이머를 재설정합니다.
- 따라서 delay 시간 간격으로 콜백 함수가 호출됩니다.

## 요약

- 타이머 setTimeout과 setinterval은 모두 일정 시간이 경과된 이후 콜백 함수가 호출되도록 타이머를 생성합니다.
- delay보다 짧은 간격으로 이벤트가 연속해서 발생하면 debounce 함수의 첫 번째 인수로 전달한 콜백 함수는 호출되지 않다가 delay 동안 input 이벤트가 더 이상 발생하지 않으면 한 번만 호출됩니다.
- debounce 함수는 resize 이벤트 처리나 input 요소에 입력된 값으로 ajax 요청하는 입력 필드 자동완성 UI 구현, 버튼 중복 클릭 방지 처리 등에 유용하게 사용됩니다.
- throttle 함수는 scroll 이벤트 처리나 무한 스크롤 UI 구현 등에 유용하게 사용됩니다.
