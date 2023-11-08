# Framework
- 개발할 때 설계 기본이 되는 뼈대나 구조.
- 여러 기능을 가진 클래스와 인터페이스가 어떠한 목적을 위해 합쳐진 구조.
- 예를 들어, 웹 프레임워크는 '웹 서버'를 구현하기 위한 목적으로 만들어진 프레임워크다.
- Spring, Apache, Bootstrap, Vue.js 등이 있다.
- 왜 사용하는가?
  - 완성된 구조에 맡은 기능을 개발하여 넣어주면 되서 생산성이 높아진다.

# Spring Framework
- Java 기반의 애플리케이션 프레임워크.
- 특징
  - **POJO(Plain Old Java Object)**: 순수 Java만 사용해서 만든 객체. 외부 라이브러리나 모듈 사용 x         
  외부 기술이나 규약의 변화에 얽매이지 않아, 보다 유연하게 변화와 확장에 대처할 수 있다.
  - **의존성 주입(DI, Dependency Injection)**: 각각의 계층이나 서비스들 간에 의존성이 존재할 경우 프레임워크가 서로 연결시켜준다.
  - **관점지향 프로그래밍(AOP, Aspect Oriented Programming)**: 트랜잭션이나 로깅, 보안과 같이 여러 모듈에서 공통적으로 사용하는 기능의 경우, 해당 기능을 분리하여 관리할 수 있다.
  - **제어 역전(IoC, Inversion of Control)**: 컨트롤의 제어권이 사용자가 아니라 프레임워크에 있어 필요에 따라 스프링에서 사용자의 코드를 호출한다.

# 의존성 주입 방법 3가지
### 1. 생성자 주입
- 생성자가 하나라면 @Autowired 생략 가능
```java
@Controller
public class Programmer {
  // final을 붙일 수 있다
  private final Computer computer;

  // 생성자를 이용한 의존성 주입
  // @Autowired 
  public Programmer(Computer computer) {
    this.computer = computer;
  }
}
```
### 2. 설정자 주입
- setXXX 메서드를 public으로 열어두어야 하기 때문에 언제 어디서든 변경이 가능하다.
```java
@Controller
public class Programmer {
  private Computer computer;

  // setter를 이용한 의존성 주입
  @Autowired
  public void setComputer(Computer computer) {
    this.computer = computer;
  }
}
```
### 3. 필드 주입
- 코드가 간결하지만, 외부에서 변경하기 힘들다.
- 프레임워크에 의존적으로 객체지향적으로 좋지 않다.
```java
@Controller
public class Programmer {
	
    @Autowired 
    private Computer Computer;
}
```
### 생성자 주입을 권장하는 이유
- 순환 참조 방지
- 불변성: 생성자 주입할 떄 final로 선언하여 변경의 가능성을 배제할 수 있다.
- 테스트 용이

# Spring IoC 컨테이너
- 객체를 생성하고 관리하고 책임지고 의존성을 관리해주는 컨테이너
- BeanFactory: Bean을 생성하고 분배하는 클래스. 보통 BeanFactory를 확장한 ApplicationContext를 사용한다.
- ApplicationContext: BeanFactory에서 이미지 로드 등 부가 기능을 추가로 제공한다.

# 스프링 설정 정보(Spring configuration metadata)
- 애플리케이션 작성을 위해 생성할 Bean과 설정, 의존성 등의 방법을 나타내는 정보
- XML 방식, Annotation 방식, Java 방식이 있다.

# Bean
- 스프링 컨테이너에 등록된 객체
- Bean 등록 방법 2가지
  - Annotatio
  - XML

# AOP(Aspect Oriented Programming)
- 관점 지향 프로그래밍
- 핵심 로직과 부가 기능을 분리하여 애플리케이션 전체에 걸쳐 사용되는 부가 기능을 모듈화하여 재사용할 수 있도록 하는 것
- 쉽게 말하면 **AOP는 공통 기능을 재사용하는 기법**이다.
- 스프링에서는 컴파일이 끝나고 런타임 시점에서 AOP를 적용한다.

# AOP 용어
![aop용어](https://github.com/SSAFYSEOUL06CSSTUDY/06CSSTUDY/assets/50236187/ad4684a7-80fe-4c23-8be3-2f245a5efe30)

# 참고
<a href="https://velog.io/@backtony/Spring-AOP-%EC%B4%9D%EC%A0%95%EB%A6%AC">Spring - AOP 총정리</a>

# Spring MVC 흐름
![MVC 동작 과정](https://github.com/namoo1818/TIL/assets/50236187/1585ea14-b33e-46cc-95fc-0a5b9d75b0f5)

# Filter
![필터](https://github.com/SSAFYSEOUL06CSSTUDY/06CSSTUDY/assets/50236187/89fe1105-fad2-4631-a911-803a7bacc661)
- 말 그대로 요청과 응답을 거른뒤 정제하는 역할
- Dispatcher Servlet에 요청이 전달되기 전 / 후에 url 패턴에 맞는 모든 요청에 대해 부가 작업을 처리할 수 있는 기능 제공
- 즉, 스프링 컨테이너가 아닌 톰캣과 같은 웹 컨테이너에 의해 관리가 되는 것이고, 스프링 범위 밖에서 처리되는 것(빈으로 등록 가능)
## Filter Method
- init() : 필터 객체를 초기화하고 서비스에 추가하기 위한 메소드
- doFilter() : HTTP 요청이 디스패처 서블릿으로 전달되기 전에 웹 컨테이너에 의해 실행되는 메소드
- destroy() : 필터 객체를 제거하고 사용하는 자원을 반환하기 위한 메소드

# Interceptor
![인터셉터](https://github.com/SSAFYSEOUL06CSSTUDY/06CSSTUDY/assets/50236187/30c25ce7-92c7-48c6-ae05-708dd650f9a9)
- 요청에 대한 작업 전 / 후로 가로챈다.
- Dispatcher Servlet이 Controller를 호출하기 전 / 후에 인터셉터가 끼어들어 요청과 응답을 참조하거나 가공할 수 있는 기능 제공
- 웹 컨테이너에서 동작하는 필터와 달리 인터셉터는 스프링 컨텍스트에서 동작
- 필터는 Request와 Response를 조작할 수 있지만, 인터셉터는 조작할 수 없다
- 스프링에서는 HandlerInterceptor 인터페이스해야 한다
## HandlerInterceptor Method
- preHandle() : Controller가 호출되기 전에 실행
- postHandle() : Controller가 호출된 후에 실행( View 렌더링 전)
- afterCompletion() : 모든 작업이 완료된 후에 실행( View 렌더링 후)

![필터와 인터셉터](https://github.com/SSAFYSEOUL06CSSTUDY/06CSSTUDY/assets/50236187/c8eb7981-ee8a-4093-b894-22b81dfbc944)

