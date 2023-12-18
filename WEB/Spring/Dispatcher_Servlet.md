### 목차

1. [**미리 알아야 할 것 들**](#1-미리-알아야-할-것-들)
2. [**디스패쳐 서블릿**](#2-디스패쳐-서블릿)
3. [**필터 & 인터셉터 (Filter & Interceptor)**](#3-필터--인터셉터-filter--interceptor)

# 디스패쳐 서블릿(Dispatcher Servlet)

## 1. 미리 알아야 할 것 들

> 디스패쳐 서블릿을 이해 하기 위해서 선행으로 알아야 할 내용

### 1-1. Java EE(Jakarta EE)

> 기업용 애플리케이션을 개발/실행하기 위한 기술과 환경을 제공하며 서블릿(Servlet), JSP, JDBC 등의 기술을 포함 하고 있는 **분산 애플리케이션 개발 목적의 산업 표준 플랫폼**

쉽게 말해서 자바 언어로 어플리케이션을 쉽게 개발할 수 있도록 필요한 것 들을 모아둔 것

### 1-2. 서블렛(Servlet)

> Java EE의 요소 중 하나로 자바를 사용하여 웹페이지를 동적으로 생성하는 서버측 프로그램을 의미

    즉, 서블릿(Servlet)은 클라이언트 요청을 처리하고, 그 결과를 반환하는 웹 프로그래밍 기술

## 2. 디스패쳐 서블릿(Dispatcher Servlet)

> 디스패쳐 서블릿도 상속 구조를 보면 결국 서블릿<br>

    DispatcherServlet` -> `FrameworkServlet` -> `HttpServletBean` -> `HttpServlet`

> 해당 작업에 맞는 컨트롤러를 찾아서 정해주는 역할을 하는 서블릿

```java
public class DispatcherServlet extends FrameworkServlet {
}

public abstract class FrameworkServlet extends HttpServletBean {
}

public abstract class HttpServletBean extends HttpServlet {
}
```

### 2-1. 디스패쳐 서블릿의 개념(with 프론트 컨트롤러)

- 디스패처 서블릿은 스프링 MVC의 중앙 서블릿, 어플리케이션으로 오는 모든 요청을 핸들링하고 공통작업을 처리
- 실제 작업은 해당 작업에 적합한 컨트롤러에 위임해 처리하므로, 중계 역할만 처리
- 서블릿 컨테이너 맨 앞에서 모든 요청을 가장 먼저 받아 처리해주기 때문에, **프론트 컨트롤러** 라고 부름

### 2-2. 디스패쳐 서블릿의 동작과정(with Rest_Spring)

![디스패쳐 서블릿 동작과정](https://github.com/SSAFYSEOUL06CSSTUDY/06CSSTUDY/assets/108852263/7c58e19a-91d3-4068-a469-af1e7c6e5a92)

> 스프링 MVC에서 디스패쳐 서블릿의 동작과정을 설명, 필터와 인터셉터는 잠시 제외하고 설명

### **[Step1] 클라이언트의 요청이 디스패처 서블릿으로 전달**

- 프론트 컨트롤러인 디스패처 서블릿으로 요청이 가장 처음 전달

### **[Step2] 요청 정보를 통해 핸들러 매핑을 거쳐 요청을 위임할 컨트롤러를 찾음**

- 어느 컨틀로러가 해당 요청을 처리 할 수 있는지를 찾는 핸들러 매핑(Handler Mapping)을 통해 컨트롤러(핸들러)를 찾음
- 해당 컨틀롤러의 메소드를 호출
- 일반적으로 @Controller에 @RequestMapping 관련 어노테이션을 사용해 컨트롤러를 작성

### **[Step3] 요청을 핸들러 어댑터를 통해서 컨트롤러로 전달**

- 컨트롤러로 요청을 전달
- 디스패처 서블릿이 직접 전달하지는 않고 핸들러어뎁터(HandlerAdapter)를 통해 컨트롤러에 요청을 전달
- 왜 굳이 핸들러 어댑터를? ⇒ 스프링 프레임워크는 변화가 다양하기 때문에 컨트롤러의 구현 방식에 상관없이 요청을 전달 할 수 있도록 따로 어댑터를 두어 요청이 잘 전달 될 수 있도록 어댑터 패턴 적용

### **[Step4] 비지니스 로직을 처리**

- 컨트롤러는 연결되어있는 서비스를 호출하고 비지니스 로직을 실행

### **[Step5] 컨트롤러가 반환값을 반환**

- 비지니스 로직이 처리 후에는 컨트롤러가 응답값을 반환
- 클라이언트와 서버가 독립적으로 구성 되어 있는 상황이라면**(ex: React+Spring)**응답 데이터를 사용하는 경우에는 주로 ResponseEntity를 반환, 그외 응답 페이지를 보여주는 경우라면 View 반환

### **[Step6] 핸들러 어댑터가 반환값을 처리**

- 컨트롤러가 반환한 응답값은 핸들러어댑터가 ReturnValueHandler로 데이터 후 처리 후 디스패처 서블릿으로 반환
- (ResponseEntity 반환시) HttpEntityMethodProcessor를 통해 Http상태 설정
- (View 반환시) ViewResolver를 통해 View를 반환

### **[Step7] 서버의 응답을 클라이언트로 반환**

- 클라이언트로 최종 응답 값 반환
- 응답이 데이터면 그대로 반환, 응답값이 화면이면 ViewResolver를 통해 해당화면 호출

## 3. 필터 & 인터셉터 (Filter & Interceptor)

> 요청과 응답을 비즈니스 로직과 분리해서 처리해야 할 때 사용하는 클래스

![디스패쳐서블릿필터컨트롤러](https://github.com/SSAFYSEOUL06CSSTUDY/06CSSTUDY/assets/108852263/a9e7a903-db9f-4bda-8563-1d3e55d4659b)

### 3-1. 필터

- Dispatcher Servlet에 요청이 전달되기 전 / 후에 url 패턴에 맞는 모든 요청에 대해 부가 작업을 처리할 수 있는 기능을 제공
- 스프링 컨테이너가 아닌 톰캣과 같은 웹 컨테이너에 의해 관리 및 실행

### 3-2. 인터셉터

- Dispatcher Servlet이 Controller를 호출하기 전 / 후에 인터셉터가 끼어들어 요청과 응답을 참조하거나 가공할 수 있는 기능을 제공
- 웹 컨테이너에서 동작하는 필터와 달리 인터셉터는 스프링 컨텍스트에서 동작

### 3-3. 차이점 비교

- 필터는 Request와 Response를 조작할 수 있지만, 인터셉터는 조작할 수 없음
  => 필터는 객체를 바꿔 칠 수 있지만, 인터셉터는 불가능 대신 해당 객체의 값 변경은 가능
- 필터는 Spring이 아닌 SevletRequest의 하위 요소 이므로 Spring과 별도로 사용 가능(Java EE 표준)
- 인터셉터는 Spring에서 제공하는 기능
- 따라서, 웹 컨테이너(서블릿 컨테이너)에서 동작하는 필터와 달리 인터셉터는 스프링 컨텍스트에서 동작

```java
public class MyFilter implements Filter {

    @Override
    public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain)
    throws IOException, ServletException {
        // 다른 request와 response를 넣어줄 수 있음
        chain.doFilter(request, response);
    }
}
```

> 필터는 응답과 요청을 상속해 주므로 수정 가능

```java
public class MyInterceptor implements HandlerInterceptor {

    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler)
    throws Exception {
        // Request, Response를 교체할 수 없고 boolean 값만 반환 가능
        return true;
    }
}
```

> 인터셉터는 true false 만 반환하므로 조작이 불가능
