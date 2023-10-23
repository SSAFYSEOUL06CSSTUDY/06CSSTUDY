### 목차

1. [**REST란?**](#1-REST란?)
2. [**HTTP**](#2-HTTP)
3. [**REST 특징**](#3-REST-특징)
4. [**REST API 설계 **](#4-REST-API-설계)



# 1. REST란?

### REprsentational State Transfer

- 자원을 이름으로 구분해 해당 자원의 상태를 전송하여 주고 받는 것.
- WEB 기술 + HTTP 프로토콜을 활용한 아키텍처 스타일 방식

![스크린샷 2023-10-23 오후 11 03 51](https://github.com/Youth787/SSAFY_CS_Study/assets/90955152/59a9824f-a32b-4e84-abb1-794ce6d15ba2)

<br/>
<br/>

# 2. HTTP

> ## HTTP 프로토콜

![스크린샷 2023-10-23 오후 11 05 58](https://github.com/Youth787/SSAFY_CS_Study/assets/90955152/1e575c4e-e060-4a00-ab90-861e2408938b)

<br/>

> ## HTTP 응답 코드

- 1xx (정보전송 임시 응답) : 전송 프로토콜 수준의 정보 교환 (요청 후 프로세스 계속 진행)
- 2xx (성공) : 클라이언트 요청이 성공적으로 수행됨
- 3xx (리다이렉트) : 클라이언트는 요청을 완료하기 위해 추가적인 행동을 취해야 한다. (변경된 url)
- 4xx (클라이언트 에러) : 클라이언트의 잘못된 요청
- 5xx (서버 에러) : 서버쪽 오류로 인한 상태코드

![스크린샷 2023-10-23 오후 11 09 27](https://github.com/Youth787/SSAFY_CS_Study/assets/90955152/72594d0f-1b16-4983-932a-21a62bb005a9)

<br/>

> ## HTTP 메소드

![스크린샷 2023-10-23 오후 11 10 06](https://github.com/Youth787/SSAFY_CS_Study/assets/90955152/d8a3886f-dcdb-48c7-92f2-84a9c8d79957)

idempotent : 멱등 (한번 수행했을때, 여러번 수행했을때 결과가 같은가)
- 서비스 시 지원해야 할 메소드
  - GET, POST, HEAD, OPTIONS
- 나머지 메소드는 보안 취약점 야기 가능성 존재하므로 비활성화 필요

- PUT : Web Shell을 통한 시스템 침투가 가능(파일 업로드)
- DELETE : 클라이언트에서 웹 서버 파일 삭제 가능
- CONNECT : HTTP 프록시 악용 가능
- TRACE : XST 공격으로 세션 탈취 가능 

<br/>
<br/>

# 3. REST 특징


> ## Server-Client

- Server : 자원 존재 / Client : 자원 요청
- 클라이언트와 서버의 의존성 감소
  
<br/>


> ## Stateless

- Client의 context를 Server에 저장하지 않음 => 구현 단순
- Server는 모든 요청을 각자 별개의 것으로 인식하고 처리
  
<br/>


> ## Cacheable

- HTTP의 캐싱 기능 활용 가능 => 응답 시간, 성능 빨라짐
<br/>


> ## Uniform Interface

- 특정 언어에 종속되지 않고 HTTP 프로토콜을 따르면 모든 플랫폼에 적용 가능 (loosely Coupling)
<br/>



# 4. REST API 설계 

![스크린샷 2023-10-23 오후 11 15 34](https://github.com/Youth787/SSAFY_CS_Study/assets/90955152/ec81138a-0523-4823-a2c4-f7a5065b3bb4)


> ## 설계 규칙 

- '/'는 계층 관계를 나타낸다.
- URL 마지막 문자에는 '/' 포함 X
- URL 길어지면 '-'(하이픈)으로 가독성 높인다.
- '-'(밑줄)은 URL에 사용 X
- URL 경로는 소문자 사용
- 파일 확장자는 URI에 포함 X => Accept Header 활용
  
<br/>
<br/>

> ## Resource

- 동사보다 명사, 대문자보다 소문자
- 객체 인스턴스, DB: 단수 명사
- 서버 및 클라이언트 리소스 : 복수 명사

<br/>
<br/>

# 5. 요약 

### REST API란 무엇인가?
: 웹 상에서 REST는 HTTP와 Method로 자원들을 구분하여 표현한 상태를 클라이언트와 서버가 서로 전송을 주고 받아 CRUD 작업을 진행하는 것이다. 이런 인터페이스를 사용자가 활용할 수 있도록 구축해둔 것이 REST API라고 할 수 있다. 

### RESTful하다는게 무슨 뜻인가?
: REST API를 제공하는 웹 서비스를 RESTful하다고 말할 수 있다. 설계 규칙에 맞게 통용되는 일관된 컨벤션을 유지하며 API의 이해도를 높여주는 것이 중요하다. 

### REST에 사용하는 HTTP 메소드는 무엇이 있는가?
: GET, HEAD, POST, PUT, PATCH, CONNECT, TRACE, OPTIONS

### REST가 필요한 이유는? 장단점은 무엇인가?
: 이젠 웹 서버에서 웹 브라우저, 안드로이드, IOS등 다양한 멀티 플랫폼에서 통신을 지원할 수 있어야한다. 이에 필요한 아키텍처로 REST가 통용되기 시작했으며, 다양한 클라이언트를 대응할 수 있게 되었다. 장점은 HTTP 프로토콜 인프라를 사용하여 추가 인프라 구축이 필요 없고, API 메세지 의미가 명확하므로 이해도가 높아지고 있으며, 클라이언트와 서버의 의존도를 낮출 수 있다. 


