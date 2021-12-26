# JavaScript - Closure

## 설명

### 클로저란

클로저는 여러 함수형 프로그래밍 언어에서 등장하는 보편적인 특성입니다.

“A closure is the combination of a function and the lexical environment within which that function was declared.”
클로저는 함수와 그 함수가 선언됐을 때의 렉시컬 환경(Lexical environment)과의 조합이다. - by MDN

클로저란 어떤 함수에서 선언한 변수를 참조하는 내부함수를 **외부로 전달**할 경우, 함수의 실행 컨텍스트가 종료된 후에도 해당 변수가 사라지지 않는 현상입니다. - by 코어 자바스크립트

함수를 선언할 때 만들어지는 유효범위가 사라진 후에도 호출할 수 있는 함수 - by 자바스크립트 닌자 비급(존 레식)

이미 생명주기가 끝난 외부함수의 변수를 참조하는 함수 - by 인사이드 자바스크립트(송형주, 고현준)

자신이 생성될 때의 스코프에서 알 수 있었던 변수들 중 언젠가 자신이 실행될 때 사용할 변수들만을 기억하여 유지시키는 함수 - by 함수형 자바스크립트 프로그래밍(유인동)

### 클로저 예시

#### 예시 1

```javascript{ .numberLines}
// 출처: 코어 자바스크립트

var outer = function(){
    var a = 1;
    var inner = funtion(){
        return ++a;
    };
    // 결과가 아닌 함수 자체를 반환
      return inner;
};

var outer2 = outer(); // outer함수의 실행 컨텍스트 종료
console.log(outer2()); // 2
console.log(outer2()); // 3

```

#### 예시 2, 3: return 없이도 클로저가 발생하는 다양한 경우

##### 예시 2: setInterval, setTimeout

```javascript{.numberLines}
// 출처: 코어 자바스크립트

// 별도의 외부객체인 window의 메서드(setTimeout 또는 setInterval)에 전달할 콜백 함수 내부에서 지역변수를 참조합니다.

(function(){
    var a = 0;
    var intervalId=null;
    var inner = function(){
        if(++a>=10){
            clearInterval(intervalId);
        }
        console.log(a);
    };
    intervalId = setInterval(inner,1000);
})();


```

##### 예시 3: eventListener

```javascript{ .numberLines}
// 출처: 코어 자바스크립트

// 별도의 외부객체인 DOM의 메서드(addEventListener)에 등록할 핸들러 함수 내부에서 지역변수를 참조합니다.

(function(){
    var count = 0;
    var button = document.createElement("button");
    button.innerText = "click";
    button.addEventListener("click", funtion(){
        console.log(++count, "times clicked");
    });
    document.body.appendChild(button);

})();

```

### 클로저가 발생하는 이유

위에서 작성한 예시 1을 통해 클로저가 발생하는 이유에 대해 설명하겠습니다.

예시 1의 12번째 줄에서 outer함수의 실행 컨텍스트가 종료되었음에도 13, 14번째 줄에서 outer함수의 LexicalEnvironment에 접근할 수 있었던 이유는 **가비지 컬렉터(gc)** 덕분입니다.
<br />
가비지 컬렉터는 어떤 값을 참조하는 변수가 하나라도 있다면 그 값은 수집 대상에 포함시키지 않습니다.

예제 1의 outer함수는 실행 종료 시점에 inner함수를 반환합니다.
외부함수인 outer의 실행이 종료되더라도 내부 함수인 inner함수는 언젠간 outer2를 실행함으로써 화출될 가능성이 열린 것입니다.

언젠가 inner함수의 실행 컨텍스트가 활성화되면 outerEnvironmentReference가 outer함수의 LexicalEnvironment를 필요로 할 것이므로 수집대상에서 제외됩니다.

그 덕에 inner함수가 a변수에 접근할 수 있게 됩니다.

### 클로저의 장점

1. 전역변수를 줄일 수 있습니다.
2. 비슷한 형태의 코드를 재사용률을 높일 수 있습니다.

### 주의해야 할 점

#### 1. for문에 var를 쓸 때는 조심해야합니다.

   <br />
   var는 function scope이기 때문에 for문에서 var를 사용하면 전역변수로 선언됩니다.
그래서 for문의 끝이나고도 i를 호출할 수 있습니다.

##### 예시

```javascript{ .numberLines}
// 출처: https://velog.io/@draw/for%EB%AC%B8%EC%97%90%EC%84%9C-var%EB%A5%BC-%EC%82%AC%EC%9A%A9%ED%95%98%EB%A9%B4-%EC%83%9D%EA%B8%B0%EB%8A%94-%EB%AC%B8%EC%A0%9C%EC%A0%90

// 코드 1-1
var arr = [];
for (var i = 0; i < 5; i++) {
  arr[i] = function () {
    return i;
  };
}

// 코드 1-2
for (var func of arr) {
  console.log(func());
}

          ↓

// arr 배열에 함수가 담긴 모습 시각화
arr[0] = function () {
  return i; // 전역변수 i를 참조
};
arr[1] = function () {
  return i;
};

...

arr[4] = function () {
  return i;
};

          ↓

// console 출력
5
5
5
5
5
```

