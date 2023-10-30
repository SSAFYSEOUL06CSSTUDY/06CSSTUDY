### 목차

1. [**스프링 인터셉터란?**](#1-스프링-인터셉터란?)
2. [**스프링 인터셉터의 흐름**](#2-스프링-인터셉터의-흐름)
3. [**HandlerInterceptor 인터페이스**](#3-HandlerInterceptor-인터페이스)
4. [**스프링 인터셉터 예외**](#4-스프링-인터셉터-예외)
5. [**Interceptor VS Filter**](#4-Interceptor-VS-Filter)


# 1. 스프링 인터셉터란?

- 스프링 MVC 에서 인터셉터는 웹 애플리케이션 내에서 특정한 URI 호출을 말 그대로 '가로채는' 역할 <br>
- 요청과 응답을 가로채서 원하는 동작을 추가하는 역할<br>

<br>
예를 들자면, 세션을 통한 인증을 쉽게 구현할 수 있다.
<br>
요청을 받아 들이기 전, 세션에서 로그인한 사용자가 있는지 확인해보고 없다면 로그인 페이지로 redirect 시킬 수 있다.
<br>
Interceptor가 없다면 모든 컨트롤러마다 해당 로직을 넣어야 하니, 꽤나 번거롭고 비효율적.
<br>
<br>

![스크린샷 2023-10-30 오후 9 27 14](https://github.com/Youth787/SSAFY_CS_Study/assets/90955152/7338fa23-ec02-4341-9147-8846aadd0853)

<br/>
<br/>

# 2. 스프링 인터셉터의 흐름

> ### HTTP 요청 -> WAS -> 필터 -> 서블릿 -> 스프링 인터셉터 -> 컨트롤러
<br>

![스크린샷 2023-10-30 오후 9 30 45](https://github.com/Youth787/SSAFY_CS_Study/assets/90955152/00568da1-19e5-4b4b-9c0e-bdf2668dd581)

1. 각 메서드의 반환값이 true이면 핸들러의 다음 동작이 실행되지만 false이면 중단되어서 남은 인터셉터와 컨트롤러가 실행되지 않는다.

> 스프링 인터셉터 제한 

> HTTP 요청 -> WAS -> 필터 -> 서블릿 -> 스프링 인터셉터 -> 컨트롤러 // 로그인 사용자 <br>
> HTTP 요청 -> WAS -> 필터 -> 서블릿 -> 스프링 인터셉터(적절하지 않은 요청이라 판단, 컨트롤러 호출 X) // 비 로그인 사용자

&nbsp; 인터셉터에서 적절하지 않은 요청이라고 판단하면 거기에서 끝을 낼 수도 있다. <br>

2. 인터셉터는 컨트롤러 호출 전(preHandle), 호출 후(postHandle), 요청이 완료 후(afterCompletion)와 같이 단계적으로 세분화되어있다.
3. 인터셉터는 어떤 컨트롤러(hanlder)가 호출되었는지 정보를 받을 수 있고, 어떤 modelAndView가 반환되는지 응답 정보도 받을 수 있다.
  
<br/>
<br/>
<br/>

# 3. HandlerInterceptor 인터페이스
<br>

    public interface HandlerInterceptor {

    default boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler)
            throws Exception {
        return true;
    }
    
    default void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler,
            @Nullable ModelAndView modelAndView) throws Exception {
    }
    
    default void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler,
            @Nullable Exception ex) throws Exception {
    }
    }


> ### preHandle
> preHandle(request,response,handler)

- preHandle 메서드는지정된 컨트롤러의 동작 이전에 가로채는 역할
- HttpServletRequest, HttpServletResponse, Object handler 3개의 파리미터를 사용

> ### postHandle
> postHandle(request,response,handler,modelAndView)

- 컨트롤러 실행이 끝나고 아직 화면처리는 안 된 상태이므로, 필요하다면 메소드의 실행 이후에 추가적인 작업이 가능

> ### afterCompletion
> afterCompletion(request, response, handler, exception)

- DispatcherSerlvet의 화면 처리(뷰)가 완료된 상태에서 처리.

<br>
<br>


# 4. 스프링 인터셉터 예외

![스크린샷 2023-10-30 오후 9 51 09](https://github.com/Youth787/SSAFY_CS_Study/assets/90955152/3d8f9712-7e07-4959-9c35-4c91ddb64b24)

- preHandle : 컨트롤러 호출 전에 호출된다. <br>
- postHandle : 컨트롤러에서 예외가 발생하면 postHandle 은 호출되지 않는다. <br>
- afterCompletion : afterCompletion 은 항상 호출된다. <br>
  이 경우 예외( ex )를 파라미터로 받아서 어떤 예외가 발생했는지 로그로 출력할 수 있다. <br>


# 5. Interceptor VS Filter

> 공통점

  Servlet 기술의 Filter와 Spring MVC의 HandlerInterceptor는 특정 URI에 접근할 때 제어하는 용도로 사용된다는 공통점을 가지고 있다. <br>
> 차이점

1. 영역의 차이

   ![스크린샷 2023-10-30 오후 9 55 51](https://github.com/Youth787/SSAFY_CS_Study/assets/90955152/3bd99c98-c3f9-4afe-ace5-891e2026ca4e)

   Filter는 동일한 웹 애플리케이션의 영역 내에서 필요한 자원들을 활용합니다. <br>
   웹 애플리케이션 내에서 동작하므로, 스프링의 Context를 접근하기 어렵습니다. <br>

   Interceptor의 경우 스프링에서 관리되기 때문에 스프링 내의 모든 객체(빈)에 접근이 가능하다는 차이가 있다. <br>
   즉, 빈을 관리하는 스프링 컨텍스트 내에 있어서 생성된 빈들에 자유롭게 접근할 수 있다. <br>

2. 호출 시점의 차이

   Filter는 DispatcherServlet이 실행되기 전, Interceptor는 DispatcherServlet이 실행된 후에 호출.<br>
   Interceptor는 DispatcherServlet이 실행되며 호출.<br>

3. 용도의 차이

   filter : 1) 보안 관련 공통 작업 2) 모든 요청에 대한 로깅 또는 감사 3) 이미지/데이터 압축 및 문자열 인코딩<br>
   interceptor : 1) 인증/인가 등과 같은 공통 작업 2) Controller로 넘겨주는 정보의 가공 3) API 호출에 대한 로깅 또는 감사<br>


### 참고자료 
https://gngsn.tistory.com/153 <br>
https://kihwan95.tistory.com/7

 
