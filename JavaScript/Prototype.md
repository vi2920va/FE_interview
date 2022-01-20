# JavaScript - Prototype

## 설명

### 1. OOP

자바스크립트에서 객체 지향이라는 것은 다양하게 설명이 가능합니다.

1. 상속
2. 다형성
3. 추상화
4. 캡슐화

이중에서 상속에 대해서 자바스크립트는 [[Prototype]] 이라는 특이한 성질을 가집니다.

### 2. prototype & `__proto__` & [[Prototype]]

![](https://images.velog.io/images/doodream/post/ff5a8b3d-4a34-4f38-ac38-fe0763ade307/image.png)

자바스크립트의 모든 객체는 [[Prototype]]이라는 숨김 프로퍼티를 갖습니다. 보통 이러한 값은 `null` 값이나 참조하는 객체를 가지는데 참조하는 객체를 가지는 경우 해당하는 객체 자체를 본 객체의 [[Prototype]]이라고 부릅니다.

- 객체에서 어떠한 변수에 접근할때 먼저 해당 객체에 그러한 프로퍼티가 있는지 찾고 없으면 해당 객체의 [[Prototype]]에서 해당 변수를 찾습니다. 이러한 특성을 `프로토 타입 체이닝`이라고 합니다.

- `__proto__`은 [[Prototype]]을 설정하고 불러오기 위한 getter이자 setter의 이름입니다. 따라서 [[Prototype]]를 설정하는데 있어 `__proto__`를 보게 됩니다.

여기서 `prototype`과 `[[Prototype]]`의 정확한 차이점에 대해서 짚고 넘어갑시다. 일단 prototype은 일반적인 프로퍼티 입니다. 그냥 이름만 prototype인 일반적인 프로퍼티 입니다. 하지만 몇가지 특성이 있습니다.

- prototype은 함수에서 개발자가 따로 설정하지 않아도 생성되어 있으며 constructor 하나만 있는 객체를 가르키고 있습니다.

- prototype은 함수(클래스) 안에서만 존재합니다. 이로인해 만들어진 객체에서는 존재하지 않습니다.
  > ❗️객체안에서 [[Prototype]]을 조작하는 방식으로는 `__proto__`를 사용해야 합니다만, 이러한조작은 객체의 [[Prototype]]을 그때 그때 변경하기 때문에 관련 접근 코드의 최적화를 망치기 때문에 매우 느려집니다.

```js
let animal = {
  eats: true,
};

function Rabbit(name) {
  this.name = name;
}

Rabbit.prototype = animal;

let rabbit = new Rabbit("White Rabbit");
```

Rabbit.prototype은 `new Rabbit` 로 생성한 인스턴스의 `constructor` 를 animal로 설정하라는 기능을 담고 있습니다.

> `new Rabbit()`을 실행하는 순간 Rabbit함수가 실행됩니다.

- Rabbit 함수안에 빈 객체가 생성되고 this에 빈객체를 할당합니다.
- Rabbit 함수의 본문을 실행시키며 this 객체를 수정합니다.
- 수정된 this 객체를 반환하여 rabbit에 할당합니다. 따라서 rabbit은 다음과 같은 코드를 갖습니다.
- 추가로 이렇게 생성된 객체의 [[Prototype]]은 Rabbit 함수의 prototype 프로퍼티를 가르킵니다.

```js
let rabbit = {
  name: "White Rabbit";
}
```

- 추가로 Rabbit의 prototype 프로퍼티는 더이상 `{ constructor: function Rabbit }`을 가르키지 않고 animal 객체를 가르킵니다. 다만 Rabbit의 [[Prototype]]은 여전히 자기 자신을 가르킵니다.
- 만약에 `Rabbit.prototype = animal` 코드 없었다면 rabbit의 [[Prototype]]은 Rabbit 함수의 prototype 프로퍼티를 가르켰습니다.

![](https://images.velog.io/images/doodream/post/c7c1ff95-bd30-4aa4-ba98-847d93e716a4/image.png)

위와 같이 Rabbit의 prototype이라는 프로퍼티는 animal를 가리키고 있습니다. 동시에 해당기능으로 rabbit 인스턴스의 [[Prototype]]은 animal를 가르키고 있습니다.

class는 함수의 한부분이므로 class가 선언될 때 [[Prototype]]가 생겨납니다.

```js
"use strict";

const Person = function (firstName, age) {
  this.firstName = firstName;
  this.age = age;
};

const doodream = new Person("doodream", 20);
const uni = new Person("uni", 21);

Person.prototype.calcAge = function () {
  return this.age - 10;
};

console.log(doodream.calcAge());
console.log(uni.calcAge());
doodream.__proto__.printA = function () {
  console.log("A");
};
console.log(doodream.printA());
console.log(uni.printA());
console.log(doodream.__proto__ === Person.prototype);
// true
console.log(Person.prototype.isPrototypeOf(doodream));
// true
```

위 코드를 보면 Person이라는 함수가 있습니다. 그리고 doodream, uni라는 변수에 `new` 라는 키워드로 객체를 생성합니다.

이 [[Prototype]]은 숨김 프로퍼티 입니다. doodream, uni는 Person 함수에서 `new` 키워드로 생성된 객체입니다. 하지만 이렇게 생성된 객체의 [[Prototype]]는 Person의 prototype 프로퍼티인 `{ constructor : Person }`을 가르킵니다.

이렇게 파생된 객체 단위에서도 prototype을 수정할 수 있습니다.
`__proto__`프로퍼티를 사용하면 인스턴스단위에서 prototype을 수정할 수 있습니다. 이렇게 수정된 prototype은 상속되어가는 prototype에 영향을 끼쳐 이객체를 상속 받은 모든 인스턴스에서 수정됩니다.

> ❗️ 위에서 언급했다시피 이러한 수정은 [[Prototype]] 접근 연산에 악영향을 끼치므로 속도를 위해서는 수정하지 않는것이 좋습니다.

new Person()으로 부터 객체가 파생된 순간에 생겨난 객체의 [[Prototype]]가 Person의 prototype 프로퍼티를 가르킵니다.

**Person.protptype**

![](https://images.velog.io/images/doodream/post/21dd3279-3b3d-490b-9a64-8409453ff410/image.png)

Person은 함수로서 다음과 같은 익명 함수를 가르키는 [[Property]]가 존재 합니다.

---

**doodream 객체의 [[Prototype]]**

![](https://images.velog.io/images/doodream/post/12c20833-a86c-4a7f-bd40-e66db0d507f8/image.png)

---

```js
Array.prototype.unique = function () {
  return [...new Set(this)];
};

const arr = [1, 2, 3, 4, 5, 5, 5];
console.log(arr.unique());
// [1, 2, 3, 4, 5];
```

### 4. 폴리필

사실 위 코드는 네이티브 프로토 타입을 변경한 것입니다.
모던 자바스크립트에서 네이티브 프로토 타입을 변경할 때에는 폴리필을 만들때입니다.

**폴리필은 자바스크립트 명세서에 나와있는 메서드와 동일한 기능을 하는 메서드 구현체 입니다.**
즉, 특정 브라우저의 자바스크립트 엔진에서 자바스크립트 명세서에 나와있는 기능이 구현되지 않을때 이러한 기능을 폴리필을 만들어 내장 프로토 타입에 추가할 때만 네이티브 프로토 타입을 변경하는 것을 변경합니다.

- 강제로 경고가 뜬다거나 에러가 나진 않습니다. 코드 관행으로서 네이티브 프로토 타입을 변경하는 것은 코드 전체에 매우 안좋은 영향을 끼칩니다. 이는 코드 전체에 전역으로 영향을 끼치기 때문입니다.

### class.isPrototypeOf(object)

```js
console.log(Person.prototype.isPrototypeOf(doodream));
// true
```

프로퍼티가 맞나 확인해주는 빌트인 함수입니다..

### object.hasOwnProperty('property')

```js
console.dir(doodream.hasOwnProperty("printA"));
//false
```

객체에 프로퍼티이름을 문자열로 넣으면 객체 자신의 프로퍼티인지 [[Prototype]]의 프로퍼티인지 확인해주는 함수이다. 즉, static 메서드나 프로퍼티를 확인 할수 있다.

### prototype chaining

이렇게 모든 객체들은 프로토타입을 이용해서 상속을 받을수 있습니다. 결국에 [[Prototype]] 의 [[Prototype]] 의 [[Prototype]] 등.. 깊이 있는 [[Prototype]]를 갖게 될 것입니다. 이렇게 하나의 객체가 프로퍼티에 접근할 때 자신의 프로퍼티가 아닌 [[Prototype]]의 프로퍼티를 찾으려고 검색해나갈 때 prototype chaining을 합니다.

### constructor 프로퍼티

함수에는 개발자가 따로 지정하지 않아도 기본적으로 `prototype` 프로퍼티가 있습니다. 따로 여기에 추가를 하지 않는 이상 `constructor`라는 프로퍼티가 있는데, 이 프로퍼티는 바로 함수 그 자기 자신을 가르키게 됩니다.

```js
const Person = function (firstName, age) {
  this.firstName = firstName;
  this.age = age;
};

console.log(Person.prototype.constructor);
/* 
ƒ (firstName, age) {
  this.firstName = firstName;
  this.age = age;
}
*/
const ddochi = new Person.prototype.constructor("ddochi", 10);
console.log(ddochi);
const designer = new ddochi.constructor("designer", 23);
console.log(designer);
```

이렇게 생성자 함수대신 constructor를 사용하여 객체를 생성해 낼수도 있습니다. constructor는 함수 자체이므로 위와 같은 표현이 가능합니다.

이것으로 무엇에서 파생되었는지 모르는 인스턴스로 해당 인스턴스와 같은 객체를 생성해 낼수도 있습니다.

> `new` 키워드는 자동으로 클래스의 `constructor` 함수를 호출 합니다.

## 요약

- Prototype : 자바스크립트의 객체에서는 [[Prototype]] 이라는 숨김 프로퍼티를 갖습니다. 이숨김 프로퍼티값이 null 이거나 다른 객체에 의한 참조가 되는데, 다른 객체를 참조하는 경우 참조 대상을 `프로토타입(prototype)` 이라고 부릅니다.
- 클래스도 함수의 일부분 이므로 prototype 이 생겨납니다.
- 프로토 타입 체이닝이란 특정 프로퍼티에 접근할때 부모로 부터 파생된 [[Prototype]]을 가리키는데 이과정이 반복되어 올라가는 것을 말 합니다.

## 참고

- [Udemy 강의](http://https://www.udemy.com/course/react-the-complete-guide-incl-redux/learn/lecture/25599684#content)
- [코어 자바스크립트](https://ko.javascript.info/prototype-inheritance)
