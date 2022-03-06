# TypeScript - Intersection

## 설명

아래의 예시 코드 처럼 `Person` 과 `Developer` 를 둘 다 만족하는 하나의 타입의
`&` 기호를 이용하여 정의하는게 인터섹션 타입(Intersection)이다.

```tsx
interface Person {
  name: string;
  age: number;
}

interface Developer {
  name: string;
  skills: string[];
}

type Com = Person & Developer;

const D1: Com = {
  name: 'viva',
  age: 21,
  skills: ['html', 'css'],
};
console.log(D1); // { name: 'viva',....}
```

## 요약

- 모두 만족하는 하나의 타입을 인터섹션 타입
- 인터페이스 타입을 먼저 정의한 후에 `&` 연산자를 이용하여 정의

## 참고

- [타입스크립트 핸드북](https://joshua1988.github.io/ts/guide/operator.html#union-type)
