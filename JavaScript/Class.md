# JavaScript - Class

## 설명

자바스크립트는 프로토타입 기반 언어라서 '상속' 개념이 존재하지 않습니다. 이는 클래스 기반의 다른 언어에 익숙한 많은 개발자들을 혼란스럽게 했고, 따라서 클래스와 비슷하게 동작하게끔 흉내 내느 여러 기법들이 탄생했으며 이들 중 몇 가지는 널리 알려져 있습니다. 이러한 니즈에 따라 결국 ES6에는 클래스 문법이 추가됐습니다.

### 1. 클래스

class 선언은 프로토타입 기반 상속을 사용하여, 주어진 이름의 새로운 클래스를 만듭니다.

```js
class Polygon {
  constructor(height, width) {
    this.area = height * width;
  }
}

console.log(new Polygon(4, 3).area);
// expected output: 12
```

클래스에서는 인스턴스에서는 직접 접근할 수 없는, 클래스 자체에서만 접근 가능한 스테틱 메서드와(e.g. Array.isArray() -[MDN](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/isArray)) 인스턴스에서 직접 활용할 수 있는 프로토타입 메서드가 있습니다.

### 2. 클래스 상속

![image](http://www.tcpschool.com/lectures/img_java_inheritance_diagram.png)

상속(inheritance)이란 기존의 클래스에 기능을 추가하거나 재정의하여 새로운 클래스를 정의하는 것을 의미합니다.

이러한 상속은 캡슐화, 추상화와 더불어 객체 지향 프로그래밍을 구성하는 중요한 특징 중 하나입니다.

상속을 이용하면 기존에 정의되어 있는 클래스의 모든 필드와 메소드를 물려받아, 새로운 클래스를 생성할 수 있습니다.

이때 기존에 정의되어 있던 클래스를 부모 클래스(parent class) 또는 상위 클래스(super class), 기초 클래스(base class)라고도합니다.

그리고 상속을 통해 새롭게 작성되는 클래스를 자식 클래스(child class) 또는 하위 클래스(sub class), 파생 클래스(derived class)라고도 합니다.

<hr>
ES6가 나오기 전, 개발자들은 다른 언어의 클래스 상속을 JS에서 따라하고자 주로 3가지 방법을 사용했습니다. 그 방법들로는,
<br />
<br />

1. SubClass.prototype에 SuperClass의 인스턴스를 할당한 다음 프로퍼티를 모두 삭제하는 방법
2. 빈 함수(Bridge)를 활용하는 방법
3. Object.create를 이용하는 방법입니다.

위 세 가지 방법 모두 constructor 프로퍼티가 원래의 생성자 함수를 바라보도록 조정해야 합니다.

<br />

이렇게 복잡해 보이는 상속 방법은 ES6가 나오면서 class문법으로 간단히 처리가 가능해졌습니다.

ES6에서는 extends 키워드 하나로 클래스 상속이 가능합니다.

```js
class Animal {
  constructor(name) {
    this.speed = 0;
    this.name = name;
  }
  run(speed) {
    this.speed = speed;
    alert(`${this.name} 은/는 속도 ${this.speed}로 달립니다.`);
  }
}

let animal = new Animal("동물");
```

```js
class Rabbit extends Animal {
  hide() {
    alert(`${this.name} 이/가 숨었습니다!`);
  }
}

let rabbit = new Rabbit("흰 토끼");

rabbit.run(5); // 흰 토끼 은/는 속도 5로 달립니다.
rabbit.hide(); // 흰 토끼 이/가 숨었습니다!
```

### 3. Class vs Prototype

앞서 class 문법에 대한 개발자들의 니즈가 있었고,
관련 기능들을 프로토타입을 이용하여 구현했다고 설명했습니다.

그렇다면 클래스 문법은 프로토타입의 syntactic sugar그 이상 그 이하도 아닌걸까요?

결론부터 말하자면 아닙니다.
둘 사이의 차이점은 분명히 존재합니다.

#### 차이 1

클래스는 설계도라서 한번 만든 설계도는 도중에 바뀌는 일이 없습니다. class 블록 안에 내가 적은 코드는 프로그램이 시작되면 runtime에 바뀌는 일이 없고, 같은 설계도에서 찍어낸 인스턴스는 실행 중에 property나 methods 를 추가하지 않는 한 동일한 형태를 가집니다.

반면 prototype 기반 언어는 객체가 곧 설계도가 됩니다. 객체는 runtime 중에 동적으로 속성추가가 가능하므로 동적인 설계도가 가능합니다.

```js
var cat = { leg: 4 };
var metamon = {};

Object.setPrototypeOf(metamon, cat);
console.log(metamon.leg); // 4

// prototype 객체 cat에 메소드 추가
cat.speak = function () {
  return "Miew";
};
console.log(metamon.speak()); // Miew
// prototype 객체를 다른 prototype 객체로 바꿔치기 할 수도 있습니다.

var cat = {
  speak() {
    return "Miew";
  },
};
var human = {
  speak() {
    return "Hello";
  },
};
var metamon = {};

Object.setPrototypeOf(metamon, cat); // 메타몽의 프로토타입을 cat으로
console.log(metamon.speak()); // Miew

Object.setPrototypeOf(metamon, human); // 메타몽의 프로토타입을 human으로
console.log(metamon.speak()); // Hello
```

#### 차이 2

class 에서 상속은 부모 클래스와 자식 클래스간의 관계를 말합니다.
prototype 에서의 상속은 prototype 객체와 그것을 상속하는 객체와의 관계를 말합니다.

```js
function Shape() {
  this.x = 0;
  this.y = 0;
}
Shape.prototype.move = function (x, y) {
  this.x += x;
  this.y += y;
  console.info("Shape moved to : ", this.x, this.y);
};
var shape = new Shape();
shape.move(1, 3);
// new 카워드를 생성자 함수 앞에서 사용하는 건 사실 다음과 같은 일이 벌어지는 것입니다.

// 이건 사실
var shape = new Shape();

// 이것과 같습니다.
var shape = new Object(); // 빈 객체를 생성
Object.setPrototypeOf(shape, Shape.prototype); // 상속할 프로토타입 설정
Shape.call(shape); // shape객체를 this로 생성자함수를 실행
```

보면 아시겠지만 prototype 상속은 class 로 따지면 constructor 단계에서 일어나는 일일 뿐입니다. 새로운 객체를 생성할때 class 방식은 하나의 클래스에서 바로 인스턴스가 나오지만, prototype 방식은 prototype 객체 참조를 상속한 후에 인스턴스를 만드는 것입니다.

class 의 부모 자식 상속관계를 구현하려면 prototype chaining 을 사용해야합니다.

## 요약

클래스는 어떤 사물의 공통 속성을 모아 정의한 추상적인 개념이고, 인스턴스는 클래스의 속성을 지니는 구체적인 사례입니다. 상위 클래스(superclass)의 조건을 충족하면서 더욱 구체적인 조건이 추가된 것을 하위 클래스(subclass)라고 합니다.

## 참고

- [MDN_class](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Statements/class)
- [MDN_classes](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Classes)
- [코어자바스크립트](http://www.yes24.com/Product/Goods/78586788)
- [Medium](https://medium.com/@flqjsl/js-class-%EC%99%80-prototype-%EC%9D%98-%EC%B0%A8%EC%9D%B4-7dc1d7531ae0)
- [Youtube](https://www.youtube.com/watch?v=XoQKXDWbL1M)
- [tcpschool](http://www.tcpschool.com/java/java_inheritance_concept)
- [코어 자바스크립트](https://ko.javascript.info/class-inheritance)
- [Medium](https://medium.com/javascript-scene/master-the-javascript-interview-what-s-the-difference-between-class-prototypal-inheritance-e4cd0a7562e9)
- [velog](https://velog.io/@wpark/class-vs-prototype)
- [코딩앙마](https://www.youtube.com/watch?v=OpvtD7ELMQo&t=299s)
