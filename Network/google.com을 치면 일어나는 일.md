# google.com을 치면 일어나는 일

## 설명

제목에 대한 주제를 이해하려면 기본적인 네트워크의 기반지식이 갖추어져야 하는데 모든 지식을 다알아야 한다는 것은 아니고, 디버깅이나 웹이 돌아가는 일반적인 동작과정들을 이해해야 적용하는 과정을 빠르게 진행해주기 때문이다.

### 1. DNS 캐시 탐색

- 브라우저는 캐싱된 DNS(Domain Name System) 기록들을 통해 www.google.com 에 대응 되는 IP 주소가 있는지 검색합니다. 이전 블로깅에 올렸던 DNS를 보면 알 수 있습니다. [DNS](https://velog.io/@doodream/Domain-Name-System-DNS)

브라우저의 DNS 기록 검색과정은 아래와 같습니다.

- 브라우저 캐시확인 : 브라우저는 일정기간 유저가 방문한 사이트의 DNS 정보들을 기록하고 있습니다.
- OS 캐시확인 : 브라우저 캐시에도 없다면 브라우저에서는 운영체제에 `systemcall`을 통해서 OS에 기록되어있는 DNS 기록들의 캐시에 접근합니다.
  > systemcall : systemcall은 운영체제의 수준에서 커널 영역을 사용자 모드의 프로그램이 사용할 수 있게 해주는 기능입니다. ![](https://images.velog.io/images/doodream/post/55c0f4ce-1863-4190-a82f-9213715613b8/image.png)
- 라우터 캐시확인 : OSI 7계층중 데이터 통신계층에 있는 라우터에 들어있는 DNS 캐시를 조회합니다. 라우터 안에는 DNS 서버의 주소가 저장되어 있습니다. 라우터를 거치게 되어 DNS 주소가 없으면 ISP까지 올라가 확인하게 됩니다.
- ISP 캐시 : ISP는 DNS 서버를 구축하고 있습니다. 따라서 마지막으로 DNS 정보를 찾을 수 있는 마지막 단계입니다.

### 2. ISP의 DNS 서버가 보내는 DNS query

일단 ISP가 제공하는 DNS 서버를 `DNS recursor` 라고 합니다. 어느 곳의 DNS 캐시에서도 DNS 정보를 찾아내지 못했다면 ISP에서 제공하는 DNS서버 (DNS recursor) 에서 다른 DNS서버들로 해당 DNS 정보를 찾으려 DNS query를 보냅니다. IP 주소를 찾을 때까지 계속 다른 DNS 서버들을 오가며 찾던지 에러를 내든지 합니다.

쿼리로 검색하는 과정은 다음과 같습니다. 우선 도메인 이름 구조에 기반해서 검색합니다.
![](https://images.velog.io/images/doodream/post/3385e8b7-386f-4921-9304-6393fd180573/image.png)
위 사진을 참고해서 www.google.com 이라는 도메인을 분석 해봅시다. DNS recursor에서 `root name server`로 연락을 합니다. root name server는 `.com` 도메인 서버로 접근합니다. `.com` 도메인 서버는 `google.com` 도메인 서버로 접근합니다. `google.com` 네임서버는 www.google.com 에 매칭되는 IP주소를 찾고 DNS recurser로 보내게 됩니다.

이러한 모든 과정은 작은 패킷으로 전달되는 데 이 패킷안에는 DNS query와 DNS recursor의 IP 주소가 포함됩니다. 이러한 패킷이 이동할 때에 Routing table 을 이용하는데 우리가 알고있는 라우터에는 이러한 라우팅 테이블이 있어서 경로 정보를 등록하고 관리해서 최단 경로를 찾을 수 있습니다. 이 패킷들이 중간에 유실되면 request fail error 가 발생하게 됩니다.

### 3. 브라우저와 서버가 TCP 연결을 합니다.

브라우저가 서버의 IP 주소를 받게되면 연결을 시도하는데 웹사이트의 http 연결의 경우에는 일반적으로 TCP 연결을 합니다.
클라이언트와 서버간 데이터 패킷들이 오고가려면 TCP 연결이 이뤄져야 합니다. 3-way handshake 라는 과정에 의해 서버간 신뢰할 수 있는 연결이뤄집니다.

> ### 3-way handshake
>
> ![](https://images.velog.io/images/doodream/post/71502b16-04ba-47ba-9bf2-2febf2326918/image.png) > ![](https://images.velog.io/images/doodream/post/ed359899-c956-49e6-befc-fd3967e7f493/image.png)
> 컴퓨터간 신뢰할 수 있는 연결을 하기 위해서 연결을 시작할 때 위 그림의 초록색과정을 거치게 됩니다. SYN ACK 등의 데이터들은 OSI계층 순서대로 헤더 혹은 트레일러가 데이터에 붙여져지며 캡슐화와 역캡슐화가 일어나는데 이러한 TCP헤더에 들어가는 데이터입니다. ( 데이터와 TCP헤더가 붙은 것을 세그먼트라고 부릅니다.) 이 TCP 헤더중 코드 비트라는 곳에 6개 비트중에 하나입니다. 데이터를 주고 받을 때는 하늘색 과정, 연결을 끊을 때는 빨간색과정이 일어납니다. 연결을 요청할때는 SYN, ACK가 1로 활성화, 종료할때는 FIN, ACK가 1로 활성화 됩니다.

### 4. 브라우저가 웹서버에 HTTP 요청을 합니다.

연결이 되었다면 데이터를 전송합니다. 클라이언트의 브라우저는 HTTP 프로토콜로 GET 요청을 통해 서버에게 www.google.com의 웹페이지 데이터를 요구합니다.
![](https://images.velog.io/images/doodream/post/4cbcc31f-2671-4db4-8337-08c29a3b886b/image.png)

### 5. 서버가 request를 처리하고 response를 생성합니다.

서버는 브라우저로부터 request를 받아 처리합니다. 그리고 response를 생성해서 보내줍니다. request에서는 http 요청을 통해 받아온 다양한 헤더 정보와 데이터들로 요청을 처리하고 response를 생성합니다. 이때 response는 특정한 포멧 (JSON, XML, HTML ) 등으로 작성합니다.
![](https://images.velog.io/images/doodream/post/eec25b58-4016-48b8-9425-2024b9ca7542/image.png)

### 6. 서버가 HTTP response를 보냅니다.

서버의 response에는 HTTP 프로토콜에 따른 헤더 정보와 데이터들 보냅니다. 이때 status code로 response 상태를 표현합니다.
![](https://images.velog.io/images/doodream/post/93ade189-c703-4e03-baa1-b74a16123b55/image.png)

### 7.브라우저가 HTML content를 보여줍니다.

이때 Critical Path 과정을 통해서 브라우저가 렌더링과정을 거치게 됩니다. 이렇게 사용자에게 google.com 화면이 보여지게 됩니다. 이때 정적인 파일들은 브라우저에 의해 캐싱이 되어서 해당페이지를 다시 방문할 경우 서버로 부터 다시 데이터를 요청하지 않도록 합니다.

## 요약

- DNS 캐시 탐색 : 도메인의 IP 주소를 찾기위해서 DNS 캐시에서 DNS 기록을 찾습니다.
- ISP의 DNS 서버가 보내는 DNS query : ISP 의 DNS 캐시에도 없다면 다른 DNS 서버에서 DNS 기록을 찾으려 DNS query를 날립니다.
- 브라우저와 서버가 TCP 연결을 합니다. : IP주소를 찾으면 해당 IP주소에 해당하는 웹서버와 통신을 시작하기 위해 신뢰할 수 있는 TCP 연결과정인 3-way handshake를 합니다.
- 브라우저가 웹서버에 HTTP 요청을 합니다 : 신뢰할 수 있는 연결이 마련되었으면 이제 데이터를 보냅니다. 이때 HTTP 프로토콜을 통해서 데이터를 보냅니다.
- 서버가 request를 처리하고 response를 생성합니다. : 웹 서버는 request를 받아 처리하고 response를 생성합니다.
- 서버가 HTTP response를 보냅니다 : HTTP 프로토콜을 따라 서버가 request를 받아 생성한 response를 보냅니다.
- 브라우저가 HTML content를 보여줍니다 : 서버에 요청했던 response에 담겨있던 HTML 문서, js 스크립트 , image등 데이터들을 Critical Path 렌더링 과정을 통해 렌더링 하면 사용자는 www.google.com 화면을 볼 수 있습니다.

## 참고

- [운영체제 04 : 시스템 콜 (시스템 호출, System Call)](https://luckyyowu.tistory.com/133)
- [어떻게: DNS 캐시 란 무엇이며 어떻게 작동합니까?](https://kor.go-travels.com/45747-what-is-a-dns-cache-817514-7367974)
- [Browser에 www.google.com을 검색하면 어떤 일이 일어날까?](https://devjin-blog.com/what-happen-browser-search/)
