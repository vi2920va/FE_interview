# Network - HTTP VS https

## 설명

### 1. HTTP

- HTTP(Hypertext Transfer Protocol)는 **통신 프로토콜**으로입니다.
- 프로트콜 이란 상호 간에 정의한 규칙을 의미하여 특정 기기 간에 데이터를 주고 받기 위해 정의 되었습니다.

#### 1-1. HTTP 특징

- HTTP 메시지는 HTTP 서버와 HTTP 클라이언트에 의해 해석이 됩니다.
- TCP/IP 이용하는 프로토콜 입니다.
- HTTP는 연결 상태를 유지 않는 비연결성 프로토콜 입니다. 이러한 단점을 해결하기 위해 **Cookie**와 **Session**이 등장하게 됩니다.
- HTTP는 연결을 유지하는 프로토콜이기 때문에 요청(Request)과 응답(Response) 방식으로 동작합니다.

#### 1-2 HTTP 요청 메서드

HTTP 요청 메서드(Http Request Methods)를 이용합니다. HTTP 요청 메서드 HTTP Verbs 라고도 불리우며 아래와 같이 주요 메서드를 갖고 있습니다.

- GET : 존재하는 자원에 대한 **요청**
- POST : 새로운 자원을 **생성**
- PUT : 존재하는 자원에 대한 전체 **변경**
- PATCH: 존재하는 자원에 대한 부분 **변경**
- DELETE: 존재하는 자원에 대한 **삭제**

### 2. HTTPS

- HTTPS(Hypertext Transfer Protocol Secure)란 사용자 컴퓨터와 방문한 사이트 간에 전송되는 사용자 데이터의 무결성과 기밀성을 유지할 수 있게 해주는 인터넷 통신 프로토콜입니다.

#### 2-1. HTTPS 특징

- 암호화(Encryption), 교환되는 데이터를 암호화하여 도정창치, 도청자로 부터 안전하게 보호됩니다.
- 데이터 무결성(Data Integrity), 의도적 또는 비의도적으로 감지 및 탐지되고 수정되거나 손상될 수 없습니다.
- 인증(Authentication), 사용자 의도한 웹 사이트와 통신하고 있음이 증명됩니다.

## 요약

### HTTP VS HTTPS

- HTTP는 암호화 추가되지 않았기 때문에 보안에 취약합니다. 반면에 HTTPS는 안전하게 데이터를 주고 받을 수 있습니다.
- HTTPS를 이용하면 암호화/복호화 과정이 필요하기 때문에 HTTP보다 속도가 느립니다.
- 개인정보와 민감한 데이터를 주고 받는 경우 HTTPS를 사용하기를 권장합니다. 단순희 정보 조회 등만 처리 등만 해야 된다면 HTTP를 사용해도 됩니다.

## 참고

- [HTTP와 HTTPS 및 차이점](https://mangkyu.tistory.com/98)
- [HTTP란 무엇인가?](https://velog.io/@surim014/HTTP%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%B8%EA%B0%80)
