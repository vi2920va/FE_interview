# JavaScript - This

## 설명

### this의 정의

- this란 함수의 실행 컨텍스트를 가리키는 예약어입니다.
- 여기서 실행 컨텍스트란 '함수가 실행되는 환경'입니다.
- 다른 언어와 다르게 자바스크립트의 this는 상황에 따라 다른 값들을 가리킵니다.

### 일반함수와 메소드에서의 this

자바스크립트에서 this는 기본적으로 함수내에서 전역객체를(window) 가리킵니다.
하지만 객체의 메소드에서의 this는 메소드앞 객체를 가리킵니다.

```javascript
// 일반 함수의 this와 메소드에서 this 예시

console.log(this) // window

function test(){
  console.log(this) // window
}

let age = 20
const user = {
  age: 21,
  setAge: function () {
    this.age = 22;
  }
}
user.setAge()

console.log(age) // 20(1번)
console.log(user.age) // 22(2번)
```

setAge 메소드에서 this.age를 22로 변경해주는데 객체의 메소드에서 this를 사용했기 때문에 this는 user이고 user.age를 22로 변경 해주게 됩니다.
그래서 user 밖 age는 20 user.age는 22로 변경되어 출력됩니다.

### 생성자 함수에서의 this

```javascript
// 생성자 함수에서의 this 예시

function User(name,age) {
  this.name = name
  this.age = age
}

const user = new User('순이',20)
console.log(user); // user {name: '순이', age: 20}
```

new로 호출하는 객체의 생성자 함수에서 this도 생성자 함수가 생성하는 객체를 가리킵니다.
그래서 위 코드에서 this.name,this.age는 User.name,User.age로 인식되어 코드가 작동하게 됩니다.

### Call, Apply, Bind에서의 this

Call, Apply, Bind 메소드 사용 시, 메소드에 첫 번째 인수로 전달하는 객체에 바인딩 됩니다.
Bind는 Call, Apply와 다르게 this의 값을 고정 시킬 수 있습니다. 그래서 Bind로 this값을 지정해두면 call, apply와 함께 호출되더라도 this의 값은 Bind로 지정한 this로 고정되게 됩니다.

```javascript
// Call, Apply, Bind 예시

const user1 = { name : '철수' }
const user2 = { name : '영희' }
const user3 = { name : '민수' }

function update(age){
  this.age = age
};

// bind
const userUpdate = update.bind(user1)
userUpdate(20)

// call, apply
userUpdate.call(user2,22)
userUpdate.apply(user3,[23])

// bind로 userUpdate에서 this를 user1로 고정시켰기 때문에 call과 apply에 user2,user3으로 
// this의 값을 지정해주어도 bind로 고정해준 user1에서 값이 변경된다.
console.log(user1); // { name: '철수', age; 23 }
console.log(user2); // { name: '영희' }
console.log(user3); // { name: '민수' }
```

## 화살표 함수에서의 this

- 화살표 함수는 일반 함수와 달리 고유한 this를 가지고 있지 않습니다.
- 화살표 함수 안에서 this는 화살표 함수 외부의 this값을 가져오게됩니다.
- 화살표 함수는 call, apply, bind 메소드를 사용하여 this를 변경할 수 없습니다.

```javascript
// 화살표 함수에서 this 예시

const obj = {
  a: function () {
    (function () {
        console.log(this); // window
      }
    )(),
    (()=>{
      console.log(this); // obj
    })()
  }
}
obj.a()
```

메소드 내부의 즉시 실행 함수는 함수이기 때문에 this는 전역객체를 가리킵니다.
하지만 똑같이 밑에 화살표함수를 보면 this는 a메소드의 this로 obj를 가리키게됩니다.

## 요약

- this란 함수의 실행 컨텍스트를 가리키는 예약어입니다.
- 기본적으로 this는 글로벌객체(window)와 바인딩된다.
- 객체의 메소드에서 this는 메소드를 호출한 객체와 바인딩된다.
- 생성자 함수 내부에서 this는 생성자 함수가 생성할 객체와 바인딩된다.
- 함수를 선언할 때 this에 바인딩 할 객체가 정적으로 결정되는 것이 아니고, 함수를 호출할 때 함수가 어떻게 호출되었는지에 따라 this에 바인딩 할 객체가 동적으로 결정된다.
- 화살표 함수는 함수를 선언할 때 this에 바인딩 할 객체가 정적으로 결정된다.
- 화살표 함수의 this는 언제나 상위 스코프의 this를 가리킨다. 이를 Lexical this라 한다.
- 화살표 함수는 call, apply, bind 메소드를 사용하여 this를 변경할 수 없다.

## 참고

- [MDN - This](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/this)
- [모던 JavaScript - 메서드와 this](https://ko.javascript.info/object-methods)