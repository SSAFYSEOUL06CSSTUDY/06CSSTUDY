### 목차

1. [**Template Syntax**](#1-Template-Syntax)
2. [**Dynamically data binding**](#2-Dynamically-data-binding)


# 1. Template Syntax

DOM을 기본 구성 요소 인스턴스 데이터에 선언적으로 바인딩할 수 있는 HTML 기반 템플릿 구문을 사용

## Template Syntax 종류 

1. Text Interpolation

       <p> Message : {{ msg}} </p> 

* 데이터 바인딩의 가장 기본적인 형태
* 이중 중괄호 구문 (콧수염 구문)을 사용
* 콧수염 구문은 해당 구성 요소 인스턴스의 msg 속성 값으로 대체
* msg 속성이 변경될 때마다 업데이트 된다. 

<br>
2. Raw HTML

       <div v-html="rawHTML"></div>
       const rawHTML = ref('<span style="color:red">This should be red.</span>')

콧수염 구문은 데이터를 일반 텍스트로 해석하기 때문에 실제 HTML을 출력하려면 v-html을 사용해야 한다. 

> 웹사이트에서 임의의 HTML을 동적으로 렌더링하면 XSS 취약점이 쉽게 발생할 수 있다.<br>
> 이는 매우 위험하기 때문에 사용자가 제공한 컨텐츠에서의 사용은 금지한다.

<br>
3. Attribute Bindings

       <div v-bind:id="dynamicId"></div>
       const dynamicId = ref('my-id')

* 콧수염 구문은 HTML 속성 내에서 사용할 수 없기 때문에 v-bind를 사용한다. 
* HTML의 id 속성 값을 vue의 dynamicId 속성과 동기화 되도록 한다.
* 바인딩 값이 null 이나 undefind 인 경우 렌더링 요소가 제거된다.

<br>
4. JavaScript Expressions

       {{ number + 1 }}
       {{ ok ? 'YES' : 'NO’ }}
       {{ message.split('').reverse().join('') }}
       <div :id="`list-${id}`"></div>


* Vue는 모든 데이터 바인딩 내에서 JavaScript 표현식의 모든 기능을 지원
* Vue 템플릿에서 JavaScript 표현식을 사용할 수 있는 위치
  * 콧수염 구문 내부
  * 모든 directive의 속성 값 (v-로 시작하는 특수 속성)
 
<br>

> ! 표현식 주의사항 <br>
> 각 바인딩에는 하나의 단일 표현식만 포함될 수 있다.<br>
> 표현식이 아닌 선언식이나 if 같은 흐름제어도 작동하지 않는다.<br>

<br>
<br>
<br>

## Directive

> v- 접두사가 있는 특수 속성

       <p v-if="seen">Hi There</p>

* Directive의 속성 값은 단일 JavaScript 표현식이어야 함 (v-for, v-on 제외)
* 표현식 값이 변경될 때 DOM에 반응적으로 업데이트를 적용
  ex) v-if 는 seen 표현식 값의 T/F 를 기반으로 요소를 제거 / 삽입

> Directive 전체 구문

![스크린샷 2023-11-06 오후 9 26 00](https://github.com/Youth787/SSAFY_CS_Study/assets/90955152/6d27520e-6028-4a8b-93de-cdabdef38240)


<br>

> Arguments

      <a v-bind:href="myUrl">Link</a>
* 일부 directive 는 directive 뒤에 콜론(:) 으로 표시되는 인자를 사용할 수 있음
* 위 예시의 href는 HTML <a> 요소의 href 속성 값을 myUrl 값에 바인딩 하도록 하는 v-bind 인자
  
      <button v-on:click="doSomething">Button</button>

* 위 예시의 click은 이벤트 수신할 이벤트 이름을 작성하는 v-on 의 인자

<br>

> Modifiers

* . (dot) 로 표시되는 특수 접미사로, directive가 특별한 방식으로 바인딩 되어야 함을 나타냄
* 예를 들어 .prevent는 발생한 이벤트에서 event.preventDefault()를 호출하도록 v-on에 지시하는 modifier

      <form @submit.prevent="onSubmit">...</form>

<br>

> Built-in Directives

![스크린샷 2023-11-06 오후 9 28 00](https://github.com/Youth787/SSAFY_CS_Study/assets/90955152/cd935592-5ef9-45a5-b3bd-1f37cf3e4099)


<br>
<br>
<br>


# Dynamically data binding

### V-bind

하나 이상의 속성 또는 컴포넌트 데이터를 표현식에 동적으로 바인딩

<br>

### 1. Attribute Bindings

HTML 의 속성 값을 Vue의 상태 속성 값과 동기화 되도록 한다.

       <!-- v-bind.html -->
       <img v-bind:src="imageSrc">
       <a v-bind:href="myUrl">Move to url</a>

v-bind shorthand (약어) → : (colon)

       <img :src="imageSrc">
       <a :href="myUrl">Move to url</a>

> Dynamic attribute name (동적 인자 이름)

* 대괄호로 감싸서 directive argument에 JavaScript 표현식을 사용할 수도 있음
* JavaScript 표현식에 따라 동적으로 평가된 값이 최종 argument 값으로 사용됨

       <button :[key]="myValue"></button>

<br>

### 2. Class and Style Bindings

* 클래스와 스타일은 모두 속성이므로 v-bind를 사용하여 다른 속성과 마찬가지로 동적으로 문자열 값을 할당할 수 있음

그러나 단순히 문자열 연결을 사용하여 이러한 값을 생성하는 것은 번거롭고 오류가 발생하기가 쉽다. <br>
Vue는 클래스 및 스타일과 함께 v-bind를 사용할 때 객체 또는 배열을 활용한 개선 사항을 제공한다. <br>

<br>

1) Binding HTML Classes

> Binding to Objects:

  객체를 :class 에 전달하여 클래스를 동적으로 전환할 수 있다. <br>
  예시) isActive 의 T/F에 의해 active 클래스의 존재가 결정된다. <br>

       const isActive = ref(false)
       <div :class="{ active: isActive }">Text</div>

  객체에 더 많은 필드를 포함하여 여러 클래스를 전환할 수 있다.

  예시) :class directive를 일반 클래스 속성과 함께 사용 가능

       const isActive = ref(false)
       const hasInfo = ref(true)
       <div class="static" :class="{ active: isActive, 'text-primary': hasInfo }">Text</div>

  반드시 inline 방식으로 작성하지 않아도 된다.

       const isActive = ref(false)
       const hasInfo = ref(true)
       // ref는 반응 객체의 속성으로 액세스되거나 변경될 때 자동으로 unwrap
       const classObj = ref({
         active: isActive,
         'text-primary': hasInfo
       })
       <div class="static" :class="classObj">Text</div>

> Binding to Arrays

:class를 배열에 바인딩하여 클래스 목록을 적용할 수 있다.

       const activeClass = ref('active')
       const infoClass = ref('text-primary')
       <div :class="[activeClass, infoClass]">Text</div>

배열 구문 내에서 객체 구문 사용

       <div :class="[{ active: isActive }, infoClass]">Text</div>

<br>

### 3. Binding Inline Styles

> Binding to Objects

:style 은 JavaScript 객체 값에 대한 바인딩을 지원 (HTML style 속성에 해당)

       const activeColor = ref('crimson')
       const fontSize = ref(50)
       <div :style="{ color: activeColor, fontSize: fontSize + 'px' }">Text</div>

실제 CSS에서 사용하는 것처럼 :style은 kebab-cased 키 문자열도 지원 (단, camelCase 작성 권장)

       <div :style="{ 'font-size': fontSize + 'px' }">Text</div>
       
템플릿을 더 깔끔하게 작성하려면 스타일 객체에 직접 바인딩하는 것을 권장

       const styleObj = ref({
         color: activeColor,
         fontSize: fontSize.value + 'px'
       })
       <div :style="styleObj">Text</div>

> Binding to Arrays

여러 스타일 객체의 배열에 :style을 바인딩할 수 있다. <br>
작성한객체는 병합되어 동일한 요소에 적용한다.

       const styleObj2 = ref({
         color: 'blue',
         border: '1px solid black'
       })
       <div :style="[styleObj, styleObj2]">Text</div>




