# Custom Hook

커스텀 훅은 상태 저장 방식이 비슷한 로직일 경우 해당 방식을 재사용하기 위해서 만들어 질수 있습니다. 기본적으로 기본으로 제공되는 훅과 비슷한 맥락이며 훅룰을 지킵니다.

> ## 훅 규칙

- 최 상위 레벨에서 호출합니다.
- React 함수 컴포넌트 안에서 사용합니다.
- 커스텀 훅 안에서 사용합니다.
- use로 시작하는 네이밍을 해야합니다.

이러한 내용을 지키는 가운데 상태관리를 적용하는 로직을 재사용하는 기능입니다. 여기에는 기본적으로 제공되는 리액트 훅들도 사용가능 합니다.

> 주의 해야할 사항으로 커스텀 훅이 함수를 반환하고 컴포넌트 안에서 해당 함수를 사용할 때 해당 함수는 컴포넌트가 실행할때마다 재생성되는 것이기 때문에, `useEffect` 를 사용하는 경우 커스텀 훅안에 있는 함수들을 useCallback 으로 wrapping 해줘야합니다.

### axios 상태를 관리하는 커스텀 훅

![](https://images.velog.io/images/doodream/post/267f16b9-eb1b-4fba-b73a-4c0fa90b2082/image.png)

위 코드는 `api`를 ajax 함수인 axios를 관리하는 커스텀 훅으로서 isLoading, error, sendRequest를 반환합니다.

### 간단하게 훅을 사용한 컴포넌트

![](https://images.velog.io/images/doodream/post/9e8ffc13-54f2-49f1-8a44-eed502ac6815/image.png)

위 코드는 커스텀훅에서 반환된 sendRequest를 간단하게 사용하였습니다.

## 요약

- 커스툼 훅 또한 훅규칙을 지킵니다.
- 컴포넌트의 안의 같은 로직이나 훅안의 같은 로직을 아웃소싱해서 중복 코드를 방지 할 수 있고 가독성을 높일 수 있습니다.

## 참고

-[Udemy 강의](http://https://www.udemy.com/course/react-the-complete-guide-incl-redux/learn/lecture/25599684#content)
