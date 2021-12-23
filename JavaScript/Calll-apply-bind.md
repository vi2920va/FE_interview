# JavaScript - call, apply, bind

## 설명

call, apply, bind는 모두 this를 원하는 객체로 바꿔주는 방식입니다.

### 1. function.call()

- call() 메소드는 주어진 this 값 및 각각 전달된 인수와 함께 함수를 호출합니다.
- 다른 오브젝트에 있는 function을 빌리고 싶을 때 쓴다고 생각하면 됩니다.

#### 예시 1)

![image](https://user-images.githubusercontent.com/47317129/145251960-34ef5ea4-a8a4-4acf-a095-e61043cb4ee2.png)

예시 1에서 name2는 printFullName이라는 함수를 가지고 있지 않지만, name의 printFullName 함수를 call하여 사용하고 있습니다.

#### 예시 2)

![image](https://user-images.githubusercontent.com/47317129/145252475-4ae54e20-6b47-415c-9714-4fe7e55c27a2.png)

객체 외부에서 선언한 함수일 경우 예시 2와 같이 call을 사용할 수 있습니다.

#### 예시 3)

![image](https://user-images.githubusercontent.com/47317129/145252721-d24a2bce-d323-4ee6-a7fa-ff1379d673c1.png)

예시 3과 같이 추가적으로 여러 파라메터를 전달할 수 있습니다.

### 2. function.apply()

- 주어진 this 값과 배열 (또는 유사 배열 객체) 로 제공되는 arguments 로 함수를 호출합니다.
- call과 동일한데 파라메터로 배열 하나를 받는다는 점만 다릅니다(call은 여러 파라메터들을 받습니다).

#### 예시 1)

![image](https://user-images.githubusercontent.com/47317129/145253114-47e6e5c8-e933-4ba3-b5e5-4ee4a99730dc.png)

예시 1과 같이 apply는 call과 달리 배열 하나만을 파라메터로 받습니다.

### 3. function.bind()

- 새로운 함수를 생성합니다. 받게되는 첫 인자의 value로는 this 키워드를 설정하고, 이어지는 인자들은 바인드된 함수의 인수에 제공됩니다.
- bind는 call이랑 동일한데, 즉시 실행되지 않고 함수를 카피해둔 상태가 된다는 점이 다릅니다.

#### 예시 1)

![image](https://user-images.githubusercontent.com/47317129/145252876-e847e670-da7f-487b-aad3-3dc7cf8b7f65.png)

call과 apply와 다르게 bind는 즉시 실행되지 않으므로 실행을 원할 경우 bind를 통해 만든 함수를 직접 실행시켜줘야 합니다.

## 요약

- call, apply, bind는 모두 this를 원하는 객체로 바꿔줍니다.
- 셋의 차이로는 우선 크게 call, apply는 호출 시 함수를 바로 실행하지만,
- bind는 함수를 카피만 해둘뿐 실행하지 않는다는 점이 다릅니다.
- 그리고 call은 여러 인자들을 받지만 apply는 배열 하나만을 인자로 받는 다는 점이 다릅니다(bind는 call과 같이 여러 인자를 받습니다).

## 참고

- [youtube](https://www.youtube.com/watch?v=75W8UPQ5l7k&t=32s)
- [MDN(call)](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Function/call)
- [MDN(apply)](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Function/apply)
- [MDN(bind)](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Function/bind)
