# TypeScript - unkown/never/any

## any

any는 어떤 타입이든 될 수 있다. 그러나 결론부터 말하자면 **TypeScript에서 any는 가능한 사용하지 않는 것이 좋다.** 타입을 지정하는 것의 이점을 가지지 못하게 만들기 때문이다.

유일하게 허용되는 경우라면 현재 **JavaScript에서 TypeScript로 마이그레이션을 진행중**일 때 뿐이다.

만약 당장 타입을 모르는 경우라면 any대신 unknown을 사용하도록 하자.



## never

집합으로 생각했을 때 never는 0과 같다. 그래서 아래의 공식이 성립하게 된다.

```tsx
T | never ⇒ T
```

T는 알 수 없는 어떤 타입인데, never가 0이므로 어떤 것과 합집합을 하더라도 결과는 T가 된다.

`fetchPriceWithTimeout` 함수는 `fetchStock` 의 결과가 3초 이상이 지나면 에러를 던진다. 그리고 반환 타입은 `Promise<number>` 이다. 그런데 이 함수의 결과는 race의 결과에 따라서 number가 될수도 있고, 에러가 될 수도 있다.

이런 경우 `timeout`의 결과를 never로 지정해주게 되면, `stock`의 타입이 `{price: number} | never`가 된다. 즉 집합의 속성에 따라서 `{price: number}` 가 된다.

그래서 우리는 `fetchPriceWithoutTimeout`의 결과를 `{price: number} | any` 나 `{price: number} | unknown`으로 하는 대신 `number`로 지정해줄 수 있게 된다.

```tsx
function timeout(ms: number): Promise<never> {
  return new Promise((_, reject) =>
    setTimeout(() => reject(new Error("Timeout elapsed")), ms)
  )
}

async function fetchPriceWithTimeout(tickerSymbol: string): Promise<number> {
  const stock = await Promise.race([
    fetchStock(tickerSymbol), // returns `Promise<{ price: number }>`
    timeout(3000)
  ])
  return stock.price
}
```



## unknown

집합으로 생각했을 때 unknown은 1과 같다. 그래서 아래의 공식이 성립하게 된다.

```tsx
T & unknown ⇒ T
```

T는 알 수 없는 어떤 타입인데, unknown이 1이므로 어떤 것과 교집합을 하더라도 결과는 T가 된다.

unknown은 any와 다르게 타입을 구체화해주지 않으면 에러가 발생한다.

그래서 아래 코드와같이 배열인지, string인지, number인지 등을 체크해준 후에야 해당 타입처럼 사용할 수 있게 된다. (any는 이렇게 타입을 구체화하는 과정 없이 아무렇게나 사용할 수 있다.)

만약 아래 코드에서 `if (Array.isArray(x))` 이 조건문으로 타입 지정해주는 것 없이 `return "[" + x.map(prettyPrint).join(", ") + "]"` 를 호출하게 되면, ***Object is of type ‘unknown’.*** 에러가 발생하게 된다.

```tsx
function prettyPrint(x: unknown): string {
  if (Array.isArray(x)) {
    return "[" + x.map(prettyPrint).join(", ") + "]"
  }
  if (typeof x === "string") {
    return `"${x}"`
  }
  if (typeof x === "number") {
    return String(x)
  } 
  return "etc."
}
```



## 요약

- any는 타입을 무시하라고 하는 의미이므로 특별한 경우가 아니면 사용하지 말아야한다.
- never는 에러를 리턴해서 해당 값이 리턴되는 경우를 무시하고자할때 사용한다.
- unknown은 any처럼 어떤 값을 대입할 수는 있으나, 반대로 어떤 값에든 unknown타입을 대입할 수는 없다. 어떤 타입에 대입하고 싶다면 type narrowing을 통해서 타입을 구체화해주어야 한다.



## 레퍼런스

- [TypeScript에서 never과 unknown의 차이점](https://blog.logrocket.com/when-to-use-never-and-unknown-in-typescript-5e4d6c5799ad/)