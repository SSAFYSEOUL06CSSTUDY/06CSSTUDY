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
### Bean 생성
- Singleton인 경우, xml 로딩 시 객체가 생성된다
- Singleton 이고, lazy-init 속성이 true 일 경우, getBean 메서드를 호출할 때, 객체가 생성된다.
- prototype 일 경우, getBean 메서드를 호출 할 때, 객체가 생성된다.
### Bean 소멸
- IoC 컨테이너가 종료 될 때, 객체가 소멸된다. (ctx.close)
### 객체 생성/소멸 메서드 등록
|메서드|설명|
|---|---|
|init-method|생성자 호출 이후 자동으로 호출된다. (Beans 객체 내부에서 정의)|
|destroy-method|객체가 소멸될 때 자동으로 호출된다. (Beans 객체 내부에서 정의)|
|default-init-method|init-method 를 설정하지 않은 경우 자동으로 호출된다. (xml 파일에서 설정)|
|default-destroy-method|destroy-method 를 설정하지 않은 경우 자동으로 호출된다. (xml 파일에서 설정)|

### Bean 객체
```xml
public class BeanTest {
	public BeanTest() {
		System.out.println("BeanTest의 생성자");
	}
	
	public void default_init() {
		System.out.println("BeanTest의 default_init");
	}

	public void default_destroy() {
		System.out.println("BeanTest의 default_destroy");
	}
	
	public void bean_init() {
		System.out.println("BeanTest의 init 메서드");
	}
	
	public void bean_destroy() {
		System.out.println("BeanTest의 destroy 메서드");
	}
}
```
### Beans.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>

<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
	http://www.springframework.org/schema/beans/spring-beans.xsd"
	default-init-method="default_init" default-destroy-method="default_destroy">
	<!-- default-init-method , default-destroy-method 설정 -->
    
	<bean id ="bean1" class="kr.ti.espania.beans.BeanTest" lazy-init="true" init-method="bean_init" destroy-method="bean_destroy"/>		
</beans>
```
https://espania.tistory.com/383
