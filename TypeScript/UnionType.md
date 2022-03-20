# TypesScript - Union Type

## 설명

아래의 예시 코드 `getAge()` 함수의 매개변수에 `age`에 `|` 기호를 이용하여 A 이거나 B 라는 타입을 정의하는 게 유니온 타입(Union Type)

```tsx
// Union
function getAge(age: string | number) {
  if (typeof age === 'string') {
    console.log(`result age string ${age}`);
  }

  if (typeof age === 'number') {
    console.log(`result age number ${age}`);
  }
}
getAge('21');
```

### 1. 유니온 타입 사용시 주의사항

- 유니온 타입은 JavaScript의 OR(||)와 같지 않다.
- 인터페이스(interface)를 같은 타입을 다룰 경우 아래의 논리적 사고 주의해야한다.
- 함수를 호출 할 때 매개변수로 속성이 정의된 객체를 받으면 어느 타입이 오는 지 알수가 없기 때문에, 어느 타입이 오던 공통된 타입의 방향으로 오류를 안나게 하는게 것이 좋다.

```tsx
interface Person {
  name: string;
  age: number;
}

interface Developer {
  name: string;
  skill: string[];
}

function introduce(someone: Person | Developer) {
  someone.name; // 정상 동작
  someone.age; // 타입 오류
  someone.skils; // 타입 오류
}
```

### 2. 유니온 타입을 사용한 예시

```tsx
interface Loading {
  status: 'loading';
}

interface Faile {
  status: 'faile';
  code: number;
}

interface Success {
  status: 'success';
  response: any;
}

function introduce(state: Loading | Faile | Success) {
  switch (state.status) {
    case 'loading':
      return '...로딩중';
    case 'faile':
      return state.code;
    case 'success':
      return state.response;
  }
}
```

## 요약

- A 이거나 B를 만족하는 타입을 **유니온 타입** 이라고 부른다.

## 참고

- [타입스크립트 핸드북](https://joshua1988.github.io/ts/guide/operator.html#union-type)
- [타입스크립트 핸드북 ES](https://www.typescriptlang.org/docs/handbook/typescript-in-5-minutes-func.html#unions)
