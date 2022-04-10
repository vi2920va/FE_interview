# Fronent - 크로스브라우징(Cross Browsing)

## 설명

### 1. 크로스 브라우징

- 크롬, IE, 웨일, 파이어 폭스 등등 브라우저의 종류는 매우 다양합니다.
- 많은 웹 브라우저들의 동작은 W3C 국제 웹 표준화 기구에서 제공되는 가이드라인에 따라서 동작합니다.
- 하지만, W3C에서 제공되지 않는 세밀한 가이드라인은 각 브라우저의 상황에 맞게 구현되어 있습니다.
- 즉, 크로스 브라우징이란 **표준 웹 기술**을 따르면서 다른 기종은 혹은 플랫폼에서 구현되는 기술을 비슷하고 만듦과 동시에 어느 한쪽에 최적화 되어 치우지지 않도록 공통 요소를 사용하여 웹 페에지를 제작하는 기법입니다.

### 2. 크로스 브라우징 대응 방법

크로스 브라우징에 대응 방법으로는 다음과 같습니다.

2-1. 현재 기능의 지원 브라우저를 파악합니다.

- 현재 개발할 기능을 정의하고, 해당 기능을 지원할 브라우저를 파악합니다.
- 모든 브라우저를 100% 만족하여 대응하려고 하지 않습니다.
- 모든 브라우저에 대해서는 핵심 기능만 불편함 없이 동작하게 합니다.
- 최신 브라우저를 대상으로는 의도하는 기능들이 잘 동작하게 구현합니다.

2-2. 라이브러리를 사용하는 것도 대응 방법 중 하나 입니다.

- jQuery
- Polyfill

2-3. 다양한 도구를 활용합니다.

- [Can I user ?](https://caniuse.com/)
  - 내가 사용하고 싶은 CSS, JavaScript 명을 넣으면 어떤 브라우저에서 작동되는지 여부를 파악할 수 있습니다.

- [W3C Markup Validation](http://validator.w3.org/)
- [W3C CSS Validation](http://jigsaw.w3.org/css-validator/)
  - 웹 표준을 준수하는 지 검사하는 방법으로는 W3C에서 제공하는 서비스를 이용하면 됩니다.

- [MDN](https://developer.mozilla.org/ko/)
  - 기본 문법과 다양한 예제를 더불어 친철하게 설명을 해주는 사이트 입니다.

2-4. 그 외 다른 방법

- [reset.css](https://meyerweb.com/eric/tools/css/reset/)
  - 각 브라우저 마다 기본적으로 설정되어 있는 스타일 다르기 때문에 초기화 해주는 것도 대응 방법 중 하나입니다.
  
- 벤더 프리픽스(Vendor Prefix)
  - 벤더  프리픽스는 CSS 표준안으로 확정되지 않았거나 실험적으로 제공하는 기능을 쓸 때, 벤더 프리픽스를 사용해주면 그 기능이 포함되지 않는 구형 브라우저에서도 쓸 수 있습니다.

```css
* {
  -webkit-user-select: none;  /* Chrome all / Safari all */
  -moz-user-select: none;     /* Firefox all */
  -ms-user-select: none;      /* IE 10+ */
  user-select: none;          /* Likely future */지
}
```

## 요약

- 크로스 브라우징을 요약하면 웹 페이지 제작 시, 모든 브라우저에서 깨지지 않고 의도한 대로 올바르게 나오게 하는 작업을 말합니다.
- 크로스 브라우징에 대한 대응 방법으로는 여러 가지 방법이 있어서 설명에 자세히 적어놓았습니다.

## 참고

- 크로스 브라우징 [→(BLOG)](https://okayoon.tistory.com/entry/%ED%81%AC%EB%A1%9C%EC%8A%A4-%EB%B8%8C%EB%9D%BC%EC%9A%B0%EC%A7%95cross-browsing)
- 벤더 프리픽스 [→(SITE)](https://poiemaweb.com/css3-vendor-prefix)
