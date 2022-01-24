# TypeScript - Generics(제네릭)

## 설명

재사용 가능한 코드를 작성하는 것은 소프트웨어 엔지니어링에서 중요한 부분입니다.

Typescript에서도 단일 타입이 아닌 다양한 타입에서 작동하는 함수나 컴포넌트를 Generics(제네릭)을 통해서코드를 작성할 수 있습니다.

```tsx
function memo(content: string): string {
  return content;
}
```

- 제네릭이 없다면 memo란 함수에 특정 타입을 줘야 하고 위 함수에서는 string타입만 받는 함수가 되게 됩니다. 이렇게 된다면 다른 타입을 받아야 되는 코드에서는 재사용 할 수 없는 함수가 된다는 단점이 있습니다.

```tsx
function memo(content: any): any {
  return content;
}
```

- any타입도 모든 타입을 다 받을 수 있다는 점에서 제네릭이라고 볼 수 있습니다. 하지만 any를 사용하게 되면 **memo**란 함수에서 인자인 **content**에 string타입을 넘긴다 해도 any타입이 반환 됩니다. 그래서 우리는 무엇이 반환되는지 표시하기 위해 인자의 타입을 캡처할 방법으로 값이 아닌 타입에 적용되는 타입 변수를 사용하고 반환 될때 지정해준 타입변수를 반환타입으로 다시 사용하게 합니다.

```tsx
function memo<T>(content: T): T {
  return content;
}
```

- 위 함수에 T라는 타입을 추가해주면 함수를 호출할 때 넘긴 타입에 대해 타입스크립트가 추정할 수 있기 때문에 들어갈때와 나갈때 타입을 검증할 수 있게 됩니다.

```tsx
function textLength<T>(arg: T): number {
  return arg.length; // 'T' 형식에 'length' 속성이 없습니다.
}

function textLength<T>(arg: T[]): number {
  return arg.length;
}
```

- 위 함수에 text의 길이를 리턴해주는 함수가 있다고 했을 때 제네릭 T가 length가 없는 number일 경우도 있습니다. 그러므로 에러를 리턴해줍니다.
- 배열은 length를 가지기 때문에 T타입을 T[]로 사용해서 number[] 형식으로 사용할 수 있습니다.

### 제네릭 예시

```tsx
interface IUser<T> {
	name:string;
	age:number;
	hobby:T;
}

const minsu:IUser<string> = {
	name:'민수';
	age:27;
	hobby:'축구';
}

const sumin:IUser<Array> = {
	name:'수민';
	age:30;
	hobby:['영화보기','독서'];
}
```

- interface에 제네릭 타입을 부여해 좀 더 유연하게 객체에 타입을 지정해 줄 수 있습니다.

```tsx
const request = {
  post<params>(path: string, data: params): Promise<AxiosResponse<any>> {
    return axios.post(url, data);
  },
};
```

- 위 request객체에서 axios post메소드의 data파라미터는 중간에 타입이 변경되면 안되기 때문에 <params>란 제네릭 타입을 지정해줘서 타입체크를 해줍니다.

## 요약

- 제네릭은 여러 타입에서 동작하는 함수나 컴포넌트를 만드는데 사용된다.

## 참고

[https://www.typescriptlang.org/docs/handbook/2/generics.html](https://www.typescriptlang.org/docs/handbook/2/generics.html)