코드 1-1에서 이미 전역변수 i가 5까지 증가되었고, 배열에 할당된 함수들은 여전히 전역 변수 i를 참조하기 때문에 문제가 발생합니다.

##### 해결 방안 예시 1

```javascript{ .numberLines}
// 출처: https://velog.io/@draw/for%EB%AC%B8%EC%97%90%EC%84%9C-var%EB%A5%BC-%EC%82%AC%EC%9A%A9%ED%95%98%EB%A9%B4-%EC%83%9D%EA%B8%B0%EB%8A%94-%EB%AC%B8%EC%A0%9C%EC%A0%90

for (var i = 0; i < 5; i++) {
  arr[i] = function (index) {
    return function () {
      return index;
    };
  }(i);
}
```

클로저(closure)를 사용하여 전달인자(argument)로 i 값을 주면 배열에 각 함수가 할당될 때 그 때의 i 값을 매개변수로 받기 때문에 지역변수를 사용한 것 같은 효과가 납니다.

##### 해결 방안 예시 2: let을 씁니다.

```javascript{ .numberLines}
// 출처: https://velog.io/@draw/for%EB%AC%B8%EC%97%90%EC%84%9C-var%EB%A5%BC-%EC%82%AC%EC%9A%A9%ED%95%98%EB%A9%B4-%EC%83%9D%EA%B8%B0%EB%8A%94-%EB%AC%B8%EC%A0%9C%EC%A0%90


// 블록 스코프인 let 사용
for (let i = 0; i < 5; i++) {
    arr[i] = function () {
     return i;
    };
}
```

#### 2. 클로저는 그 본질이 메모리를 꼐속 차지하는 개념이므로 더는 사용하지 않게 된 클로제에 대해서는 메모리를 차지하지 않도록 관리해줄 필요가 있습니다.

##### 해결 방안 예시 1. return에 의한 클로저의 메모리 해제

```javascript{ .numberLines}
// 출처: 코어 자바스크립트

var outer = (funtion(){
    var a = 1;
    var inner = function(){
        return a++;
    }

    return inner;
})();

console.log(outer());
console.log(outer());

// outer 식별자의 inner함수 참조를 끊음
outer = null;

```

##### 해결 방안 예시 2. setInterval에 의한 클로저의 메모리의 해제

```javascript{ .numberLines}
// 출처: 코어 자바스크립트

(funtion(){
    var a = 0;
    var intervalId = null;
    var inner = function (){
        if(++a>=10){
            clearInterval(intervalId);
            // inner 식별자의 함수 참조를 끊음
            inner = null;
        }
        console.log(a);
    };
    intervalId = setInterval(inner, 1000);
})();
```

##### 해결 방안 예시 3. eventListener에 의한 클로저의 메모리의 해제

```javascript{ .numberLines}
// 출처: 코어 자바스크립트

(function(){
    var count = 0;
    var button = document.createElement("button");
    button.innerText = "click";

    var clickHandler = funtion(){
        console.log(++count, "times clicked");

        if(count>=10){
            buttn.removeEventListener("click", clickHandler);
            // clickHandler 식별자의 함수 참조를 끊음
            clickHandler = null;
        }
    };

    button.addEventListener("click", "clickHandler");
    document.body.appendChild(button);

})();
```

## 요약

클로저는 주변의 상태 (lexical environment)의 참조와 함께 번들로 묶인 함수의 조합입니다. 간단히 말하자면 함수가 선언될 때(실행X) 외부의 lexcial environment를 참조하게 되는 현상입니다.

클로저란 어떤 함수 A에서 선언한 변수 a를 참조하는 내부함수 B를 외부로 전달할 경우 A의 실행 컨텍스트가 종료된 이후에도 변수 a가 사라지지 않는 현상을 말합니다.

## 참고

- [MDN](https://developer.mozilla.org/ko/docs/Web/JavaScript/Closures)
- [코어자바스크립트](http://www.yes24.com/Product/Goods/78586788)
- [blog(클로져와 클로져의 사용 예제)](https://velog.io/@proshy/JS%ED%81%B4%EB%A1%9C%EC%A0%B8closure%EC%99%80-%ED%81%B4%EB%A1%9C%EC%A0%B8%EC%9D%98-%EC%82%AC%EC%9A%A9-%EC%98%88%EC%A0%9C)
- [배민 테코톡](https://www.youtube.com/watch?v=PVYjfrgZhtU&list=LL&index=12)
- [velog(for문에 var를 쓸 때 생기는 문제점)](https://velog.io/@draw/for%EB%AC%B8%EC%97%90%EC%84%9C-var%EB%A5%BC-%EC%82%AC%EC%9A%A9%ED%95%98%EB%A9%B4-%EC%83%9D%EA%B8%B0%EB%8A%94-%EB%AC%B8%EC%A0%9C%EC%A0%90)
