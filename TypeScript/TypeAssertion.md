# Typescript - TypeAssertion vs TypeAnnotation

## 설명

### 타입스크립트의 타입지정

`Typescript` 에서는 컴파일을 하며 사용자에게 타입에 대한 에러를 확인시켜줍니다. 따라서 코드자체의 동작에는 전혀 영향을 미치지 않습니다. 이렇게 변수에 타입지정을 해줄때에 두가지 방식이 있습니다.

- `TypeAssertion`
- `TypeAnnotation`

### TypeAssertion

타입스크립트는 타입을 추론해서 알아냅니다. 해당 데이터 타입을 알수 없을때에는 `any` 나 `unknown` 으로 데이터 타입을 추론하는데, 개발자 입장에서 해당 추론을 무시하고 타입을 지정해줄 수 있습니다.

마치

`내가 너보다 타입에 대해서 더 잘 알고, 나의 주장에 대해 의심하지마!` 라고 말하는 것 처럼요.

```ts
let doodream = {};
doodream.id = 123; // error
doodream.name = "doo"; // error
```

위 코드에서는 객체안에 프로퍼티가 정의되지 않았기에 타입스크립트에서는 이렇게 에러를 발생 시킵니다.
![](https://images.velog.io/images/doodream/post/5aa8940e-5649-440f-b2dd-37c51ce386a1/image.png)

이러한 에러는

```ts
interface Person {
  id: number;
  name: string;
}

let doodream = {} as Person;
doodream.id = 123;
doodream.name = "doo";
```

위와 같이 간단하게 Person을 `as` 라는 키워드로 Typeassertion 시키면 됩니다. 이렇게 되면 타입스크립트는 {} 객체가 Person 이라는 인터페이스의 타입이라고 인식하며 타입추론을 가능하게 해줍니다.

> ❗️ `as` vs `<>` 원래 타입표명은 꺽쇠 형태로서 표현되었습니다만, JSX 구문과의 혼란을 초래하여 as를 주로 사용하도록 권장되고 있습니다.

### 한계

타입표명은 말그대로 그냥 타입추론만 가능하게 즉, 자동완성만 가능하게 만들어주는 기능으로서 작용될 뿐입니다.

```ts
interface Person {
  id: number;
  name: string;
}

let doodream = {} as Person;
//doodream.id = 123;
//doodream.name = "doo";
```

위와 같은 코드에서 사용자가 기껏 타입표명을 해놔도 실제로 객체 프로퍼티를 정의하지 않은 상황에서 문제를 일으키지 않습니다. 사용자입장에서는 이러한점들을 하나하나 신경써야하며 컴파일러가 이를 알려주지 않기때문에, `버그 구멍` 이 발생할 수 있습니다.

### Type annotation

이방식은 타입을 명시적으로 지정해줍니다. 변수 뒤에 `:Type` 형태로 지정을 해주는 방식입니다.

```ts
interface Person {
  id: number;
  name: string;
}

let doodream: Person = {
  //   id: 123,
  //   name: "doo",
};
```

![](https://images.velog.io/images/doodream/post/1c827ed9-5274-482d-867e-95d168ce797b/image.png)

type assertion과는 다르게 속성 타입이 명시적으로 지정됨으로서 속성의 정의가 안되면 에러문구를 띄워줍니다. 이런식의 `버그 구멍`들을 막아줍니다.

> ❗️ 하지만 type assertion 자체가 나쁘다는 것은 아닙니다. 언급했다시피 타입스크립트의 타입추론보다 개발자가 더 해당 변수에대해 자세한 타입명세를 가지고 있다면 as 키워드로 type assertion을 사용해도 됩니다. 하지만 서로 상속관계가 아닌 전혀다른 타입으로 type assertion을 시도한다면 에러가 납니다. 이경우 any 나 unknown으로 이중 type assertion을 시도해야합니다. 되도록 좋지 않은 타입지정 형태라 최후의 방법이라고 생각합니다.

## 요약

- type assertion은 `as` 키워드를 사용하여 타입추론 보다 개발자의 타입지정을 우선적으로 컴파일될 수 있도록 해줍니다.
- type annonation은 `:Type` 형태로 타입을 명시적으로 지정하며 타입추론을 통해 정의되지 않은 타입에 대한 에러까지 표시해줍니다.

## 참고

- [타입 스크립트 공식문서](https://www.typescriptlang.org/docs/handbook/intro.html)
- [타입 스크립트 딥다이브](https://radlohead.gitbook.io/typescript-deep-dive/type-system/type-assertion)
- [Medium](https://levelup.gitconnected.com/typescript-best-practices-type-assertions-and-type-annotations-d30c0acb19ec)
