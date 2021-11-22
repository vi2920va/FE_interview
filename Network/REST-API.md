# NetWork - REST API

## 설명

### 1.응용 프로그래밍 인터페이스(Application Programming Interface)

API는 소프트웨어 다른 소프트웨어로 부터 지정된 형식으로 명령, 요청을 받을 수 있는 수단입니다.

### 2.REST(Representational State Transfer)

REST는 웹에 존재하는 모든 자원에 고유한 URL를 부여하여 자원에 대한 주소를 지정하는 방법론, 또는 규칙입니다.

### 3. REST 구성과 특징

3-1. REST 구성은 아래와 같이 이루어집니다.

- 자원(Resource): HTTP URI
- 자원에 대한 행위(Verb): HTTP Method
- 자원에 대한 행위의 내용(Representations): HTTP Message Pay Load

3-2. REST 특징

- 서버 클라인트 구조(Server Client)
: 사용자 인증, 로그인 등을 직접 관리하는 구조로 각각 역할의 구분되기 때문에 서버와의 개발 내용이 명확해지고 의존성이 줄어듭니다.

- 무상태(Stateless)
: 세션, 쿠키 등의 정보를 별도로 저장하지 관리하지 않으므로 서비스의 자유도가 높아지고, 불필요한 정보를 관리하지 않으므로 구현이 단순화해진다.

- 캐시 처리 기능(Cacheable)
: 웹 표준 HTTP 프로토콜이 가진 강력한 특징 중 하나인 캐싱 기능을 적용할 수 있습니다.

- 인터페이스 일관성(Uniform Interface)
: URL로 지정한 소스에 대한 조작을 통일되고, 한정적인 인터페이스로 수행합니다.

- 계층 구조(Layered System)
: REST는 다중 계층으로 구성될 수 있으 보안, 암호화 계층을 추가해 구조상의 유연성을 둘 수 있습니다.

### 4. REST 설계 규칙

- URL은 정보의 자원을 표현해야 합니다.
- 자원에 대한 행위는 HTTMP 메서드로 표현됩니다.(GET, POST, PUT, DELETE)
- 슬래시 기호(/)는 계층 관계를 나타나는데 사용됩니다.
- URL에 마지막 슬래시(/)를 사용하면 안됩니다.
- 하이픈(-)기호는 가독성을 높이는데 사용됩니다.
- 기타 여러 사항은 아래의 링크를 참고하여 이용하시면 됩니다.

## 요약

- RESTful은 REST API를 제공하는 웹 서비스를 **RESTful** 하다고 말할 수 있습니다.
- 즉, REST 원리를 따르는 시스템을 RESTful 이라는 용어로 지칭됩니다.
- RESTful은 이해하기 쉽고, 사용하기 쉬운 RESR API를 만드는 것을 목적으로 합니다.
- REST 6가지 규칙을 잘 지켜서 설계된 API를 RESTful API라고 합니다.

## 참고

- [REST API, RESTful API](https://velog.io/@stampid/REST-API%EC%99%80-RESTful-API)
