# React Test

## React Test 작성하기

작성하는 코드가 원하는대로 동작하는지 확인하기 위한 테스트 방법들입니다.

### 1. 테스트 시작/ 끝맺음하기 (Jest)

테스트를 시작할때는 document에 우리가 원하는 DOM 엘리먼트를 추가해야하고, 끝날때는 cleanup 함수를 호출한 후에 document에서 트리를 언마운트 해줘야한다.

전자는 `beforeEach` 에서 후자는 `afterEach` 에서 작업하면 된다. 이것들은 각각의 테스트가 시작하기 전 후에 반드시 실행되며 테스트를 독립적으로 진행되도록 도와준다.

(React18부터 `ReactDOM.render` 가 지원하지 않으면서 root를 사용해야함. [링크 참고](https://reactjs.org/blog/2022/03/08/react-18-upgrade-guide.html#updates-to-client-rendering-apis))

```jsx
import {createRoot} from 'react-dom/client';

let container = null;
let root = null;

beforeEach(() => {
    // setup a DOM element as a render target
    container = document.createElement("div");
    root = createRoot(container);
    document.body.appendChild(container);
  });
  
afterEach(() => {
    // cleanup on exiting
    root.unmount();
    container.remove();
    container = null;
});
```

### 2. 별다른 DOM 테스팅 라이브러리 없이 작성하기

```jsx
it("changes value when clicked", async () => {
    const onChange = jest.fn(); // mocking function

    act(() => {
        root.render(<Toggle onChange={onChange} />);
    });
  
    const button = document.querySelector("[data-testid=toggle]");
    expect(button.innerHTML).toBe("Turn on");

    act(() => {
      button.click();
    });

    expect(onChange).toHaveBeenCalledTimes(1);
    expect(button.innerHTML).toBe("Turn off");
  
    act(() => {
      for (let i = 0; i < 5; i++) {
        button.dispatchEvent(new MouseEvent("click", { bubbles: true }));
      }
    });
  
    expect(onChange).toHaveBeenCalledTimes(6);
    expect(button.innerHTML).toBe("Turn on");
  });
```



## DOM 테스트 라이브러리들

### 1. Enzyme

Enzyme is a JavaScript Testing utility for React that makes it easier to test your **React Components' output**. You can also manipulate, traverse, and in some ways simulate runtime given the output.

- 컴포넌트의 props와 state 값들에 관한 테스트를 진행하고 싶을 때 사용함.
- DOM 테스트도 가능함. (testing-library보다는 더 복잡하고 어렵게 사용해야함)
- Adapter를 별도로 사용해야함.
- CRA와 사용할때 별도 설치 필요.

```jsx
describe("RangeCounterA", () => {
  let component;
  beforeEach(() => {
    component = mount(<RangeCounterA />);
  });

  it("shows range reached alert when reached limit by clicking control buttons",
    () => {
      component = mount(<RangeCounterA min={0} max={1}  />);
      component.instance().incrementCounter(); // 함수를 컴포넌트에서 직접 호출
      component.update();
      const alert = component.find('.RangeCounter__alert');
      expect(alert.text()).toEqual('Range limit reached!');
    }
  );
});
```

### 2. react-testing-library

The React Testing Library is a very light-weight solution for testing React components. It provides light utility functions on top of `react-dom` and `react-dom/test-utils`, in a way that encourages better testing practices.

- DOM 테스트를 하기에 간결함. 유저가 테스트하는 것 같은 방식으로 테스트.
- 엔자임처럼 컴포넌트 props나 state 값들에 관한 테스트를 하기엔 어려움.
- CRA 설치시 기본으로 설치됨.
- 사용하기 쉽고 편함.

```jsx
describe("RangeCounterB", () => {
  it("shows range reached alert when reached limit by clicking control buttons",
    () => {
      const { getByText } = render(<RangeCounterB min={0} max={1} />);
      const incrementButton = getByText("+");
      fireEvent.click(incrementButton); // 사용자가 클릭하는 것처럼 호출
      expect(getByText("Range limit reached!")).toBeVisible();
    }
  );
});
```



## 참고

- [react testing](https://ko.reactjs.org/docs/testing.html)
- [Enzyme vs react-testling-library](https://blog.logrocket.com/enzyme-vs-react-testing-library-a-mindset-shift/)