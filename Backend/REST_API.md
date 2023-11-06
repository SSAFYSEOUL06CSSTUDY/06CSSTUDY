# REST API

REST API란 REST를 기반으로 만들어진 API

## REST?

REST(Representational State Trasfer)의 약자, 자원을 이름으로 구분하여 해당 자원의 상태를 주고 받는 모든 것

**즉 REST란**
1. HTTP URI(Uniform Resource Identifier)를 통해 자원(Resource)을 명시하고,
2. HTTP Method(POST,GET,PUT,DELETE,PATCH 등)를 통해
3. 해당 자원(URI)에 대한 CRUD Operation을 적용하는 것을 의미함.

> **CRUD Operation이란**
> CRUD는 대부분의 컴퓨터 소프트웨어가 가지는 기본적인 데이터 처리 기능인 Create(생성), Read(읽기), Update(갱신), Delete(삭제)를 묶어서 일컫는 말로 
> REST에서의 CRUD Operation 동작 예시는 다음과 같음.

### REST 구성 요소

REST는 다음과 같은 3가지로 구성. 

1. 자원(Resource) : HTTP URI
2. 자원에 대한 행위(Verb) : HTTP Method
3. 자원에 대한 행위의 내용 (Representations) : HTTP Message Pay Load

### REST의 특징

1. Server-Client(서버-클라이언트 구조) : 클라이언트와 서버 애플리케이션은 서로 간에 완전 독립적이어야함(서로 알아야 하는 유일한 정보는 URI).
2. Stateless(무상태) : 각 요청에서 이의처리에 필요한 모든 정보를 포함해야 함.
3. Cacheable(캐시 처리 가능) : 가능하면 리소스를 클라이언트 또는 서버측에서 캐싱할 수 있어야 함(서버 확장성 증가, 클라이언트 성능 향상).
4. Layered System(계층화) : 호출과 응답이 서로 다른 계층을 통과함. 
5. Uniform Interface(인터페이스 일관성) : 요청이 어디에서 오는지와 무관하게, 동일한 리소스에 대한 모든 API요청은 동일하게 보여야 함.

### REST의 장단점

장점

 - HTTP 프로토콜의 인프라를 그대로 사용하므로 REST API 사용을 위한 별도의 인프라를 구출할 필요가 없음.
 - HTTP 프로토콜의 표준을 최대한 활용하여 여러 추가적인 장점을 함께 가져갈 수 있게 해 줌.
 - HTTP 표준 프로토콜에 따르는 모든 플랫폼에서 사용이 가능.
 - Hypermedia API의 기본을 충실히 지키면서 범용성을 보장.
 - REST API 메시지가 의도하는 바를 명확하게 나타내므로 의도하는 바를 쉽게 파악가능.
 - 여러 가지 서비스 디자인에서 생길 수 있는 문제 최소화.
 - 서버와 클라이언트의 역할을 명확하게 분리.

 단점
 
 - 표준이 자체가 존재하지 않아 정의가 필요함.
 - HTTP Method 형태가 제한적.
 - 브라우저를 통해 테스트할 일이 많은 서비스라면 쉽게 고칠 수 있는 URL보다 Header 정보의 값을 처리해야 하므로 전문성이 요구됨.
 - 구형 브라우저에서 호환이 되지 않아 지원해주지 못하는 동작이 많음.(익스폴로어)
   
## REST API?

API(애플리케이션 프로그래밍 인터페이스)는 애플리케이션이나 디바이스가 서로 간에 연결하여 통신할 수 있는 방법을 정의하는 규칙 세트.<br>
REST API는 REST(REpresentational State Transfer) 아키텍처 스타일의 디자인 원칙을 준수하는 API<br>
이러한 이유로 REST API를 RESTful API라고도 함<br>

REST API를 올바르게 설계하기 위해서는 지켜야 하는 몇가지 규칙이 있음.

#### REST API 설계 예시

1. URI는 동사보다는 명사를, 대문자보다는 소문자를 사용하여야 함.
```
Bad Example : http://khj93.com/Running/
Good Example : http://khj93.com/run/  
```
2. 마지막에 슬래시(/)를 포함하지 않음.
```
Bad Example : http://khj93.com/test/  
Good Example : http://khj93.com/test
```
3. 언더바 대신 하이픈을 사용함.
```
Bad Example : http://khj93.com/test_blog
Good Example : http://khj93.com/test-blog  
```
4. 파일확장자는 URI에 포함하지 않음.
```
Bad Example : http://khj93.com/photo.jpg  
Good Example : http://khj93.com/photo  
```
5. 행위를 포함하지 않음.
```
Bad Example : http://khj93.com/delete-post/1  
Good Example : http://khj93.com/post/1  
```

## RESTful?

<img src="https://github.com/myeon0109/06CSSTUDY/blob/2259617e38a025de8c2c98817da24c7ee37cac43/image/%EB%8B%A4%EC%9A%B4%EB%A1%9C%EB%93%9C2.png">

RESTFUL이란 REST의 원리를 따르는 시스템을 의미함. <br>
하지만 REST를 사용했다 하여 모두가 RESTful 한 것은 아님. <br> 
REST API의 설계 규칙을 올바르게 지킨 시스템을 RESTful하다 말할 수 있으며 <br>
모든 CRUD 기능을 POST로 처리 하는 API 혹은 URI 규칙을 올바르게 지키지 않은 API는 <br>
REST API의 설계 규칙을 올바르게 지키지 못한 시스템은 REST API를 사용하였지만 RESTful 하지 못한 시스템이라고 할 수 있음.<br>


<br>  <br>  <br>  <br>
  
[ 참고링크 ] 
 - https://gmlwjd9405.io/2018/09/21/rest-and-restful.html
 - www.incodom.kr/REST








