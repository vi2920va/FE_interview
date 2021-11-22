# React - React Operation && Optimization Techniques

## 설명

# React 동작요소

`React`는 js 라이브러리 입니다. 즉, 자바스크립트를 이용해서 사용자 인터페이스를 구축하기 위한 것입니다.

따라서 우리는 `Components`를 이용합니다. 이러한 사용자 인터페이스를 만들기 위해서 `Components`라는 개념요소를 사용합니다.

- React는 이러한 `Components`를 수용해서 **사용자 인터페이스를 효과적으로 구성**하게 됩니다.
- 그리고 이렇게 구성된 사용자 인터페이스를 효과적으로 **업데이트** 하게됩니다.

그리고 `ReactDOM`도 있습니다. 이 `ReactDOM`은 결국에는 `React` 자체와는 다르게 웹에 대한 인터페이스 입니다. 무슨말 이냐면 `React.js` 우리가 부르는 `React`는 **웹에 대해서 관여하지 않습니다.** 브라우저에 대해서 아무것도 모릅니다.
![](https://images.velog.io/images/doodream/post/c01b5661-0d42-4be7-aed2-107b05626eab/image.png)

# ReactDOM

`React`는 `Components`를 사용하지만 해당구성요소 안에 HTML tag가 포함되어있는지 아닌지 아니면 완전히 허상의 Element tag가 있는지 아닌지는 중요하지 않습니다. 결국에 이를 표현하게 브라우저에 전달해주는 것은 `ReactDOM`의 일입니다. 궁극적으로 실제 HTML요소를 화면에 불러오게 해줍니다.

요약하면 React는 컴포넌트를 관리하는 라이브러리일 뿐이고 (이전상태와 비교했을때 어떻게 달라졌는지) 이렇게 변경된 모든 사항에 대해서 ReactDOM에 전달해줍니다. **이렇게 ReactDOM은 실제 화면이 어떻게 보여질지 에 대한 것에 그 기능이 있습니다. **

보통 리액트에서 컴포넌트의 상태가 변경되면 재실행되어 브라우저에서 리렌더링 된다고 알고 있지만 모두 리렌더링 되는 것은 아닙니다. 드물게 필요할 때만 변경되며 이는 브라우저 성능에 중요합니다.

ReactDOM 에서는 이를 가상으로 비교합니다. 다시말하면 `VirualDOM`에서 이를 비교합니다.** 가상의 메모리 안에서 (이전상태와 마지막 스냅샷상태의)비교**가 이루어지기 때문에 브라우저에 렌더링되는 실제 DOM에 접근해서 비교하는 기존의 방식과는 계산 차이가 많이 나게 됩니다.
실제로 반영될 화면이 재구성이 되어야 할지 안되어야 할지 `VirualDOM`을 통해서 리렌더링 여부를 결정합니다.

## VirtualDOM diffing

![](https://images.velog.io/images/doodream/post/552924e5-4a3b-4520-b3e3-5df425f26901/image.png)

이전상태와 마지막 스냅샷이 차이를 React가 ReactDOM에게 전달해줍니다. 여기서 ReactDOM은 가상돔 비교를 통해서 다른 것은 리렌더링하지 않고 새로운 부분만 리렌더링해야할 것을 구분합니다.

이게 어떻게 되느냐는 아래 과정을 봐봅시다.

### App.js

![](https://images.velog.io/images/doodream/post/e1290442-0f3a-47d6-9184-fa4015b6379f/image.png)

### Demo.js

![](https://images.velog.io/images/doodream/post/d638fae0-516c-4e65-a561-6839e745261d/image.png)

App 컴포넌트에서 showDifference라는 상태관리변수를 이용해서 Demo라는 자식 컴포넌트로 넘겨 `<p>` 태그를 관리하는 모습입니다. 여기서 버튼을 클릭하면 상태관리 변수가 있는 App.js 의 컴포넌트가 재실행되는 모습입니다.

![](https://images.velog.io/images/doodream/post/b250b165-1b24-4391-b211-c29c443873b0/%E1%84%87%E1%85%B5%E1%84%80%E1%85%AD1.gif)

컴포넌트의 구성요소가 바뀌면 해당 컴포넌트가 재실행 됩니다. 하지만 컴포넌트의 모든 것이 재실행되는데 DOM들도 모두 재실행될까요?

아닙니다. 아래와 같이 DOM은 정말로 바뀌는 Demo.js의 `<p>` 태그만 바뀌는 모습입니다. (번쩍 하고 빛나는 부분만 변한다고 inspector가 알려줌 )이렇게 React는 컴포넌트를 재실행하며 바뀌는 요소의 모든것을 ReactDOM에 전달하고 ReactDOM은 버추얼 돔을 이용해서 정말로 변해야하는 부분만 렌더링 합니다.

> 주의할 점은 App.js가 재실행되는 과정에서 Demo.js도 재실행되었습니다. App.js에서 Demo.js또한 그 일부이기 때문입니다. 상위 컴포넌트에서의 구성요소 변화로 인한 재실행은 하위컴포넌트도 그 일부이기 때문에 재실행됩니다. 하지만 말했다시피 DOM에서의 변화는 그렇지 않습니다. 당연하게도 Button.js도 App.js의 일부이기 때문에 재실행됩니다.

![](https://images.velog.io/images/doodream/post/1c9f9ce8-aa05-48f3-a2e8-a51e2f5b62e0/%E1%84%87%E1%85%B5%E1%84%80%E1%85%AD2.gif)

### 의문

리액트는 트리구조로 되어있습니다. 그런데 거의 최상위 컴포넌트에서만 아주 조그마한 변경이 일어나는데 해당변경으로 인해서 아래의 트리구조로 엮여있는 모든 컴포넌트들을 모두 재평가해야하나? 코드상으로는 하위 컴포넌트에서의 변경은 없는데? 혹은 DOM의 변경을 일으키지 않는 코드 변화인데 해당 기능과 전혀 관계없는 하위컴포넌트들을 모두 재평가 해야하나?

이러한 의문이 들수 있습니다. 실제로 이러한 과정은 약간의 성능차이를 발생시킬수 있습니다. 실제로 작은 앱들의 경우는 성능차이가 별로 없지만 앱이 거대해 질수록 (트리구조의 깊이가 깊어질수록) 이러한 성능차이가 발생하게 될 것입니다. 따라서 최적화가 필요합니다.

따라서 개발자는 이렇게 재실행하며 비교하는 React에게 이야기 해줄 수 있습니다.
**특정한 상황에서는 이 컴포넌트는 재실행하지마** 비교할 필요 없어

## React.memo

이 기능이 위 의문을 일부 해결해줍니다. 상위 컴포넌트에서 변경이 일어났는데 하위컴포넌트는 상위 컴포넌트에서 받는 props 변경이 전혀일어나지 않는 경우 이 기능을 입하면 하위컴포넌트는 재실행되지 않습니다. 그러면 그 하위컴포넌트 아래의 트리구조에 달려있는 컴포넌트들도 재실행이 되지 않겠죠.

![](https://images.velog.io/images/doodream/post/994d5ce2-2ff8-45a8-883b-cc813c3892ab/image.png)

위코드에서 Demo 컴포넌트는 props로 받는 요소들이 모두 static이기 때문에 변화가 생길 이유가 없습니다. 이때 React.memo를 적용받지 않을 경우
![](https://images.velog.io/images/doodream/post/f5dec6d5-7d9e-4e02-83b4-002da97d22f8/image.png)

![](https://images.velog.io/images/doodream/post/6d533460-6d63-47be-b7e2-873167b3ed20/%E1%84%87%E1%85%B5%E1%84%80%E1%85%AD3.gif)

Demo.js는 재실행됩니다. 하지만 다음과 같이 `React.memo`를 Demo.js에 래핑시키면
![](https://images.velog.io/images/doodream/post/af3888d2-9436-4646-a8b1-0c60706860fb/image.png)

![](https://images.velog.io/images/doodream/post/120347d4-4a99-48e7-808c-2bcee165ffff/%E1%84%87%E1%85%B5%E1%84%80%E1%85%AD4.gif)

Demo.js는 재실행 되지 않습니다.

### 추가의문

그렇다면 이러한 과정을 모든 컴포넌트에 하면되지 않을까? 라는 의문이 생깁니다. `React.memo`또한 계산 리소스가 필요합니다. 이전의 `props`들을 저장하고 새로 재실행될때 현재 `props`들과 비교해서 변경이 되었는지 안되었는지를 비교하는데 이 비교과정은 컴포넌트가 상위에 있을수록 유용합니다. 최하위 컴포넌트에 있다면 이러한 최적화는 의미가 없을지도 모릅니다.

또한 상위 컴포넌트에 따라서 거의 대부분 하위 컴포넌트가 변경되어야한다면 `React.memo`는 의미가 없습니다. props 비교를 하든 안하든 어차피 변경되기 때문이죠.

즉, React.memo는 최상단 컴포넌트에서 중요하게 잘라내야할 몇가지 컴포넌트에서 의미가 있습니다.

> 다만 React.memo는 props로 최적화를 진행합니다만, false같은 값이 정확하게 정해져 있는 경우 ( primitiveValue )에만 유의미 합니다. reference Value ( 객체, 배열, 함수등 ) 같이 매번 자바스크립트로 생성할때마다 새로 생성되는 참조값들을 반영하는 참조값들에 대해서는 이러한 비교가 의미가 없습니다. JS에서 함수는 Function 객체입니다.
> ![](https://images.velog.io/images/doodream/post/a8767626-0c1c-44c5-b1c0-1c8677eaee05/image.png) > ![](https://images.velog.io/images/doodream/post/8d68077c-2df5-4e40-8ca3-4779f37ab450/image.png)

## useCallback

컴포넌트가 비교를 위해 재실행 될때 함수는 참조값이라서 React.memo가 불가능하다면 함수자체를 저장(보존)해놨다가 비교가 가능하게 만들면 되지 않을까? 이러한 의문에서 탄생한 것이 useCallback입니다.

이함수를 이용해서 컴포넌트가 재실행될때 함수객체를 다시 생성하지않고 이전에 보존된 함수를 다시 불러올수 있습니다. 참조값은 가르키는 값이 같을때 같은 값이라고 인지합니다.
![](https://images.velog.io/images/doodream/post/980c808b-d336-430e-892f-8a9742ab4d8d/image.png)

이러한 개념을 이용해서 함수를 React 구성요소 어딘가에 저장해놨다가 함수가 재실행될때 저장해놓은 요소를 가르키게 함으로서 함수를 보존합니다.

사용방법은 간단합니다. useCallback()을 이용해서 보존하고픈 함수를 래핑하면됩니다.

```jsx
const func = useCallback (function, [...dependencyArr])
```

### useCallback 사용시점

`useCallback`을 사용하지 않을때 의 Button.js 에 `React.memo`를 적용한다해도 `props`로 넘기는 `handleClickButton` 함수가 재생성되기 때문에 `React.memo`에서 새로운 props로 인식해서 함수자체의 기능변화는 없으나 Button.js 컴포넌트가 재실행 되게 됩니다. 개발자 입장에서는 재실행할 필요가 없는 컴포넌트임에도 말입니다. 따라서 `React.memo` 기능은 `props`로 넘어가는 함수객체의 비교를 할수 없기에 이를 보조하는 기능으로 **함수객체를 보존함으로서 `React.memo`로 인한 컴포넌트 재실행 방지 기능을 활성화 시키기 위해 `useCallback`을 사용합니다.**

**Button.js**
![](https://images.velog.io/images/doodream/post/ad9ee793-c10d-462f-99c7-9c85d19e199e/image.png)

**useCallBack 사용안할때 **
![](https://images.velog.io/images/doodream/post/ed42a75c-330f-4113-a5a1-a3de526e1ac2/%E1%84%87%E1%85%B5%E1%84%80%E1%85%AD4.gif)

**useCallback 사용할때**
![](https://images.velog.io/images/doodream/post/0cd779d6-4d9a-44f8-b359-d930ea87d596/image.png)
![](https://images.velog.io/images/doodream/post/d40cde1b-0653-4f85-bf0a-1eb2977fc364/%E1%84%87%E1%85%B5%E1%84%80%E1%85%AD5.gif)

### useCallback 의 종속성 배열

위의 useCallback 함수의 두번째 인수로 [] 빈배열을 넣었는데 이뜻은 어떤 경우에도 이함수는 변하지 않는다는 뜻입니다. 그렇다면 [] 안에 `a`라는 변수를 넣는다면 어떤 뜻일까요?

useCallback함수의 콜백함수 안에 a 라는 변수가 있을때 해당 a함수가 변하면 useCallback함수의 콜백함수를 재구성한다는 뜻입니다.

여기서 클로저의 개념을 짚고 넘어가야겠습니다.

> [클로저 개념 짚고가기](https://ko.javascript.info/closure)
> 모든 자바스크립트 함수는 클로저입니다. 자바스크립트 함수 내부에서는 외부의 변수에 대해 접근할 수 있는데 이 이유는 함수가 만들어질때 실행컨텍스트안에 렉시컬 환경이 있기 때문입니다. 함수는 자기가 만들어진 실행컨텍스트안의 렉시컬 환경에 접근할 수 있습니다. 함수는 렉시컬 환경의 정보들을 담을 수 있는 숨김 프로퍼티인 [[Environment]]를 갖고 있는데 이안에 외부 렉시컬 정보들을 참조 할수 있는 값이 들어갑니다. 이 프로퍼티로 인해 외부 변수들을 스코프체인 순서대로 접근할 수 있는 것입니다.

따라서 useCallback 함수로 보존된 함수는 재실행되기 전의 외부변수들에 접근할 수 있습니다.

![](https://images.velog.io/images/doodream/post/bcc147e5-1a7e-4d75-af29-6d266c7092cd/image.png)

위 코드를 살펴보면 allow Toggle 버튼을 누르면 allowToggle이 true로 바뀌고 이로인해 Click! 버튼을 누르면 showDifference 가 true로 바뀌면서 Demo의 `<p>`가 바뀌어야 할 것 같습니다만 아닙니다.
![](https://images.velog.io/images/doodream/post/97b8c3dd-b7c8-4366-b5c5-749d43ce7260/%E1%84%87%E1%85%B5%E1%84%80%E1%85%AD7.gif)

useCallback의 종속성 배열에 allowToggle이 없기 때문에 콜백함수는 여전히 allowToggle이 false인 값을 가진 함수를 보존하고 있기 때문입니다. 따라서 allowToggle을 종속성 배열에 넣어준다면
![](https://images.velog.io/images/doodream/post/0fc9e4dd-06d0-4bdb-9f4d-e7ff2864f03d/image.png)

![](https://images.velog.io/images/doodream/post/0254e1f1-4c3a-40c9-888d-db088604bfdb/%E1%84%87%E1%85%B5%E1%84%80%E1%85%AD8.gif)

이렇게 기대되로 allowToggle이 바뀌면 `useCallback` 의 콜백함수가 새로 반영되면서 위와 같이 기대한 동작이 되는 것을 볼수 있습니다.

## useMemo

`useMemo`는 저장하려는 모든 종류의 데이터를 저장할 수 있습니다. `useCallback`이 함수에 대해 함수 자체를 저장하는 것처럼 말입니다. 따라서 `prop`이 고정되는 static 데이터나 잘 변하지 않는 종류의 데이터라면 `useMemo`를 사용한다면 컴포넌트가 재실행되도 이전값과 동일하기 때문에 `React.memo`에 의해 자식 컴포넌트가 재실행되지 않을 수 있습니다. 실제의 예를 한번들어서 알아봅시다.

![](https://images.velog.io/images/doodream/post/4f653e7c-a60f-4929-9ce6-2346128bd689/image.png)
![](https://images.velog.io/images/doodream/post/27764349-c633-4d8b-a070-25547ac0b36c/image.png)
![](https://images.velog.io/images/doodream/post/b4e1635e-6958-4605-90e8-a726ea87bf74/image.png)

위 컴포넌트를 실행하면 아래와 같이 `App.js`가 실행되고 콘솔은 다음과 같이 나타 납니다.

![](https://images.velog.io/images/doodream/post/24f1a2fa-d99a-45c7-b820-9a3b2d7e7796/useMemo1.gif)

Demo 컴포넌트는 변하는 것이 없는데 왜 재실행될까요? 이유는 App.js가 재실행 될때마다 items로 넘어가는 배열값이 계속 생성되어 새로운 배열값으로 Demo 컴포넌트로 들어가기 때문입니다. 따라서 Demo의 `React.memo`는 배열값 비교를 할수가 없으므로 새로운 props가 넘어왔다고 인식해서 컴포넌트를 재실행되게 됩니다.

여기에서 `useMemo` 를 사용해 봅시다. 먼저 App.js에서 Demo로 넘어가는 items 는 고정 데이터 입니다. 바뀔일이 없기 때문에 `useDemo`를 이용해서 해당 값을 저장해봅시다.

```jsx
const value = useMemo(() => {}, []);
```

useMemo도 useCallback과 사용방법이 비슷합니다. 다른 점이라면 useCallback은 콜백함수 자체를 저장하는 방면 useMemo는 콜백함수의 반환값을 저장합니다.
![](https://images.velog.io/images/doodream/post/256d3da7-c67a-4300-9699-2e4bfa836143/image.png)
![](https://images.velog.io/images/doodream/post/7f484704-bd53-4e0b-b252-bd6ee3f309fd/useMemo2.gif)

`listItem`이 저장되어 Demo 컴포넌트가 더이상 재실행되지 않습니다.

![](https://images.velog.io/images/doodream/post/f763996a-dffa-4bfc-b964-5dee161a3e7d/image.png)

Demo 컴포넌트 안에서도 정렬하는 배열값에 대해서 useMemo를 씌워둔다면 새로운 items props가 들어오지 않는 이상 같은 items에 대해서 정렬값을 수행하지 않습니다.

## 요약

- 리액트와 리액트 DOM은 다릅니다. 리액트의 주요기능은 컴포넌트의 상태관리 입니다.
- ReactDOM은 리액트가 관리한 값들을 받아 실제 DOM으로 렌더링하기 전에 VirtualDOM에서 비교를 진행한 후 실제로 변경될 부분만 브라우저로 넘깁니다.
- 리액트 내부에서 상태변경시 컴포넌트 전체가 재실행 되는데 앱이 커지게되면 재실행하는 비용이 만만하지 않습니다. 이를 위한 최적화 기법을 소개합니다.
- React.memo : 컴포넌트의 props를 이전 값과 비교해서 변경되지 않으면 컴포넌트를 재실행하지 않습니다.
- useCallback : 콜백함수를 저장해서 종속성배열의 값들이 변하지 않는 경우 이전 함수를 그대로 호출해서 함수가 재생성되어 props로 넘어갈때 컴포넌트가 재실행되는 것을 방지합니다. 즉, React.memo의 기능을 보조합니다.
- useMemo : 콜백함수가 반환하는 값을 저장해서 종속성배열의 값들이 변하지 않는경우 이전 값을 그대로 불러와 계속해서 값이 재생성되어 props로 넘어갈때 컴포넌트가 재실행되는것을 방지합니다. 즉, React.memo의 기능을 보조합니다.

## 참고

-[Udemy 강의](http://https://www.udemy.com/course/react-the-complete-guide-incl-redux/learn/lecture/25599684#content)
