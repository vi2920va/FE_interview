# Frontend - CORS

## 설명

### 1. 동일 출처 정책 (Same-Origin Policy)

하나의 출처에서 다른 출처의 문서나 스크립트에서 가져온 리소스와 상호작용하는 것을 제한하는 정책. 기본적으로 동일한 출처의 리소스와 상호작용만 하도록 허용합니다.

#### 1-1. 출처(Origin)

어떤 웹 컨텐츠에 접근하려고 할 때 사용하는 **URL의 스킴(protocol), 호스트명(domain), 포트번호를 출처(Origin)** 라고 합니다.

아래의 두 개의 URL은 **동일한 출처**를 가진다고 할 수 있다. 리소스의 경로는 다르지만 **동일한 프로토콜(http), 도메인(example.com), 포트번호(기본 80포트)**를 가졌기 때문이다. (단,  Internet Explorer는 프로토콜과 포트번호를 별도로 검사하지 않습니다.)

- http://example.com/app1/index.html
- http://example.com/app2/index.html

#### 1-2. 동일 출처 정책의 한계

요즘에는 Open API 등 다른 출처의 URL에 자원을 요청하는 경우가 굉장히 많아졌다. 동일 출처 정책은 웹을 충분히 활용하지 못하도록하는 방해꾼이 되었다. 그래서 서로 다른 출처끼리 상호작용 할 때는 CORS의 관리하에 가능해졌습니다.

#### 1-3. 교차 출처 네트워크 접근

| 상호착용 종류  | 허용 여부 | 예시                                                         |
| :--------------: | :---------: | :------------------------------------------------------------ |
| 교차 출처 쓰기 | ✅         | link, redirect, 양식 제출 등                                 |
| 교차 출처 삽입 | ✅         | script 태그 내의 src, link 태그 내의 href, img 또는 video 태그, @font-fac, iframe ... |
| 교차 출처 읽기 | ❎         |                                                              |

### 2. CORS (Cross-Origin Resource Sharing)

브라우저는 보안상의 이유로 교차출처 HTTP  요청을 제한한다. 브라우저의 Web API인 XMLHttpRequest와 Fetch API 역시 동일 출처 정책을 따른다. 그래서 다른 출처에서 리소스를 가져오기 위해서는 그 출처에서 CORS 헤더를 포함한 응답을 반환해야 합니다.

#### 2-1. Preflight 요청

OPTIONS 메서드를 통해 도메인의 리소스로 HTTP요청을 보내 **실제 요청을 안전하게 요청할 수 있는지 확인**한다. 이 과정을 거치는 이유는 **아무 클라이언트나 서버에 접근해 유저 데이터에 영향을 미치는 것을 방지하기 위함**이다. 그래서 Preflight 과정을 통해 서버에서 허용하는 출처들의 목록이나 허용 가능한 메소드 등을 확인하는 과정을 거칩니다.

요청 헤더에 Origin과 Access-Control-Request-Method를 포함해 어떤 출처에서 보낸 것이며, 어떤 메소드를 사용할지 서버에 알려준다. Access-Control-Request-* 헤더는 Preflight 요청에서만 보내고 실제 요청을 보낼 때는 포함시키지 않습니다.

서버는 응답 헤더에 가능한 출처의 목록(*는 모든 도메인에서 접근가능함을 의미)과 요청 가능한 메소드들의 종류를 보내줍니다.

#### 2-2. 단순 요청

단순 요청은 CORS preflight를 발생시키지 않는다. 단순 요청의 충족 요건은 아래와 같습니다.

- GET, HEAD, POST요청만 허용하는 API 리소스에 대해 발행된 요청합니다.
- POST 메서드의 경우 Origin 헤더를 포함해야합니다.
- 요청에 사용자 지정 헤더가 없습니다.
- 요청 페이로드 컨텐츠 유형이 `text/plain`, `multipart/form-data` 또는 `application/x-www-form-urlencoded`
- 요청에 사용자 지정 헤더가 없습니다.

단, 단순 교차 출처 요청으로 POST 메서드를 요청한다면, 응답에는 'Access-Control-Allow-Origin' 헤더가 포함되어야 합니다. 위 조건 외 모든 요청들은 비단순 요청으로 취급한다. (가령 POST요청이지만 비표준 HTTP헤더를 포함하는 경우에는 preflight가 실행됩니다.)

