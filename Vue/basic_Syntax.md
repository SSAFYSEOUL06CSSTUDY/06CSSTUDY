### 목차

1. [**Template Syntax**](#1-Template-Syntax)
2. [**Dynamically data binding**](#2-Dynamically-data-binding)
3. [**Event Handling**](#3-Event-Handling)
4. [**Form input Bindings**](#4-Form-input-Bindings)



# 1. Template Syntax

DOM을 기본 구성 요소 인스턴스 데이터에 선언적으로 바인딩할 수 있는 HTML 기반 템플릿 구문을 사용

### Template Syntax 종류 
1. Text Interpolation

       <p> Message : {{ msg}} </p> 

* 데이터 바인딩의 가장 기본적인 형태
* 이중 중괄호 구문 (콧수염 구문)을 사용
* 콧수염 구문은 해당 구성 요소 인스턴스의 msg 속성 값으로 대체
* msg 속성이 변경될 때마다 업데이트 된다. 

2. Raw HTML

       <div v-html="rawHTML"></div>
       const rawHTML = ref('<span style="color:red">This should be red.</span>')

콧수염 구문은 데이터를 일반 텍스트로 해석하기 때문에 실제 HTML을 출력하려면 v-html을 사용해야 한다. 

> 웹사이트에서 임의의 HTML을 동적으로 렌더링하면 XSS 취약점이 쉽게 발생할 수 있다.<br>
> 이는 매우 위험하기 때문에 사용자가 제공한 컨텐츠에서의 사용은 금지한다.

3. Attribute Bindings

       <div v-bind:id="dynamicId"></div>
       const dynamicId = ref('my-id')

* 콧수염 구문은 HTML 속성 내에서 사용할 수 없기 때문에 v-bind를 사용한다. 
* HTML의 id 속성 값을 vue의 dynamicId 속성과 동기화 되도록 한다.
* 바인딩 값이 null 이나 undefind 인 경우 렌더링 요소가 제거된다.

4. JavaScript Expressions

       {{ number + 1 }}
       {{ ok ? 'YES' : 'NO’ }}
       {{ message.split('').reverse().join('') }}
       <div :id="`list-${id}`"></div>

* Vue는 모든 데이터 바인딩 내에서 JavaScript 표현식의 모든 기능을 지원
* Vue 템플릿에서 JavaScript 표현식을 사용할 수 있는 위치
  * 콧수염 구문 내부
  * 모든 directive의 속성 값 (v-로 시작하는 특수 속성)
 

> ! 표현식 주의사항 <br>
> 각 바인딩에는 하나의 단일 표현식만 포함될 수 있다.<br>
> 표현식이 아닌 선언식이나 if 같은 흐름제어도 작동하지 않는다.<br>

<br>
<br>

### Directive

> v- 접두사가 있는 특수 속성

       <p v-if="seen">Hi There</p>

* Directive의 속성 값은 단일 JavaScript 표현식이어야 함 (v-for, v-on 제외)
* 표현식 값이 변경될 때 DOM에 반응적으로 업데이트를 적용
  ex) v-if 는 seen 표현식 값의 T/F 를 기반으로 요소를 제거 / 삽입

> Directive 전체 구문

![스크린샷 2023-11-06 오후 9 26 00](https://github.com/Youth787/SSAFY_CS_Study/assets/90955152/6d27520e-6028-4a8b-93de-cdabdef38240)


> Arguments

      <a v-bind:href="myUrl">Link</a>

* 일부 directive 는 directive 뒤에 콜론(:) 으로 표시되는 인자를 사용할 수 있음
* 위 예시의 href는 HTML <a> 요소의 href 속성 값을 myUrl 값에 바인딩 하도록 하는 v-bind 인자

      <button v-on:click="doSomething">Button</button>

* 위 예시의 click은 이벤트 수신할 이벤트 이름을 작성하는 v-on 의 인자

> Modifiers

* . (dot) 로 표시되는 특수 접미사로, directive가 특별한 방식으로 바인딩 되어야 함을 나타냄
* 예를 들어 .prevent는 발생한 이벤트에서 event.preventDefault()를 호출하도록 v-on에 지시하는 modifier

      <form @submit.prevent="onSubmit">...</form>

> Built-in Directives

![스크린샷 2023-11-06 오후 9 28 00](https://github.com/Youth787/SSAFY_CS_Study/assets/90955152/cd935592-5ef9-45a5-b3bd-1f37cf3e4099)








