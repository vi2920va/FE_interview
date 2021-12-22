# TypeScript - Type & Interface

## 설명

type alias 와 interface는 변수나 객체의 타입 이름을 지정하는 방법입니다.

### 확장

```tsx
// type
type person = {
	name:string;
	age:number;
}
type student = person & {
	school:string;
} 

// interface
interface person {
	name:string;
	age:number;
}
interface student extends person {
	school:string;
}
```

- type 과 interface는 서로 확장이 가능하지만 확장하는 방식이 다릅니다.

### 선언적 확장

```tsx
// type
type person = {
  name:string;
  age:number;
}
// person 식별자가 중복되었다는 에러가 뜸
type person = {
  hobby:string;
}

//interface
interface person {
  name:string;
  age:number;
}

interface person {
  hobby:string
}

const user:person = {
  name:'tom',
  age:21,
  hobby:'영화보기'
}
```

- type에서는 person이란 식별자를 선언한 뒤 다시 person에 타입을 선언해서 입력하면 확장이 이루어 지지않지만 interface에서는 자동으로 확장이 이루어진다.

### interface는 객체 타입만 지정가능하고 type은 단일 변수 타입도 지정 가능하다

```tsx
// type
type name = string;
type user = {
	name:string;
}

// interface
interface name = string // Error
interface user = {
	name:string;
}
```

## 요약

- type 과 interface는 거의 비슷한 역할을 하지만 일부 다른점이 있다.
- type 과 interface는 확장을 하는 방식에서 차이가 있다.
- interface는 선언적 확장이 가능하지만 type은 불가능하다.
- interface는 객체타입만 선언가능하고 type은 단일 타입도 선언가능하다.

## 참고

- [https://www.typescriptlang.org/docs/handbook/2/everyday-types.html](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html)