# JavaScript - require VS import

## 설명

require와 import는 모두 자바스크립트에서 모듈을 불러오는 방식입니다.

### 1. require

- 자바스크립트 자체가 지원하는 패키지 읽는 방법입니다.
- node.js의 빌트인 함수입니다.
- 브라우저에서 직접 쓸 수 없지만 외부 라이브러리를 사용하면 쓸 수 있습니다.

### 2. import

- ES6의 사양으로 새로운 패키지 읽는 방법입니다.
- 모든 브라우저에서 사용 가능한 것이 아니므로 Babel같은 ES6 코드 변환 도구를 사용할 수 없다면 require 키워드를 사용해야 합니다(외부 라이브러리나 웹팩 설정을 통해).

### 3. 차이점

- require는 CommonJS를 사용하는 node.js문이지만 import는 ES6에서만 사용됩니다.
- require는 프로그램의 어느 지점에서나 호출 할 수 있지만 import는 파일의 시작 부분에서만 실행할 수 있습니다.
- 일반적으로 import는 사용자가 필요한 모듈 부분만 선택하고 로드 할 수 있기 때문에 더 선호됩니다. 이 명령문은 require보다 성능이 우수하며 메모리를 절약합니다(트리셰이킹 가능).
- import는 동적으로 로드가 가능하지만 require는 불가합니다.

## 요약

- require와 import는 모두 자바스크립트에서 모듈을 불러오는 방식입니다.
- 주로 node.js환경에서는 CommonJS방식인 require를, 클라이언트 사이드에서는 ES6방식의 import를 씁니다.

## 참고

- [D2](https://d2.naver.com/helloworld/12864)
- [stackoverflow](https://stackoverflow.com/questions/31644189/javascript-require-in-browser)
