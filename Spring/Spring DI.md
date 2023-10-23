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
