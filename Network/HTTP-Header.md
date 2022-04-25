# Network - HTTP Header

## 설명

### 1. 일반 헤더

#### 1.1 HTTP 헤더 개요

HTTP 헤더는 클라이언트와 서버가 요청 또는 응답으로 부가적인 정보를 전송할 수 있도록 해준다.
e.g.) 메시지 바디의 내용, 메시지 바디의 크기, 압축, 인증, 요청 클라이언트, 서버 정보, 캐시 관리 정보 등

##### 헤더 분류

    - General: 요청과 응답 모두에 적용되지만 바디에서 최종적으로 전송되는 데이터와는 관련이 없는 헤더 e.g.) Connection: close
    - Request: 패치될 리소스나 클라이언트 자체에 대한 자세한 정보를 포함하는 헤더 (내가 보내는 메시지 헤더) e.g.) User-Agent: Mozilla/5.0 (Macintosh; ..)
    - Response: 위치 또는 서버 자체에 대한 정보(이름, 버전 등)와 같이 응답에 대한 부가적인 정보를 갖는 헤더 (내가 받는 메시지 헤더) e.g.) Server: Apache
    - Entity: 컨텐츠 길이나 MIME 타입과 같이 엔티티 바디에 대한 자세한 정보를 포함하는 헤더 e.g.) Content-Type: text/html, Content-Length: 3423

#### 1.2 표현

서버가 보낼 리소스가 어떤 식으로 표현되어있는지를 나타낸다.
e.g.) HTML로 되어있는지, JSON으로 되어있는지 등

표현 헤더는 전송, 응답 둘다에서 사용한다.

##### 종류

- **Content-Type**: 표현 데이터의 형식

  - e.g.) text/html; charset=utf-8, application/json, image/png

- **Content-Encoding**: 표현 데이터의 압축 방식

  - 표현 데이터를 압축하기 위해 사용
  - 데이터를 전달하는 곳에서 압축 후 인코딩 헤더 추가
  - 데이터를 읽는 쪽에서 인코딩 헤더의 정보로 압축 해제
  - e.g.) gzip, deflate, dentity

- **Content-Language**: 표현 데이터의 자연 언어

  - 표현 데이터의 자연 언어를 표현
  - e.g.) ko, en, en-US

- **Content-Length**: 표현 데이터의 길이

  - 바이트 단위
  - Transfer-Encoding(전송 코딩)을 사용하면 Content-Length를 사용하면 안 된다.

#### 1.3 콘텐츠 협상

클라이언트가 **선호하는 표현을 요청**하는 것으로 Get 요청에서만 사용한다.
예컨대 한국어로된 사이트를 원하지만, 만일 한국어 사이트가 없다면 영어 사이트를 요청하는 경우가 이에 해당한다.

##### 종류

- **Accept**: 클라이언트가 선호하는 미디어 **타입** 전달
- **Accept-Charset**: 클라이언트가 선호하는 **문자** 인코딩
- **Accept-Encoding**: 클라이언트가 선호하는 **압축** 인코딩
- **Accept-Language**: 클라이언트가 선호하는 **자연 언어**

클라이언트가 선호하는 우선순위 지정 또한 할 수 있다.
우선순위는 다음과 같은 방식으로 동작한다.

1. Qunatity Values(q) 값이 높은 조건을 우선 시한다.
   Qunatity Values(q)는 0부터 1까지의 값이다. 생략 시 1이 된다.
   e.g.) Accept-Language: ko-KR,ko;q=0.9,en-US;q=0.8
   우선순위: ko-KR>ko>en-US

2. 구체적인 조건이 우선한다.
   e.g) `Accept: text/*,text/plain,text/plain;format=flowed,*/*`
   우선순위: `text/plain;format=flowed`>`text/plain`>`text/*`>`*/*`

3. 구체적인 조건에 맞추어 미디어 타입을 맞춘다.
   e.g) `Accept: text/*;q=0.3,text/html;q=0.7,text/html;level=1,text/html;level=2;q=0.4,*/*;q=0/5`
   여기서 text/plain의 경우 위에 명시 된 내용에 매칭되는 내역이 없기 때문에 `text/*;q=0.3`에 포함되어 0.3의 우선순위를 가지게 된다.

