# JavaScript - Array.forEach VS Array.map

## 설명

### 1. Array.forEach()

`array.forEach()`메서드는 주어진 배열 요소 각각에 대하여 실행합니다.

- `array.forEach()`는 배열 전체를 순회하므로 중간에 순회하는 걸 멈출 수(break) 없습니다.

```javascript
// Syntax
array.forEach(callback(currentValue[index[, array]])[thisArg])

//---------------------------------------------------------------------
// callback        : 각 요소에 대해 실행할 함수는 세 가지 매개변수를 받습니다.
// currentValue    : 처리할 현재 요소
// index(option)   : 처리할 현재 인덱스
// array(option)   : map를 호출한 배열
// thisArg(option) : callback을 실행할 때 this로 사용할 값
```

### 2. Array.map()

- `Array.map()`메서드는 배열 내의 모든 요소 각각에 대하여 주어진 함수를 호출한 결과를 모아 새로운 배열을 반환합니다.

```javascript
// Syntax
array.map(callback(currentValue[, index[, array]])[, thisArg])
//---------------------------------------------------------------------
// callback        : 각 요소에 대해 실행할 함수는 세 가지 매개변수를 받습니다.
// currentValue    : 처리할 현재 요소
// index(option)   : 처리할 현재 인덱스
// array(option)   : forEach를 호출한 배열
// thisArg(option) : callback을 실행할 때 this로 사용할 값
```

## 요약

즉, `array.forEach()`는 반환된 결과를 따로 모아서 처리하는 값이 없기 때문에 `return`을 사용하게 되면 `undefined`가 할당된다.

## 참고

- Array.prototype.forEach [→ (MDN)](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach)
- Array.prototype.map [→ (MDN)](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/map)
