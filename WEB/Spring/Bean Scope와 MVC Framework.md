# Bean
- Spring에서 사용하는 POJO(plain, old java object) 기반 객체
- 일반적으로 XML 파일에 정의
- Bean Scope는 기본적으로 Singleton이지만 필요에 경우 개발자가 Bean 사용 범위를 설정할 수 있다

## Bean 태그

|태그|설명|
|---|---|
|class|정규화된 자바 클래스 이름|
|id|bean의 고유 식별자|
|scope|객체의 범위|
|constructor-arg|생성 시 생성자에 전달할 인수|
|property|생성 시 bean setter에 전달할 인수|
|init method|bean 생성 메서드|
|destroy method|bean 종료 메서드|

## Scope 종류

|Scope|설명|
|---|---|
|singleton|해당 Bean에 대해 IoC 컨테이너에서 단 하나의 객체로만 존재한다|
|prototype|해당 Bean에 대해 다수의 객체가 존재할 수 있다|
|request|해당 Bean에 대해 하나의 `HTTP Request`의 라이프사이클에서 단 하나의 객체로만 존재한다|
|session|해당 Bean에 대해 하나의 `HTTP Session`의 라이프사이클에서 단 하나의 객체로만 존재한다|
|global session|해당 Bean에 대해 하나의 `Global HTTP Session`의 라이프사이클에서 단 하나의 객체로만 존재한다.|   

> request, session, global session은 MVC 웹 어플리케이션에서만 사용함

Scope들은 Bean으로 등록하는 클래스에 어노테이션으로 설정해줄 수 있다
```xml
<!-- A simple bean definition -->
<bean id="..." class="..."></bean>

<!-- A bean definition with scope-->
<bean id="..." class="..." scope="singleton"></bean>

<!-- A bean definition with property -->
<bean id="..." class="...">
	<property name="message" value="Hello World!"/>
</bean>

<!-- A bean definition with initialization method -->
<bean id="..." class="..." init-method="..."></bean>
https://gmlwjd9405.github.io/2018/11/10/spring-beans.html
```

https://gmlwjd9405.github.io/2018/11/10/spring-beans.html

## Bean 생명주기
- Bean 생성
- Bean 소멸
- 객체 생성/소멸 메서드 등록

https://espania.tistory.com/383

# MVC Framework