컨텐츠 협상 헤더에 따른 리소스 전송은 일반적으로 많이 사용되는 서버 프레임워크에서 기본적으로 구현이 되어있다.

#### 1.4 전송 방식

어떤 <b>길이 (length)</b>로 컨텐츠를 보낼 것인지를 나타낸다.

##### 종류

1. 단순 전송
   : 컨텐츠를 한 번에 요청하고 한 번에 받는 단순한 전송, 보내는 컨텐츠 길이를 서버에서 알고 있다. (e.g. Content-Length: 3243)
2. 압축 전송
   : 컨텐츠를 압축해서 전송하는 경우, 이 경우에는 어떤 방식으로 압축했는지를 표기해줘야 한다. (e.g. Content-Encoding: gzip)
3. 분할 전송
   : 컨텐츠의 용량이 커서 분할하여 부분적으로 클라이언트에서 바로 보여줄 수 있도록 하는 전송,
   이때는 컨텐츠의 길이를 예측할 수 없으므로 컨텐츠 길이 정보를 헤더에 넣으면 안 된다. (e.g. Transfer-Encoding: chunked)
4. 범위 전송
   : 컨텐츠 전체를 요청하는 것이 아닌, 범위로 요청하여 받는 경우의 전송 (e.g. Content-Range: 1001-2000/2000)

#### 1.5 일반 정보

정보성 헤더들에 대한 내용들이다.

##### 종류

1. From: 유저 에이전트의 이메일 정보
   크롤링을 하지 말라고 메일을 보내는 경우 활용된다. 그 외에는 잘 사용되지 않는다. 요청에서 사용된다.
2. Referer: 이전 웹 페이지 정보
   유저 유입 정보를 알아내기 위해 매우 자주 사용된다.
   정확한 스펠링은 Referrer인데 실수로 오타가 난 상태로 만들어졌고 계속 그대로 사용하고 있다. 요청에서 사용된다.
3. User-Agent: 유저 에이전트 어플케이션 정보
   어떤 브라우저에서 에러가 생기는지 서버에서 디버깅 시 유용하게 사용되는 정보이다. 요청에서 사용된다.
4. Server: 요청을 처리하는 오리진 서버의 소프트웨어 정보
   오리진 서버란, 중간에 거치는 프록시 서버가 아닌 나의 요청을 직접적으로 처리하는 서버를 의미한다. 응답에서 사용된다.
5. Date: 메시지가 생성된 날짜, 응답에서만 사용된다.

#### 1.6 특별한 정보

1. Host: 요청한 호스트 정보 (도메인)
   - 요청에서 사용
   - **필수 값이다.**
   - 하나의 서버가 여러 도메인을 처리해야 할 때
   - 하나의 IP 주소에 여러 도메인이 적용되어 있을 때
2. Location: 페이지 리다이렉션, 어디로 리다이렉션할지를 나타냄
3. Allow: 허용가능한 HTTP 메서드
4. Retry-After: 유저 에이전트가 다음 요청을 하기까지 기다려야 하는 시간

#### 1.7 인증

1. Authorization: 클라이언트 인증 정보를 서버에 전달
2. WWW-Authenticate: 리소스 접근 시 필요한 인증 방법 정의
   - 리소스 접근 시 필요한 인증 방법 정의
   - 401 Unauthorized 응답과 함께 사용

참고로 OAuth 인증에 들어가는 헤더값은 다르니 각 인증방법 마다 확인을 해야한다.

#### 1.8 쿠키

1. Set-Cookie: 서버에서 클라이언트로 쿠키 전달 (응답)
2. Cookie: 클라이언트가 서버에서 받은 쿠키를 저장하고, HTTP 요청 시 서버로 전달

- **생명주기**를 지정하여 만료일을 지정할 수 있다.

  - expires: 만료일이 되면 쿠키 삭제
  - max-age: 일정 시간 후 쿠키를 삭제하게 할 수 있다. 0이나 음수를 지정하면 쿠키 삭제 (단위: 초)
  - 세션 쿠키: 만료날짜를 생략하면 브라우저 종료 시까지만 유지
  - 영속 쿠키: 만료날짜를 입력하면 해당날짜까지만 유지

- 도메인
  - 지정한 도메인의 하위 도메인에서도 사용할 수 있게 해준다.