### 3. CORS 해결방법

#### 3-1. 서버에서 CORS를 허용해주기

서버를 직접 제어할 수 있다면, 서버에서 `Access-Control-Allow-Origin` 헤더에 클라이언트 출처를 허용해주면 됩니다. 가장 정석인 방법이지만, Open API를 사용할 때처럼 서버를 직접 제어할 수 없는 경우에는 사용할 수 없습니다.

```javascript
const express = require('express');
const app = express();

app.get('/allow-cors', function(request, response) {
 response.set('Access-Control-Allow-Origin', '허용 도메인');
 response.sendFile(__dirname + '/message.json');
});

const listener = app.listen(process.env.PORT, function() {
  console.log('Your app is listening on port ' + listener.address().port);
});
```

위의 방법은 가장 정석이지만, 서버에서 응답을 보낼 때마다 `Access-Control-Allow-Origin` 헤더를 추가해줘야하는 번거로움이 있다. Express를 이용해 서버를 구축한 경우 Node.js 미들웨어인 CORS를 설치하면 매번 CORS관련 설정을 직접해줄 필요가 없습니다.

```javascript
const express = require('express');
const cors = require('cors');
const app = express();

app.use(cors({ origin: '허용 도메인' }));
```

#### 3-2. 프록시 서버 사용하기

서버를 직접 제어할 수 없는 상황이라면 프록시 서버를 사용하는 방법이 있다. CORS 정책은 브라우저에서 강제하는 방법이다. 서버에서 브라우저에게 몇몇 조건이 걸린 헤더들을 내려주면 브라우저에서는 SOP를 지키며 작업을 처리하지만 프록시 서버는 그렇지 않다. 서버 간 통신에서는 이 정책이 적용되지 않기 때문이다.

또한 프록시 서버는 클라이언트에게 응답을 보낼 때 `Access-Control-Allow-Origin`의 출처로 요청 도메인의 출처를 포함시켜주면 되기 때문에 CORS 문제를 해결 할 수 있습니다.

#### 3-3. https-proxy-middleware 라이브러리 설치

개발 중에 로컬 환경에서 클라이언트와 서버의 출처의 포트번호가 달라 CORS 문제가 발생할 때 사용하는 방법입니다. CRA환경에서 개발중일 때도 사용할 수 있습니다.

- [https-proxy-middleware 모듈](https://github.com/chimurai/http-proxy-middleware#readme)
- [CRA에서 https-proxy-middleware 설정방법](https://create-react-app.dev/docs/proxying-api-requests-in-development/)

## 요약

- SOP는 동일한 출처에서만 리소스를 상호작용하도록 제한하는 정책입니다.
- 현실적으로 SOP를 지키기는 어렵기 때문에, 브라우저에서는 CORS 정책을 통해 출처가 다른 도메인 간의 리소스 상호작용을 제한적으로 허용합니다.
- 브라우저는 Preflight 요청을 통해서 서버에 요청을 보내도 되는지 확인 요청을 보낸다. 이때 클라이언트의 출처를 밝히는 `Origin` 헤더를 포함해 요청을 보내고, 서버에서는 요청이 가능한 출처를 포함한 `Access-Control-Allow-Origin` 헤더를 포함해 응답을 보내줍니다.
- CORS 이슈는 서버에서 클라이언트를 허용 출처로 포함해주는 것이 가장 최선이지만, 서버를 제어할 수 없는 환경에서는 프록시 서버를 이용해 해결할 수 있습니다.

## 참고

- [MDN - 동일 출처 정책](https://developer.mozilla.org/ko/docs/Web/Security/Same-origin_policy)
- [MDN - CORS](https://developer.mozilla.org/ko/docs/Web/HTTP/CORS)
- [AWS - REST API 리소스에 대한 CORS 활성화](https://docs.aws.amazon.com/ko_kr/apigateway/latest/developerguide/how-to-cors.html)
- [web.dev - CORS](https://web.dev/cross-origin-resource-sharing/)
- [stackexchange - CORS Proxy가 작동하는 방법](https://security.stackexchange.com/questions/191737/how-do-cors-proxy-websites-work)
- [CORS 해결방법 내용 참고 블로그](https://xiubindev.tistory.com/115)
- [CORS는 왜 이렇게 우리를 힘들게 하는걸까?](https://evan-moon.github.io/2020/05/21/about-cors/)
