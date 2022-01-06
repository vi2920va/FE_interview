# JavaScript - Event Handler

## 설명

### 1. 이벤트 드리븐 프로그래밍

- 이벤트 드리븐 프로그래밍은 이벤트와 특정 방향으로 몰고가다의 과거완료형인 driven의 합성어로 이벤트의 발생에 의해 특정 방향으로 가도록 된이라는 뜻입니다.
- 따라서 이벤트의 발생에 의해 프로그램 흐름이 결정되는 프로그래밍 패러다임이라고 합니다.
- 애플리케이션이 특정 타입의 이벤트에 대해 반응하여 어떤 일을 하고 싶다면 해당하는 타입의 이벤트가 발생했을 때 호출될 함수를 브라우저에게 알려 호출을 위임합니다.
- 이때 이벤트가 발생했을 때 호출될 함수를 이벤트 핸들러라 하고, 이벤트가 발생했을 때 브라우저에게 이벤트 핸들러의 호출을 위임하는 것을 이벤트 핸들러 등록이라 합니다.
- 예를 들어, 특정 버튼 요소에서 클릭 이벤트가 발생하면 특정 함수(이벤트 핸들러)를 호출하도록 브라우저에게 위임(이벤트 핸들러 등록)할 수 있습니다.
- 이벤트와 그에 대응하는 함수(이벤트 핸들러)를 통해 사용자와 애플리케이션은 상호작용을 할 수 있습니다.

### 2. 이벤트 핸들러 등록

- 이벤트 핸들러 어트리뷰트 방식
  - 이벤트 핸들러 어트리뷰트의 이름은 onClick과 같이 on 접두사와 이벤트의 종류를 나타내는 이벤트 타입으로 이루어져 있습니다.
  - Angular/React/Svelte/Vue.js 같은 프레임워크/라이브러리에서는 이벤트 핸들러 어트리뷰트 방식으로 이벤트를 처리합니다.
- 이벤트 핸들러 프로퍼티 방식
  - 이벤트 핸들러 프로퍼티에 함수를 바인딩하면 이벤트 핸들러가 등록됩니다.
  ```js
  const $button = document.querySelector('button');
  
  // 이벤트 핸들러 프로퍼티에 이벤트 핸들러를 바인딩
  $button.onClick = function(){
      console.log('button click');
  };
  ```
  - 이벤트 핸들러 프로퍼티 방식은 이벤트 핸들러 어트리뷰트 방식의 HTML과 자바스크립트가 뒤섞이는 문제를 해결할 수 있습니다.
  - 하지만 이벤트 핸들러 프로퍼티에 하나의 이벤트 핸들러만 바인딩할 수 있다는 단점이 있습니다.
  - 두 개이상의 이벤트 핸들러가 바인딩되면 첫 번째 바인딩된 이벤트 핸들러는 두 번째 바인딩된 이벤트 핸들러에 의해 재할당되어 실행되지 않습니다.
- addEventListener 메서드 방식
  - addEventHandler 메서드의 첫 번째 매개변수에는 이벤트의 종류를 나타내는 문자열인 이벤트 타입을 전달합니다.
  - 두 번째 매개변수에는 이벤트 핸들러를 전달합니다.
  - 마지막 매개변수에는 이벤트를 캐치할 이벤트 전파 단계(캡처링 또는 버블링)를 지정합니다.
    - 생략하거나 false를 지정하면 버블링 단계에서 이벤트를 캐치하고, true를 지정하면 캡처링 단계에서 이벤트를 캐치합니다.
  ```js
  const $button = document.querySelector('button');

  $button.addEvnetListener('click', function(){
      console.log('button click');
  });
  ```
  - 동일한 HTML 요소에서 발생한 동일한 이벤트에 대해 이벤트 핸들러 프로퍼티 방식은 하나 이상의 이벤트 핸들러를 등록할 수 없지만 addEventListener 메서드는 하나 이상의 이벤트 핸들러를 등록할 수 있습니다.
  - 이때 이벤트 핸들러는 등록된 순서대로 호출됩니다.

### 3. 이벤트 핸들러 제거

- addEventListener 메서드로 등록한 이벤트 핸들러를 제거하려면 EventTarget.prototype.removeEventListener 메서드를 사용합니다.
- removeEventListener 메서드에 전달할 인수는 addEventListener 메서드와 동일합니다.
- 단, addEventListener 메서드에 전달한 인수와 removeEventListener 메서드에 전달한 인수가 일치하지 않으면 이벤트 핸들러가 제거되지 않습니다.
```js
const $button = document.querySelector('button');
const handleClick = () => console.log('button click');
// 이벤트 핸들러 등록
$button.addEventListener('click', handleClick);

// 이벤트 핸들러 제거
$button.removeEventListener('click',handleClick,true); // 실패
$button.removeEventListener('click',handleClick); // 성공
```
- 무명 함수를 이벤트 핸들러로 등록한 경우는 제거할 수 없습니다.
- 이벤트 핸들러를 제거라혀면 이벤트 핸들러의 참조를 변수나 자료구조에 저장하고 있어야 합니다.
- 기명 이벤트 핸들러 내부에서 removeEventListener 메서드를 호출하여 이벤트 핸들러를 제거하는 것은 가능하고 이때 이벤트 핸들러는 단 한번만 호출됩니다.
```js
// 기명 함수를 이벤트 핸들러로 등록
$button.addEventListener('click', function foo(){
    console.log('button click');
    // 버튼을 여러 번 클릭해도 단 한번만 이벤트 핸들러가 호출된다.
    $button.removeEventListener('click',foo);
});
```
- 이벤트 핸들러 프로퍼티 방식으로 등록한 이벤트 핸들러를 제거하려면 이벤트 핸들러 프로퍼티에 null을 할당합니다.
  ```js
  const $button = document.querySelector('button');
  
  // 이벤트 핸들러 프로퍼티에 이벤트 핸들러를 바인딩
  $button.onClick = function(){
      console.log('button click');
  };

  // 이벤트 핸들러 프로퍼티에 null을 할당하여 이벤트 핸들러를 제거한다.
  $button.onClick = null;
  ```

### 4. 이벤트와 콜백함수의 차이점

- 콜백함수는 비동기식 함수에서 결과를 반환할 때 호출되지만, 이벤트 핸들링은 옵저버 패턴에 의해 작동됩니다.
- 콜백은 이벤트가 발생하면 특정 메서드를 호출해 알려주고(1개), 이벤트 리스너는 이벤트가 발생하면 연결된 이벤트 리스너 모두에게 이벤트를 전달합니다(n개).



## 요약

이벤트 핸들러는 이벤트가 발생했을 때 브라우저에게 호출을 위임한 함수입니다.
이벤트 핸들러를 등록하는 방법은 이벤트 핸들러 어트리뷰트 방식, 이벤트 핸들러 프로퍼티 방식, addEventListener 방식 세 가지가 있습니다.
