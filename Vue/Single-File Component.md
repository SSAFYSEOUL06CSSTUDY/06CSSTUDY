### 목차

1. [**컴포넌트(SFC / String)**](#1-컴포넌트(SFC-/-String))
2. [**컴포넌트 등록**](#2-컴포넌트-등록)
3. [**컴포넌트 사용**](#3-컴포넌트-사용)


## 📌 1. 컴포넌트(SFC / String)

### 1. Single File Component(SFC)

> 
> 
> 
> **기본적으로 Single-File Component는 화면의 특정 영역에 대한 HTML, CSS, JS 코드를 한 파일에서 관리하는 방법이다.**
> 

Vue에서 Single-file component는 **템플릿(template), 로직(script), 스타일(style)을 하나의 파일로 캡슐화한 특수 파일 형식**(*.vue)을 말한다.

빌드도구를 사용할 때 컴포넌트는 일반적으로 SFC로 정의할 수 있다.

```
<template>
	<p>템플릿 영역입니다.</p>
</template>

<script>
	<p>로직 영역입니다.</p>
</script>

<style>
	<p>스타일 영역입니다.</p>
</style>
```

**1) `<template>` 영역**

- 각 *.vue 파일은 한 번에 최대 하나의 top-level `<template>` 블록을 포함할 수 있다.
- 콘텐츠는 추출되어 @vue/compiler-dom으로 전달되고, JavaScript 렌더 기능으로 사전 컴파일되고, render 옵션으로 내보내어 컴포넌트에 첨부된다.

**2) `<script>` 영역**

- 각 *.vue 파일은 한 번에 최대 하나의 `<script>` 블록을 포함할 수 있다. (`<script setup>` 제외. `<script>`와 `<script setup>`은 공존할 수 있다.)
- 스크립트는 ES모듈로 실행된다.
- default export는 일반 객체 또는 defineComponent의 반환 값으로 Vue 컴포넌트 옵션 객체여야 한다.

**3) `<style>` 영역**

- 단일 *.vue 파일에는 여러 `<style>` 태그가 포함될 수 있다.
- `<style>` 태그는 현재 컴포넌트에 스타일을 캡슐화하는 데 도움이 되도록 scoped 또는 module 속성을 가질 수 있다.

SFC말고 다른 방법도 있을까? 문자열 템플릿(string template)이 있다.

### 2. 문자열 템플릿(string template)

> 
> 
> 
> **빌드 도구를 사용하지 않을 때 컴포넌트는 Vue 옵션을 포함하는 일반 JavaScript 객체로 정의할 수 있다.**
> 

```
import { ref } from 'vue/dist/vue.esm-bundler.js';
export default {
  setup() {
    const counter = ref(0);
    return {
      counter,
    };
  },
  template: `
	  <button @click="counter++">클릭 횟수 {{ counter }}</button>
  `,
};
```

💡 ***vue.esm-bundler.js 이란?***

런타임 컴파일러를 포함한다. 빌드도구(Vite)를 사용하지만 여전히 런타임 문자열 템플릿을 원하는 경우 vue.esm-bundler.js를 사용한다.

## 📌 2. 컴포넌트 등록

> 
> 
> 
> Vue 컴포넌트는 `<template>`안에서 발견 되었을 때 Vue가 구현 위치를 알 수 있도록 등록을 해놓아야 한다. **컴포넌트를 등록하는 방법에는 전역(Global), 지역(Local) 두 가지가 있다.**
> 

### 1) 전역 등록

**전역 등록된 컴포넌트는 애플리케이션 어떤 곳에서든 사용 가능하다.**

`app.component()`  메서드를 사용하여 현재 Vue 애플리케이션에서 전역적으로 사용할 수 있도록 등록할 수 있다.

```
// main.js파일
import { createApp } from 'vue';
import App from './App.vue';

import AppCard from './components/AppCard.vue';

const app = createApp(App)
app.component('AppCard', AppCard)
app.mount('#app');
```

이렇게 등록해놓으면 어떤 곳에서든 사용 가능하다.

```
// TheView.vue 파일

<template>
 	<AppCard></AppCard>
</template>

```

**전역등록 단점**

- 컴포넌트를 전역 등록하면 컴포넌트를 사용하지 않더라도 계속해서 최종 빌드에 해당 컴포넌트가 포함된다. 이는 사용자가 다운로드하는 파일의 크기를 불필요하게 증가시킨다.
- 전역등록을 하게 되면 애플리케이션의 컴포넌트간 종속 관계를 명시적을 확인하기 힘들다. 상위 컴포넌트, 하위 컴포넌트의 구분이 힘들면 유지보수가 어렵게 된다.

### 2) 지역등록

**지역 등록된 컴포넌트는 현재 컴포넌트 영역에서만 사용할 수 있다.**

Vue 컴포넌트 인스턴스의 `components` 옵션을 사용해서 등록할 수 있다.

```
// ParentComponent.vue 파일
import ChildComponent from './ChildComponent.vue'

export default {
	components: {
		ChildComponent
	},
	setup() {
		// ...
	}
}
```

ParentComponent 컴포넌트에 로컬 등록된 ChildComponent는 현재 컴포넌트인 ParentComponent 컴포넌트에서만 사용 가능하다.

## 📌 3. 컴포넌트 사용

등록된 컴포넌트는 `<template>`에서 원하는 만큼 사용할 수 있다.

```
<h2>Single-File Component</h2>
<ButtonCounter></ButtonCounter>
<ButtonCounter></ButtonCounter>
```

컴포넌트는 사용할 때마다 해당 컴포넌트의 새 인스턴스가 생성된다.

즉, 사용할 때마다 setup() 함수 가 실행 된다는 것을 의미한다.

## 📌 4. 참고 자료

[Vue공식문서](https://vuejs.org/api/sfc-spec.html)

Vue3 완벽마스터 - 짐코딩

[Song's DLog](https://velog.io/@falling_star3/Vue.js-Single-File-ComponentSFC)
