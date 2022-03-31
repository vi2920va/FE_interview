# React - react-router-dom. -v6

## 필요성(설명)

React는 SPA(Single Page Application)으로 기존 SSR(Server Side Rendering)과는 달리 페이지 전체 새로고침이 아닌 적시에 필요한 부분만 렌더링하는 특징을 가지고 있다.

그러나 React 자체에 이러한 Routing 기능이 내장되어 있지는 않기 때문에 react-router-dom 라이브러리를 사용하여 구현하게 된다.

=> 결과적으로 react-router-dom이 a 태그를 이용할 때 생기는 무의미한 재랜더링을 예방하여 속도가 빨라지게 된다.

### 1. Router component들의 기본형(low-level interface)

**BrowserRouter**
www.123123/post

1. HTML5의 history API를 이용해 UI를 업데이트
2. 동적인 페이지에 적합
3. 새로 고침 / 경로를 이용해서 페이지를 찾아갈 때 에러가 발생할 수 있다 => 추가적인 세팅이 필요하다
4. 가장 보편적으로 이용된다

**HashRouter**
www.123123/#/post

1. URL의 hash를 이용한다
2. 주소에 #가 붙는다
3. 정적인 페이지에 적합하다
4. 검색 엔진으로 읽어내지 못한다('#'때문)
5. 새로고침해도 에러가 나지 않는다
6. #가 들어있어 주소가 보기에 좋지 않다.
7. 검색엔진에도 잡히지 않는다

**MemoryRouter**

1. 브라우저의 주소와 아예 무관하다
2. 브라우저가 아닌 환경에서 쓰기 좋다
3. 테스트, 임베디드 웹앱, 리액트 네이티브 등에서 쓸 수 있다

**NativeRouter**

1. React Native를 통해 IOS나 Android 앱을 만들 때 사용된다

**StaticRouter**

1. 서버에서 주로 사용한다

### 2. 추가적인 compomnent들

- Link

1. 클릭하면 다른 주소로 이동이 가능하다
2. <\Link> 와 <\a> 의 차이
   - <\a> 태그를 이용하면 페이지가 리렌더링되게 된다.
   - <\Link>를 이용하면 단순히 URL만 변경된다

- Routes

1. v6 이전 이름은 Switch였다.
   Routes로 변경되면서 경로 탐색 우선순위 문제가 해결되었다.
2. 일치하는 경로를 Routes에서 탐색한 후 경로가 일치하면 탐색을 중단한 후 렌더링한다
3. <\Routes> 외부에 있는 <\Route> 경로는 탐색할 수 없다

- Route

1. <\Link> 컴포넌트에 to로 지정된 path에 컴포넌트를 연결해준다
   `<Route path="/" element={<Home/>}/>`

### 3. 유용한 API

- useNavigate()
  - 원하는 경로로 이동할 수 있다
- useLocation()
  - 사용자가 머물러있는 페이지에 대한 정보를 알려준다(쿼리스트링 정보)
- useParams()
  - 경로(path parameter)의 정보를 얻을 수 있다.
  - match를 이용해 경로를 찾을 수도 있다.

## 요약

- React의 SPA 특성을 효율적으로 잘 사용하기 위해 만들어졌다.
- 페이지 이동을 원하는 컴포넌트가 라우터 컴포넌트의 기본형 안에 들어있어야 작동한다
- 컴포넌트 기본형 외에도 라우팅 시 유용한 추가 컴포넌트들이 존재한다

## 참고

- [[react] Router 이론](https://velog.io/@lllen/react-router)
- [React Router dom의 유용한 hooks들](https://velog.io/@yiyb0603/React-Router-dom%EC%9D%98-%EC%9C%A0%EC%9A%A9%ED%95%9C-hooks%EB%93%A4)
- [BrowserRouter HashRouter 차이 + BrowserRouter 배포시 흰 화면 문제](https://pottatt0.tistory.com/44?category=989402)
