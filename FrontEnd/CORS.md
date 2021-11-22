# Frontend - CORS

## 설명

### 1. 동일 출처 정책 (Same-Origin Policy)

하나의 출처에서 다른 출처의 문서나 스크립트에서 가져온 리소스와 상호작용하는 것을 제한하는 정책. 기본적으로 동일한 출처의 리소스와 상호작용만 하도록 허용한다.

#### 출처(Origin)

어떤 웹 컨텐츠에 접근하려고 할 때 사용하는 **URL의 스킴(protocol), 호스트명(domain), 포트번호를 출처(Origin)**라고 한다. 

아래의 두 개의 URL은 **동일한 출처**를 가진다고 할 수 있다. 리소스의 경로는 다르지만 **동일한 프로토콜(http), 도메인(example.com), 포트번호(기본 80포트)**를 가졌기 때문이다. (단,  Internet Explorer는 프로토콜과 포트번호를 별도로 검사하지 않는다.)

- http://example.com/app1/index.html
- http://example.com/app2/index.html

#### 동일 출처 정책의 한계

요즘에는 Open API 등 다른 출처의 URL에 자원을 요청하는 경우가 굉장히 많아졌다. 동일 출처 정책은 웹을 충분히 활용하지 못하도록하는 방해꾼이 되었다. 서로 다른 출처끼리도 상호작용을 할 수 있도록 하기 위해 CORS가 등장했다. 

### 2. CORS (Cross-Origin Resource Sharing)
#### 소제목 

## 요약
- 전역 변수로 var 키워드가 사용되며 재선언 재할당이 가능하다
- let은 중복적으로 선언이 불가능하다.

## 참고
- [MDN 동일 출처 정책](https://developer.mozilla.org/ko/docs/Web/Security/Same-origin_policy)