- 경로
  - 지정한 경로 하위 경로에서도 사용할 수 있게 해준다.
- 보안
  - Secure
    - 쿠키는 http, https를 구분하지 않고 전송
    - Secure를 적용하면 https인 경우에만 전송
  - HttpOnly
    - XSS 공격\* 방지
    - 자바스크립트에서 접근 불가 (document.cookie)
    - HTTP 전송에만 허용
  - SameSite
    - XSRF(CSRF) 공격\* 방지
    - 요청 도메인과 쿠키에 설정된 도메인이 같은 경우만 쿠키 전송

\*크로스 사이트 스크립팅(Cross Site Scripting, XSS)은 공격자가 상대방의 브라우저에 스크립트가 실행되도록 해 사용자의 세션을 가로채거나, 웹사이트를 변조하거나, 악의적 콘텐츠를 삽입하거나, 피싱 공격을 진행하는 것을 말한다.
\*CSRF(Cross-Site Request Forgery)는 사용자가 자신의 의지와는 무관하게 공격자가 의도한 행위(수정, 삭제, 등록 등)를 특정 웹사이트에 요청하게 하는 공격을 말한다. 사용자가 의도하지 않았지만, 자신도 모르게 서버를 공격하게 되는 경우이다.

### 2. 캐시와 조건부 요청

#### 2.1 캐시 기본 동작

- 캐시 덕분에 캐시 가능 시간동안 네트워크를 사용하지 않아도 된다.

- 비싼 네트워크 사용량을 줄일 수 있다.

- 브라우저 로딩 속도가 매우 빠르다.

- 빠른 사용자 경험을 만들 수 있다.

#### 2.2 검증 헤더와 조건부 요청 1

- 캐시 유효 시간이 초과해도, 서버의 데이터가 갱신되지 않으면 304 Not Modified + 헤더 메타 정보(Last-Modified 포함)만 응답하고 바디는 보내지 않는다.

- 데이터가 갱신된 부분이 없는지는 클라이언트에서 보내는 If-Modified-Since와 서버의 Last-Modified를 비교하여 판단한다.
- 클라이언트는 서버가 보낸 응답 헤더 정보로 캐시의 메타 정보를 갱신

- 클라이언트는 캐시에 저장되어 있는 데이터 재활용

- 결과적으로 네트워크 다운로드가 발생하지만 용량이 적은 헤더 정보만 다운로드

- 매우 실용적인 해결책

<br />

- If-Modified-Since와Last-Modified 사용의 단점
  - 1초 미만 단위로 캐시 조정이 불가능
  - 날짜 기반의 정해진 로직 사용
  - 데이터를 수정해서 날짜가 다르지만, 수정 전과 후의 데이터가 똑같은 경우를 구분할 수 없음
  - 서버에서 변도의 캐시 로직을 관리하고 싶은 경우 사용하기 적합하지 않음
    - e.g.) 스페이스나 주석처럼 크게 영향이 없는 경우 캐시를 유지하고 싶은 경우
      -> 이 때는 ETag를 사용하면 된다.

#### 2.3 검증 헤더와 조건부 요청 2

