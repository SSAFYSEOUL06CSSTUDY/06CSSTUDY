# REST API
  
# API란?
- Application Programming Interface
> 소프트웨어가 다른 소프트웨어로부터 지정된 형식으로 요청, 명령을 받을 수 있는 수단
- Web API, Window API 등

# REST란?
- Representational State Transfer
> HTTP URI(Uniform Resource Identifier)를 통해 자원(Resource)을 명시하고   
> HTTP Method(POST, GET, PUT, DELETE, PATCH 등)를 통해  
> 해당 자원(URI)에 대한 CRUD을 적용하는 것을 의미한다   

# REST 구성 요소
1. 자원(Resource): URI
   - 모든 자원에 고유한 ID가 존재하고, 이 자원은 Server에 존재한다.
   - 자원을 구별하는 ID는 ‘/groups/:group_id’와 같은 HTTP URI 다.
   - Client는 URI를 이용해서 자원을 지정하고 해당 자원의 상태(정보)에 대한 조작을 Server에 요청한다.
2. 행위(Verb): HTTP Method
   - HTTP 프로토콜의 Method를 사용한다.
   - HTTP 프로토콜은 **GET, POST, PUT, DELETE** 와 같은 메서드를 제공한다.
3. 표현(Representation of Resource)
   - Client가 자원의 상태(정보)에 대한 조작을 요청하면 Server는 이에 적절한 응답(Representation)을 보낸다.
   - REST에서 하나의 자원은 **JSON, XML, TEXT, RSS** 등 여러 형태의 Representation으로 나타내어 질 수 있다.
   - JSON 혹은 XML를 통해 데이터를 주고 받는 것이 일반적이다

# REST API란? 
> REST의 원리를 따르는 API로 정보를 주고받을 때 개발자들 사이에서 정한 약속
- 형식이기 때문에 기술에 구애받지 않는다
- 과거 SOAP이란 복잡한 형식을 대체한 것
- 각 요청의 의도를 쉽게 파악 가능
- 개발자는 혼자 개발하는 것이 아니기 때문에 RESTful하게 API를 만들어야 한다 
- 서버에 REST API로 요청을 보낼 때는 HTTP란 규약에 따라 신호를 전송함

# HTTP Method 
<img href="http method" src="https://github.com/SSAFYSEOUL06CSSTUDY/06CSSTUDY/assets/50236187/340134b1-ebfe-4bc9-86f9-c7d3bf12f1f0" height=200> <br>
- post, put, patch는 body라는 주머니가 있어서 정보를 get이나 delete보다 많이, 그리고 비교적 안전하게 감춰서 실어보낼 수 있다
- 이들의 목적에 따라 구분해서 사용해야 한다
### GET: 정보 조회 Read
> 2반 학생을 보는 요청   
> http://(도메인)/classes/2/students

### POST: 정보 추가 Create, 추가된 정보를 body에 실어보낸다
> 2반에 새로운 학생을 추가하는 요청   
> https://(도메인)/classes/2/students

### PUT, PATCH: 정보 수정 Update, 수정된 정보를 body에 실어보낸다
- PUT은 정보를 통째로 갈아끼울 때, PATCH는 정보 중 일부를 변경할 때 사용
> 2반의 index가 14인 학생의 정보 수정   
> https://(도메인)/classes/2/students/14

### DELETE: 정보 삭제 Delete

# REST API 규칙
1. URI는 동사보다는 명사를, 대문자보다는 소문자를 사용하여야 한다.
2. 마지막에 슬래시(/)를 포함하지 않는다.
3. 언더바(_) 대신 하이폰(-)을 사용한다.
4. 파일확장자는 URI에 포함하지 않는다.
5. 행위를 포함하지 않는다.

# REST 특징
### 1. Uniform Interface(인터페이스 일관성)
   URI로 지정한 리소스에 대한 조작을 통일되고 한정적인 인터페이스로 수행한다.    
### 2. Stateless(무상태) 
   REST는 무상태성 성격을 갖고, 작업을 위한 상태정보를 따로 저장하거나 관리하지 않는다.      
   세션 정보나 쿠키정보를 별도로 저장하고 관리하지 않기 때문에 API 서버는 들어오는 요청만을 단순히 처리하면 된다.    
   이를 통해서 서비스의 자유도가 높아지고 서버에서 불필요한 정보를 관리하지 않는다.    
### 3. Cacheable(캐시 처리 가능)    
   기존 웹 표준을 그대로 사용하기 때문에 HTTP가 가진 캐싱 기능을 적용할 수 있다.   
### 4. Self-descriptiveness     
   메서지의 모든 요소는 메시지만 보고 그 뜻을 알아야 한다.      
### 5. Server-Client(서버-클라이언트 구조)    
   REST 서버는 API를 제공하고, 클라이언트는 사용자 인증이나 컨텍스트 등을 직접 관리하는 구조로 각각의 역할이 확실히 구분되기 때문에 클라이언트와 서버에서 개발해야 할 내용이 명확해지고 서로간의 의존성이 줄어들게 된다.     
### 6. 계층형 구조     
   REST 서버는 다중 계층으로 구성될 수 있으며, 로드 밸런싱, 암호화 계층 등을 추가해 구조상의 유연성과 프록시, 게이트웨이 같은 네트워크 기반의 중간매체를 사용할 수 있게 한다.    

# REST 장단점
## 장점
- 확장성: 클라이언트-서버 상호 작용을 최적화하기 때문에 효율적으로 크기 조정할 수 있다.
- 유연성: RESTful 웹 서비스는 완전한 클라이언트-서버 분리를 지원한다.
- 독립성: REST API는 사용되는 기술과 독립적이다. API 설계에 영향을 주지 않고 다양한 프로그래밍 언어로 클라이언트 및 서버 애플리케이션을 모두 작성하거나 변경할 수 있다.
## 단점
- 표준이 존재하지 않는다.
- 사용할 수 있는 메소드가 4가지 밖에 없다.
  - HTTP Method 형태가 제한적이다.

# 참고
<a href="https://mslilsunshine.tistory.com/167">REST API, 알고 사용하자!</a> <br>
<a href="https://gmlwjd9405.github.io/2018/09/21/rest-and-restful.html">[Network] REST란? REST API란? RESTful이란?</a> <br>
<a href="https://khj93.tistory.com/entry/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC-REST-API%EB%9E%80-REST-RESTful%EC%9D%B4%EB%9E%80">[네트워크]REST API란?REST,RESTful이란?</a> <br>
<a href="https://meetup.nhncloud.com/posts/92">REST API 제대로 알고 사용하기</a> <br>
<a href="https://aws.amazon.com/ko/what-is/restful-api/">RESTful API란 무엇인가요?</a> <br>
<a href="https://youtu.be/iOueE9AXDQQ?si=-6ow9jAurn4wWsNE">REST API가 뭔가요?</a> <br>
<a href="https://youtu.be/RP_f5dMoHFc?si=KGfWSxLy0XDpLvWV">그런 REST API로 괜찮은가</a>
