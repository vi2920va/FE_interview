# TypeScript의 Type들

## TypeScript  Types

| Type    | Description                                                  | Example                                             |
| ------- | ------------------------------------------------------------ | --------------------------------------------------- |
| number  | 자바스크립트의 원시 타입                                     | 1,2,-1,0.4                                          |
| string  | 자바스크립트의 원시 타입                                     | 'hello'                                             |
| boolean | 자바스크립트의 원시 타입                                     | true, false                                         |
| object  | 자바스크립트의 객체와 유사함.                                | `const person: {name: string} = {name: 'Max'}`      |
| array   | string[], number[] 등처럼 배열 내부에 들어갈 원소들의 타입을 지정해줄 수도 있다. | `const students: string[] = ['Kate', 'Max', 'Bob']` |
| tuple   | 타입스크립트에만 있으며, 길이가 정해진 배열.                 | `let role: [number, string];`                       |
| enum    | 타입스크립트에만 있으며, 자동적으로 숫자를 넣어줌. 오른쪽 예시같은 경우에는 NEW, OLD에 각각 0,1이라는 숫자가 들어감. | `enum Type {NEW, OLD}`                              |
| any     | 아무 타입이나 될 수 있다. (그러나 남용하면 타입스크립트를 사용하는 의미가 없으므로 적당히 사용해야함.) |                                                     |
| union   | A 또는 B 타입 등 다수의 타입을 가질 수 있는 경우 사용.       | `let id: number | string;`                          |

## Type Alias

타입 이름에 별명을 붙여줄 수 있다.

EX)

- number 타입을 Age라고 별명짓기
- `number|string` 의 유니언 타입을 `Combinable` 이라고 이름짓기
- `type Combinable = number | string;`

## let과 const

const로 선언한 값은 선언과 동시에 값을 대입한다. 한번 대입된 값은 변하지 않으므로 타입스크립트에서는 대입한 값 자체가 하나의 타입이 된다.

반면 let은 선언과 동시에 값을 대입하는 경우 타입 유추를 통해 어떤 타입인지 결정한다. 만약 변수를 선언하되 바로 값을 대입하지 않으면 `any` 타입으로 간주된다.

## 함수

함수에서 void타입을 가지는 것은 리턴값을 무시하라는 의미다. 그래서 void 타입 함수에서 값을 리턴한다고 해도 별다른 에러가 발생하지는 않는다.

## unknown

어떤 타입의 값을 넣어도 에러가 나지 않는다. any 타입과 유사하다.

unknown과 any의 차이점은 unknown이 조금 더 제한적이라는 것이다.

- unkown은 타입 자체가 unknown이므로 string타입을 가진 변수에 unknown타입을 가진 변수를 대입하면 에러가 발생한다.
- any 타입은 string 타입을 가진 변수에 대입해도 에러가 발생하지 않는다.