- 검증 헤더

  - 캐시 데이터와 서버 데이터가 같은지 검증하는 데이터
  - Last-Modified , ETag(Entity Tag)\*
    \*ETag HTTP 응답 헤더: 캐시용 데이터에 임의의 고유한 버전 이름을 달아두는 것,
    데이터가 변경되면 이 이름을 바꾸어서 변경함 (Hash를 다시 생성)

    **서버에 ETag를 If-None-Match 헤더의 값으로 보내서 같으면 유지하고, 다르면 새로 받는 로직으로 캐시 컨트롤을 할 수 있다.**

    특정 버전의 리소스를 식별하는 식별자 ([참고](https://developer.mozilla.org/ko/docs/Web/HTTP/Headers/ETag))

<br />

- 조건부 요청 헤더

  - 검증 헤더로 조건에 따른 분기
  - If-Modified-Since: Last-Modified 사용
  - If-None-Match: ETag 사용
  - 조건이 만족하면 200 OK
  - 조건이 만족하지 않으면 304 Not Modified

#### 2.4 캐시와 조건부 요청 헤더

##### 캐시

캐시는 캐시 제어와 관련한 헤더들이 있다.

##### 종류

1. Cache-Control
   : 캐시 제어
   가장 중요하다. 아래 2, 3번의 기능도 Cache-Control로 할 수 있다.

   - max-age: 캐시 유효 시간, **초 단위**
   - no-cache: 데이터는 캐시해도 되지만, 항상 원(오리지널) 서버에 검증하고 사용해야 한다.
   - no-store: 데이터에 민감한 정보가 있으므로 저장하면 안 된다. (메모리에서 사용하고 최대한 빨리 삭제해야 한다.)

2. Pragma
   : 캐시 제어 (하위 호환)
3. Expires
   : 캐시 유효 기간 (하위 호환), 구체적인 **만료일** 지정
   Expires보다 Cache-Control의 max-age로 지정하는 것이 더 유연한 방법이고 권장되는 방법이다.
   따라서 Cache-Control의 max-age와 함께 쓰일 경우 무시된다.

##### 검증 헤더와 조건부 요청 헤더

- 검증 헤더 (Validator)

  - ETag: "v1.0", ETag: "asid93jkrh2l"
  - Last-Modified: Thu, 04 Jun 2020 07:19:24 GMT

- 조건부 요청 헤더

  - If-Match, If-None-Match: ETag 값 사용
  - If-Modified-Since, If-Unmodified-Since: Last-Modified 값 사용

#### 2.5 프록시 캐시

프록시 캐시 서버(CDN server)와 통신할 때 쓰는 헤더

- Cache-Control: public

  - 응답이 public 캐시 (프록시 캐시 서버)에 저장되어도 된다는 뜻

- Cache-Control: private

  - 응답이 해당 사용자만을 위한 것이고, private 캐시에 저장해야 한다는 뜻 (기본값)
  - e.g.) 유저의 로그인 정보

- Cache-Control: s-maxage

  - 프록시 캐시에만 적용되는 max-age

- Age: 60 (HTTP 헤더)

  - 오리진 서버에서 응답 후 프록시 캐시 내에 머문 시간(초)

#### 2.6 캐시 무효화

캐시를 적용하지 않았음에도 브라우저가 디폴트로 GET 요청의 경우 캐싱을 진행하는 경우가 있다.
만일 절대 캐싱되면 안 되는 페이지가 있다면 아래 헤더를 (Cache-Control, Pragma) 넣어 캐시를 확실하게 무효화시켜야 한다.

e.g.) 유저의 통장 잔고; 매번 바뀌는 정보이기 때문에 캐싱을 할 필요가 없다.

##### Cache-Control

- Cache-Control: no-cache

  - 데이터는 캐시해도 되지만, 항상 원 서버에 검증하고 사용(이름에 주의!)

- Cache-Control: no-store

  - 데이터에 민감한 정보가 있으므로 저장하면 안 됨
    (메모리에서 사용하고 최대한 빨리 삭제)

- Cache-Control: must-revalidate

  - 캐시 만료후 최초 조회시 원 서버에 검증해야함
  - 원 서버 접근 실패시 반드시 오류가 발생해야함 - 504(Gateway Timeout)
  - must-revalidate는 캐시 유효 시간이라면 캐시를 사용함

- no-cache와 must-revalidate의 차이

  : no-cache의 경우 원서버에 요청을 날리지 못하게 될 경우 기존의 캐싱된 데이터를 보내줄 수도 있다.
  하지만 must-revalidate의 경우 원서버에 네트워크 요청을 날리지 못하게 될 경우 무조건 504에러를 리턴한다.

##### Pragma

- Pragma: no-cache

  - HTTP 1.0 하위 호환

## 참고

- [모든 개발자를 위한 HTTP 웹 기본 지식](https://www.inflearn.com/course/http-%EC%9B%B9-%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC/dashboard)
- [MDN](https://developer.mozilla.org/ko/docs/Web/HTTP/Headers)
- [크로스 사이트 스크립팅의 정의 및 공격 유형](https://nordvpn.com/ko/blog/xss-attack/)
- [CSRF(Cross-Site Request Forgery) 공격과 방어](https://junhyunny.github.io/information/security/spring-boot/spring-security/cross-site-reqeust-forgery/)
