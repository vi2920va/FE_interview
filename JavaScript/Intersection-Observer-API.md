# Intersection Observer API

## Intersection Observer API

Intersection Observer API는 타겟 요소와 상위요소 또는 최상위 document의 뷰포트(화면에 보이는 직사각형 영역)의 intersection 내의 변화를 **비동기적으로 관찰**하는 방법이다.



## 언제 사용될까?

- 페이지가 스크롤 되는 도중에 이미지나 다른 컨텐츠를 지연로딩할때
- 인피니트 스크롤 구현
- 광고를 봤는지 확인하는 용도



## Intersection Observer가 없다면

이전에는 `getBoundingClientRect()` 같은 메소드를 이벤트 핸들러마다 호출/계산을 반복해줬다. 즉 스크롤 이벤트가 호출될때마다 현재 요소가 몇% 노출되었는지를 직접 계산해줘야 했다.

이 모든 코드들은 **메인스레드 위에서 실행이 되기때문에 성능상의 문제**를 일으킬 수도 있었다.

Intersection Observer API는 감시하고자하는 요소가 다른 요소(뷰포트)에 들어가거나 나갈때 또는 요청한 부분만큼 두 요소의 교차부분이 변경될때마다 실행될 **콜백 함수**를 등록할 수 있게 된다.

즉, **교차하는지 지켜보기 위해서 메인스레드를 사용할 필요가 없어지고** 브라우저는 원하는대로 교차영역 관리를 최적화할 수 있게된다.

정확히 몇 픽셀이 겹쳤고, 어떤 픽셀이 겹쳤는지는 알 수 없지만 몇 %가 겹쳤는지는 확인할 수 있다.



## 콜백이 실행되는 경우

1. 대상 요소가 기기 뷰포트나 특정 요소(root를 설정했다면 root요소, 아니면 document)와 교차함.
2. observer가 **최초로 타겟을 관측하도록 요청**받을때.
3. 

## 예제코드

```jsx
const lastElement = document.querySelector("#last");

const options = {
  root: null, // document가 루트가 된다.
  rootMargin: "0px",
  threshold: 0
};

let observer = new IntersectionObserver((entities, observer) => {
  if (entities[0].intersectionRatio > 0.5) {
    console.log("Last Element!");
  }
}, options);

observer.observe(lastElement);
```

![img](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F74a0425e-f76b-4bc4-a0db-e1da1e3676fe%2FUntitled.png?table=block&id=710dfaf2-9a33-49cf-87dd-e7d5cf3012fc&spaceId=6c80bbf0-c204-46b7-ad0e-82bfdb2bc394&width=2000&userId=2fee838d-3fab-4dee-9e14-47b5e88c3164&cache=v2)마지막뷰(20번째박스)가 50%이상 뷰포트에 나왔을때 콘솔로그 출력.



## 요약

- Intersection Observer API를 사용하면 메인 스레드를 사용하지 않고 비동기적으로 요소의 겹침을 계산할 수 있다.

## 레퍼런스

- [MDN Intersection Observer API](https://developer.mozilla.org/ko/docs/Web/API/Intersection_Observer_API)