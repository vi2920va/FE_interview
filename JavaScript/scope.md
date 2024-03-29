# JavaScript - Scope

## 설명

모든 식별자(변수 이름, 함수 이름, 클래스 이름 등)는 자신이 선언된 위치에 의해 다른 코드가 식별자 자신을 참조할 수 있는 유효 범위가 결정된다. 이를 스코프라 합니다.  

### 스코프 체인  

- 스코프가 함수의 중첩에 의해 계층적 구조를 갖는다는 것입니다.  
- 변수를 참조할 때 자바스크립트 엔진은 스코프 체인을 통해 변수를 참조하는 코드의 스코프에서 시작하여 상위 - 스코프 방향으로 이동하며 선언된 변수를 검색합니다. 절대 하위 스코프로 내려가면서 식별자를 검색하는 일은 없습니다.  
- 이는 상위 스코프에서 유효한 변수는 하위 스코프에서 자유롭게 참조할 수 있지만 하위 스코프에서 유효한 변수를 상위 스코프에서 참조할 수 없다는 것입니다.  

### 함수 레벨 스코프

- 블록 레벨 스코프는 함수 몸체(function) 뿐만 아니라 모든 코드 블록(if, for, while, try/catch 등)이 지역 스코프를 만듭니다.  
- var 키워드로 선언된 변수는 오로지 함수의 코드 블록(함수 몸체)만을 지역 스코프로 인정합니다.  
- let, const 키워드는 블록 레벨 스코프를 지원합니다.  

### 동적 스코프 vs 정적(렉시컬) 스코프

- 동적 스코프는 함수를 어디서 호출했는지에 따라 상위 스코프를 결정합니다.  
- 정적 스코프는 함수를 어디서 정의했는지에 따라 상위 스코프를 결정합니다. 자바스크립트는 렉시컬 스코프를 따른다.
- 즉 함수의 상위 스코프는 언제나 자신이 정의된 스코프를 따릅니다.

## 요약

### 스코프란 ?

- 프로그래밍 언어에서는 스코프(유효 범위)를 통해 식별자인 변수 이름의 충돌을 방지하여 같은 이름의 변수를 사용할 수 있게 합니다.

- 자바스크립트는 렉시컬 스코프를 따르기 때문에 함수 정의가 실행되어 생성된 함수 객체는 결정된 상위 스코프를 기억합니다.
