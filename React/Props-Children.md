# React - props.children

## 설명

- `props.children`은 컴포넌트의 여는 태그와 닫는 태그 사이의 **로직, 배열 등 형태와 상관없이** 다른 컴포넌트에 내용을 전달할 수 있습니다.
- 즉, `props.children`은 공통된 레이아웃에 많이 사용됩니다.

### 1. 예제

- 두 개의 사진을 자세히 보면 공통된 부분을 찾을 수 있습니다.
- 이 경우에 컨테이너 컴포넌트를 만들고, 콘텐츠의 내용을 `props.children`을 사용할 수 있습니다.

[![2.png](https://i.postimg.cc/BnmGw4jp/2.png)](https://postimg.cc/4YHMYCbH)

[![4.png](https://i.postimg.cc/253NsMvD/4.png)](https://postimg.cc/5HcrvPn7)

```jsx
import React, { useState } from 'react';
import { FiChevronLeft, FiChevronRight } from "react-icons/fi";

function Slide({ title, list, name, children, onSlide }) {
  const [slideCount, setSlideCount] = useState(1);

  const handleNextClick = () => {
    if (slideCount < list.length) {
      setSlideCount(prev => prev + 1);
      onSlide('next', slideCount);
    }
  };
  const hadlePrevClick = () => {
    if (slideCount > 1) {
      setSlideCount(prev => prev - 1);
      onSlide('prev', slideCount);
    }
  };

  if (list.length === 0) {
    return null;
  }

  return (
    <section className="slide">
      <div className={`slide-wrapper__${name}`}>
        <h3 className={`slide-title__${name}`}>{title}</h3>
        <ol className={`slide-count__list-${name}`}>
          <li>
            <button
              className="button--prev"
              disabled={slideCount === 1}
              onClick={hadlePrevClick}
            >
              <FiChevronLeft />
            </button>
          </li>
          <li className="count--item">
            <div className="count--item__text">
              <span>{slideCount}</span>
              <span>/</span>
              <span>{list.length}</span>
            </div>
          </li>
          <li>
            <button
              className="button--next"
              onClick={handleNextClick}
              disabled={slideCount === list.length}
            >
              <FiChevronRight />
            </button>
          </li>
        </ol>
      </div>
      {children}
    </section>
  );
}

export default Slide;
```

## 요약

`props.children`은 JSX에서 태그와 태그 사이에 속성(props)입니다. 

## 참고

- [props.children(예제코드)](https://codesandbox.io/s/children-hx7sz?file=/src/App.js)
- [[React] this.props.children](https://velog.io/@hanei100/React-this.props.children)
- [props.children](https://ko.reactjs.org/docs/glossary.html#propschildren)