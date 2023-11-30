## 📌 1. Composition API

> 
> 
> 
> **코드의 가독성을 높이고, 컴포넌트의 로직을 유연하게 구성할 수 있도록 하는 함수 기반의 API. Vue3 공식문서에서 Composition API를 쓸 것을 권장하고 있다.**
> 

### 1.1 Composition API란?

Composition API는 Vue3에서 새로 추가된 함수 기반의 API로, 컴포넌트 로직을 유연하게 구성할 수 있도록 하여 코드의 재사용성과 가독성을 높여준다.

기존의 Vue는 프로젝트 규모가 커질수록 컴포넌트의 계층 구조가 복잡해지며, 이로인해 추적 및 관리가 어려웠다고 한다. 이러한 Vue의 단점을 커버하기 위해 Vue3부터 컴포지션 API가 추가되었다.

### 1.2 Composition API 살펴보기

> 
> 
> 
> Composition API는 옵션을 선언하는 대신 import한 함수를 사용하여 vue 컴포넌트를 작성할 수 있는 API 세트이다.
> 
- 
    
    **반응형(Reactivity) API**
    
    : ref() 및 reactive()를 사용하여 반응형 상태, 계산된 상태 및 감시자를 직접 생성할 수 있다.
    
- 
    
    **생명주기 훅**
    
    : onMounted() 및 onUnmounted()를 사용하여 컴포넌트 생명주기에 프로그래밍 방식으로 연결할 수 있다.
    
- 
    
    **의존성 주입(Dependency Injection)**
    
    : provide() 및 inject()를 사용하면 반응형 API를 사용하는 동안 Vue의 의존성 주입 시스템을 활용할 수 있다.
    

### Composition API 예시

```
<template>
  <button @click="plus()">숫자: {{ cnt }}</button>
</template>

<script setup>
import { ref, onMounted } from 'vue'

// 반응형
const cnt = ref(0)
const plus = () => { cnt.value++ }

// 생명주기 훅
onMounted(() => { console.log(`OnMounted`) })
</script>
```

Options API 기존에는 data, methods, computed에 각각 로직이 흩어져 있었는데 Composition API를 활용하면 로직이 모아지는 것을 알 수 있다.

이 부분은 각각 장단점이 존재한다. 기존의 Options API를 주로 사용하던 사람은 해당 구조에 익숙해져 있기 때문에 로직이 나뉘어져 있는 것이 편할 것이고, 로직을 모아서 사용하던 사람은 Composition API가 편할 수 있다.

### 1.3 Composition API 장점

**1. 더 나은 로직 재사용성**

컴포지션 API의 가장 큰 장점은 *컴포저블 함수의 형태로 깔끔하고 효율적인 로직 재사용이 가능하다는 것입니다. 옵션 API의 기본 로직 재사용 메커니즘인 **믹스인의 모든 단점을 해결한다.

컴포지션 API의 로직 재사용 기능은 컴포저블 유틸리티의 계속 성장하는 컬렉션인 VueUse와 같은 인상적인 커뮤니티 프로젝트를 탄생시켰다. 이는 믹스인의 불분명한 출처의 속성, 네임스페이스 충돌 등의 단점을 해결한다.

***💡 *컴포저블이란?**
 Vue 앱의 컨텍스트에서 컴포저블은 Vue 컴포지션 API를 활용하여 상태 저장 로직를 캡슐화하고 재사용하는 함수이다. 여러 컴포넌트에서 동일한 로직을 재사용하기 위해 컴포저블 함수로 재구성 후 외부 파일로 추출할 수 있다.*

* **💡 *믹스인이란?**
 Vue2에서 컴포넌트에 재사용 가능한 기능을 배포하는 유연한 방법. mixins 옵션은 믹스인 객체로 이루어진 배열이다. Vue3에서 믹스인은 계속 지원되지만, 더 이상 권장되지 않는 방법이다.*

```
var HelloMixins = {
  // 컴포넌트 옵션 (data, methods, created 등)
};

new Vue({
  mixins: [HelloMixins]
})
```

**2. 보다 유연한 코드 구성**

Options API는 data, methods, computed로 로직이 흩어지게 된다. 이는 간단한 코드에서는 구조적으로 나뉘어진다는 장점이 있겠지만, 코드가 복잡해질수록 가독성을 떨어뜨린다.

<img width="752" alt="스크린샷 2023-11-07 오전 11 48 22" src="https://github.com/Youth787/SSAFY_CS_Study/assets/90955152/3bf0b2cc-04fb-43aa-a3af-3bac039bc9c7">

<출처: https://ko.vuejs.org/ >

좌측은 Options API이며, 우측은 Composition API로 리팩토링 했다.

이 후, 동일한 논리적 문제를 기준으로 같은 색상을 칠해보았다.

같은 문제를 처리하는 코드들이 여러 부분에 분할되어 있는 것을 알 수 있다.

이는 코드가 길어질수록, 단일 논리를 이해하고 탐색하기 위해 지속적으로 위아래로 스크롤하는 불편함을 야기한다. 또한, 이를 재사용 가능한 유틸리티로 추출하려는 경우 코드의 조각을 찾는데 상당한 노력이 필요하다.

반대로, 우측의 Composition API는 코드들이 흩어져있지 않기 때문에 다른 블록들로 이동하며 코드 조각을 찾을 필요가 없다. 최소한의 노력으로 재사용을 위한 추출도 가능하다.

리팩토링을 위한 소모 시간 감소는 대규모 코드베이스에 장기적인 유지 관리의 핵심이다.

**3. 더 나은 타입 추론**

현재 많은 프론트엔드 개발자들이 TypeScript를 채택하고 있다. Composition API는 기본적으로 유형 친화적인 일반 변수와 함수를 주로 사용한다. 대부분의 경우, Composition API 코드는 TypeScript와 일반 JavaScript에서 거의 동일하게 보인다. 이것은 JavaScript 사용자가 유형 추론의 이점을 누릴 수 있도록 한다.

**4. 더 작은 프로덕션 번들 및 더 적은 오버헤드**

컴포지션 API와 `<script setup>`으로 작성된 코드는 옵션 API에 비해 더 효율적이고 축소하기 쉽다. 이는 `<script setup>` 컴포넌트의 템플릿이 `<script setup>` 코드의 동일한 범위에 인라인된 함수로 컴파일되기 때문이다. this의 속성 접근과 달리 컴파일된 템플릿 코드는 인스턴스 프록시 없이 `<script setup>` 내부에 선언된 변수에 직접 접근할 수 있다. 이것은 모든 변수 이름을 안전하게 단축할 수 있기 때문에 더 나은 축소로 이어진다.
