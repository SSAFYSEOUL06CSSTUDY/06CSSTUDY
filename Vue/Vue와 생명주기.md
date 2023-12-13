# Vue
- 사용자 인터페이스를 구축하기 위한 JavaScript 프레임워크
- 표준 HTML, CSS 및 JavaScript를 기반으로 구축
- 단순한 것 부터 복잡한 것 까지 사용자 인터페이스를 효율적으로 개발할 수 있는 컴포넌트 기반 프로그래밍 모델을 제공
  
# Vue를 학습하는 이유
1. 쉬운 학습 곡선 및 간편한 문법
   - 새로운 개발자들도 빠르게 학습할 수 있음
2. 반응성 시스템
   - 데이터 변경에 따라 자동으로 화면이 업데이트됨
3. 모듈화 및 유연한 구조
   - 어플리케이션을 컴포넌트 조각으로 나눌 수 있다
   - 코드를 재사용성을 높이고 유지보수를 용이하게 한다

# Vue의 2가지 핵심 기능
1. 선언적 렌더링(Declarative Rendering)
   - HTML을 확장하는 템플릿 구문을 사용하여 HTML이 JavaScript 데이터를 기반으로 어떻게 보이는지 설명할 수 있음
2. 반응형(Reactivity)
   - JavaScript 상태 변경사항을 자동으로 추적하고 변경사항이 발생할 때 DOM을 효율적으로 업데이트

# Vue를 사용하는 방법
1. CDN 방식
- HTML 파일 안에서 url 경로를 포함시켜 CDN으로부터 Vue 라이브러리를 불러오는 방식
```
최신버전 불러오기
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>

특정버전 불러오기
<script src="https://cdn.jsdelivr.net/npm/vue@2.6.0"></script>
```
2. NPM 방식
- npm으로 Vue 라이브러리를 불러오는 방식
```
$ npm init
$ npm install vue
```

# API 스타일
1. 옵션(Options) API
```vue
<script>
export default {
  // data()에서 반환된 속성들은 반응적인 상태가 되어 `this`에 노출됩니다.
  data() {
    return {
      count: 0
    }
  },

  // methods는 속성 값을 변경하고 업데이트 할 수 있는 함수.
  // 템플릿 내에서 이벤트 헨들러로 바인딩 될 수 있음.
  methods: {
    increment() {
      this.count++
    }
  },

  // 생명주기 훅(Lifecycle hooks)은 컴포넌트 생명주기의 여러 단계에서 호출됩니다.
  // 이 함수는 컴포넌트가 마운트 된 후 호출됩니다.
  mounted() {
    console.log(`숫자 세기의 초기값은 ${ this.count } 입니다.`)
  }
}
</script>

<template>
  <button @click="increment">숫자 세기: {{ count }}</button>
</template>
```


2. 컴포지션(Composition) API
```vue
<script setup>
import { ref, onMounted } from 'vue'

// 반응적인 상태의 속성
const count = ref(0)

// 속성 값을 변경하고 업데이트 할 수 있는 함수.
function increment() {
  count.value++
}

// 생명 주기 훅
onMounted(() => {
  console.log(`숫자 세기의 초기값은 ${ count.value } 입니다.`)
})
</script>

<template>
  <button @click="increment">숫자 세기: {{ count }}</button>
</template>
```

# Vue.js의 라이프사이클
<img href="lifecycle P7awcnoo" src="https://github.com/SSAFYSEOUL06CSSTUDY/06CSSTUDY/assets/50236187/cfa9a227-0589-49af-ba0f-8593084306f6" height=1000>

## onMounted()
- 컴포넌트가 마운트된 후 호출될 콜백 등록
- 동기식 자식 컴포넌트가 모두 마운트됨
- 자체 DOM 트리가 생성되어 상위 컨테이너에 삽입됨
```vue
<script setup>
import { ref, onMounted } from 'vue'

const el = ref()

onMounted(() => {
  el.value // <div>
})
</script>

<template>
  <div ref="el"></div>
</template>
```

## onUpdated()
- 반응형 상태 변경으로 컴포넌트의 DOM 트리가 업데이트된 후 호출될 콜백을 등록
- 부모 컴포넌트의 updated 훅은 자식 컴포넌트의 훅 이후에 호출
- 컴포넌트의 DOM 업데이트 후에 호출됨
- 특정 상태 변경 이후에 업데이트된 DOM에 접근해야 한다면, 대신에 nextTick()을 사용
```vue
<script setup>
import { ref, onUpdated } from 'vue'

const count = ref(0)

onUpdated(() => {
  // 텍스트 내용은 현재 `count.value`와 같아야 함
  console.log(document.getElementById('count').textContent)
})
</script>

<template>
  <button id="count" @click="count++">{{ count }}</button>
</template>
```

## onUnmounted()
- 컴포넌트가 마운트 해제된 후 호출될 콜백을 등록
### 컴포넌트가 마운트 해제되었다고 간주하는 조건
1. 모든 자식 컴포넌트가 마운트 해제됨
2. 관련된 모든 반응형 이펙트(setup()에서 생성된 렌더 이펙트, 계산된 속성, 감시자)가 중지됨.
```vue
<script setup>
import { onMounted, onUnmounted } from 'vue'

let intervalId
onMounted(() => {
  intervalId = setInterval(() => {
    // ...
  })
})

onUnmounted(() => clearInterval(intervalId))
</script>
```

더 자세한 설명은 공식 가이드 참고하세요.
