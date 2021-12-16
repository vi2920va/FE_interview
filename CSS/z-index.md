# CSS - z-index

## 설명

- `z-index`는 다른 쌓임 순서를 적용하고 싶을 때 `position`속성을 지정하고 `z-index` 속성을 지정해야 됩니다,
- `z-index`는 속성은 하나의 정수 값을 가질 수 있다.(양수, 음수 모두 가능)
- `z-index`로 지정한 값은 요소의 z축 상의 위치를 뜻합니다.
- 각 요소들은 번호가 붙어있고, **높은 번호**를 가진 요소는 **낮은 번호** 가진 요소 위에 렌더링 된다.

### 1. z-index를 사용한 예제

 `position`속성 지정하지 않으면, `z-index`값이 아무리 높아도 소용이 없다는 걸 아래의 사진에서 확인할 수 있다.

![z-index를 사용한 예제](https://media.prod.mdn.mozit.cloud/attachments/2012/07/09/789/3f63768682968cb88d0e5270731bdfbb/understanding_zindex_03.png)

### 2. 쌓임 맥락(Stacking context)

- 자식 엘리먼트들의 `z-index` 속성 값은 오로지 부모 안에서만 의미를 가집니다.

- 쌓임 맥락은 부모 엘리먼트의 쌓임 맥락을 구성하는 하나의 단위입니다.

- 부모가 가지고 있는 `z-index`값이라는 기본 속성이 낮으면, 자식의 `z-index`값이 높아도 부모의 쌓임 순서를 따릅니다.

## 요약

- `z-index`는 요소 배치 순서를 결정할 때 사용됩니다.
- `z-index`를 사용할 때 반드시 `position` 속성을 지정해야 합니다.
- 또한 부모의 `z-index` 값이 작으면, 아무리 자식의 `z-index`값이 커도 위로 올라올 수 없습니다.

## 참고

- [z-index - MDN](https://developer.mozilla.org/ko/docs/Web/CSS/CSS_Positioning/Understanding_z_index/Adding_z-index)
- [z-index 속성의 이해](https://www.edwith.org/htmlcss/lecture/16620?isDesc=false)
- [z-index가 동작하지않는 이유 4가지 (그리고 고치는 방법)](https://erwinousy.medium.com/z-index%EA%B0%80-%EB%8F%99%EC%9E%91%ED%95%98%EC%A7%80%EC%95%8A%EB%8A%94-%EC%9D%B4%EC%9C%A0-4%EA%B0%80%EC%A7%80-%EA%B7%B8%EB%A6%AC%EA%B3%A0-%EA%B3%A0%EC%B9%98%EB%8A%94-%EB%B0%A9%EB%B2%95-d5097572b82f)
- [z-index에 관해 아무도 말해 주지 않은 것](https://mytory.net/archives/10997)
