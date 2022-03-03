# TypeScript - Intersection

## 설명

`&` 기호를 이용하여 모두 만족하는 하나의 타입을 인터섹션 타입(Intersection)이라고 부른다.
<img width="571" alt="intersection-diagram 01f4fdfe" src="https://user-images.githubusercontent.com/76679130/156077918-4f1c7dea-0a99-426a-96b6-95d010f161ca.png">

### 1. 예제 코드

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

## 참고

- [타입스크립트 핸드북](https://joshua1988.github.io/ts/guide/operator.html#union-type)
