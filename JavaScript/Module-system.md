# JavaScript - Module system

## 설명

### 1. 모듈이란

- 모듈이란 디테일한 내용을 추상화해주는 재사용 가능한 코드 단위입니다.
- 주로 단일 파일을 뜻하지만, 반드시 단일 파일일 필요는 없습니다.
- 자동차의 부품처럼 코드로 이루어진 서비스 전체의 코드 부품이라고 생각할 수 있습니다.

### 2. 모듈 시스템을 쓰는 이유

- 1. 소프트웨어 구성
     : 복잡한 소프트웨어를 구성하는 코드 블록 단위로서 활용합니다.
- 2. 컴포넌트 분리
     : 전체 소프트웨어 흐름을 고려하지 않고 개발할 수 있습니다.
- 3. 코드 추상화
     : 소프트웨어에서 사용 시 디테일한 내용을 고려할 필요 없이 부품으로서 바로 사용할 수 있습니다.
- 4. 코드 구조화
     : 모듈을 사용하여 소프트웨어를 만드는 것이 하나의 긴 파일로 이루어진 소프트웨어를 만드는 것보다 구조적으로 좋습니다.
- 5. 코드 재활용성 향상
     : 모듈로 이루어진 코드는 여러 프로젝트에서 이용되어지기 쉽습니다.

### 3. 주의할 점

top level await 주의해야 합니다.

- top level await란 ES2022에 등장한 개념으로 async 함수 밖에서 await을 쓸 수 있는 것을 의미합니다.
- 현재로서는 모듈에서만 동작하는 개념입니다.

- 모듈 상단에 있는 비동기 처리는 동기처리처럼 진행이 됩니다.
- 해당 처리가 다 끝나야 나머지 임포트를 진행할 수 있습니다.

### 4. Module을 import/export 하는 방법

#### 4-1. export

- export는 다른 파일에서 사용할 수 있도록 모듈을 내보냅니다.

- 종류

- 1. named export
  - 선언된 변수명 그대로 export 합니다
    named export 는 모듈 내에 여러개 존재할 수 있습니다.
  - named export 는 변수 선언과 동시에 내보내도록 하거나 먼저 정의된 변수들을 모아서 내보내는 방법이 있습니다.
- 2. default export

  - default export 는 모듈에서 하나만 존재할 수 있습니다.

#### 4-2. import

- import 는 다른 파일에서 모듈을 불러옵니다.
- default export 를 불러오거나, named export 를 불러올 수 있습니다.
- 그리고 as 를 이용해 alias 처리 \*(asterisk) 를 통해 named export 전체를 불러올 수도 있습니다.

## 요약

- require와 import는 모두 자바스크립트에서 모듈을 불러오는 방식입니다.
- 주로 node.js환경에서는 CommonJS방식인 require를, 클라이언트 사이드에서는 ES6방식의 import를 씁니다.

## 참고

- Modules [→ (MDN)](https://developer.mozilla.org/ko/docs/Web/JavaScript/Guide/Modules)
- JavaScript Module System [→ (BLOG)](https://velog.io/@doondoony/JavaScript-Module-System)
