### 목차

1. [**Props 기본 개념**](#1-Template-Syntax)
2. [**Props 상속 과정 (부모 ⇒ 자식)**](#2-props-상속-과정-부모-⇒-자식)
3. [**Props 수정 과정 (자식 ⇒ 부모)**](#3-props-수정-과정-자식-⇒-부모)

# 1. Props 기본 개념

우리가 SFC(Single-File Components : 싱글 파일 컴포넌츠)를 사용하면서 동일한 데이터를

서로 다른 컴포넌트에서 반복적으로 사용하는 일이 빈번하게 발생

예를 들어 로그인 후 “유저 이름”은 다양한 컴포넌트에서 사용됩니다.

이를 관리하기 위해 각각의 컴포넌트에서 공통되는 “(공통)부모 컴포넌트”에서 해당 값을 관리하고 각각의 컴포넌트에 값을 내려줍니다.

### 그런데 만약 A→B→C 로 값이 상속되는 중에 B에서는 해당 값을 사용하지 않지만 계속해서 값을 상속해 줘야 하는 경우, A→B→C→D→……→Z로 값을 매우 깊게 상속해주는 경우 등에서는 이런 방식은 매우 불편하다.

> 이를 해결해주는 것이 Vue3의 공식 상태 관리 라이브러리 Pinia(https://pinia.vuejs.kr/)

> 이전 버전에서 사용하던 Vuex는 여러 문제가 생겨서 Pinia로 대체되는 추세(마이그레이션해서 넘어가는 중)

> 외부 데이터 저장소(상태값 저장소)를 만들어서 관리하는 개념

---

기본 흐름 파악

- 부모 → 자식으로 data를 props로 상속
- 자식 컴포넌트에서 값 변경시
  자식 → 부모로 해당 사실을 $emit으로 알려줍니다.
  ※ 자식이 직접 값을 변경하지 X, 값은 부모가 변경

---

데이터는 무조건 단 방향으로만 관리

why? 양방향으로 관리하면 사고 발생

Data의 관리는 항상 부모가

자식 컴포넌트는 부모에게 값을 어떻게 바꾸고 싶다는 것 만을 알려줍니다.(emit)

자식이 직접 값을 관리하기 시작하면 각각의 경우마다 값이 달라지는 문제가 생깁니다.

그래서 여기서 단방향은 부모→ 자식 으로의 단방향을 말합니다.데이터는 항상 부모가 자식에게 내려주는 것

# one-way-down binding 하향식 단방향 바인딩

모든 props는 부모 → 자식 속성으로 하향식 단방향 바인딩을 형성

다시, props의 성질을 정리하면

- 부모→자식 O / 부모 컴포넌트 업데이트시 자식 컴포넌트의 모든 props가 최신으로 업데이트
- 자식→부모 X

# 2. Props 상속 과정 (부모 ⇒ 자식)

## 자식 컴포넌트로 데이터 props

```jsx
<자식 컴포넌트 my-msg="myMsg"/> /Static Props
```

왼쪽에는 props이름

오른쪽에는 문자열로 내려줄 props 값을 할당

그렇다면 props 값에 별도로 선언한 변수를 할당하고 싶다면 어떻게 하면 될까요?(Dynamic Props)

```jsx
<template>
	<자식 컴포넌트 :my-msg="myMsg"/>
	=>	<자식 컴포넌트 v-bind:my-msg="myMsg"/>
</template>
<script>
	const myMsg = ref("메세지 props");
</script>
```

---

## 자식 컴포넌트에서 데이터 받기

### 자식 컴포넌트에서는 defineProps를 통해 값을 받아줍니다.

※ 이렇게 받은 데이터는 다시 props 하는 것도 가능합니다.

이 때 두 가지 선언 방식을 사용합니다.

- 배열 선언 - 가장 기본적인 방식, 상속받은 props를 나열해주면 됩니다.

```jsx
const props = defineProps([myMsg, dynamicProps]);
```

- 객체 선언 - Vue3 스타일 가이드에서 권장되는 방식

```jsx
const props = defineProps({
  myMsg: String,
  dynamicProps: String,
});
```

⇒ 배열 방식과는 다르게 해당 props 변수를 key, 변수의 타입을 value한 객체 값으로 데이터를 관리합니다.

### template에 props 변수 할당

```jsx
변수 할당은 정말 간단합니다.
<template>
	<p>{{ myMsg }}</p>
</template>
<script>
	const props = defineProps({
	  myMsg: String,
	});
</script>
```

---

## props 변수명 설정시 주의사항

```jsx
1. <자식 컴포넌트 my-msg="myMsg"/> /kebab-case
2. <자식 컴포넌트 myMsg="myMsg"/>/camelCase
```

1번, 2번 둘 다 가능하지만 1번이 Vue3 코드 스타일로 권장

```jsx
1. const props = defineProps({
	  myMsg: String,
	});
2. const props = defineProps({
	  my-msg: String,
	});
```

내려줄때와는 다르게 kebab-case로 선언시 작동X

자식 컴포넌트에서 선언-할당시 무조건 camelCase로 변수명 선언

---

# 3. Props 수정 과정 (자식 ⇒ 부모)

## $emit()

```jsx
자식 컴포넌트가 이벤트를 발생시켜 부모 컴포넌트로 데이터를 전달하는 역할의 메서드
```

말이 어렵습니다. \*\*\*\*

### **그냥 자식노드에서 부모노드에서 선언된 값 바꾸고 싶을때 사용하는 메서드**

코드를 통해 먼저 기본 구조를 알아 보겠습니다.

```jsx
[ emit 이벤트를 생성하는 자식 컴포넌트 ]
<template>
	<button @click="$emit('increaseBy', 1)"> 누르면 1증가</button>
</template>

[ 해당emit를 받아주는 부모 컴포넌트 ]
방법 1.
<template>
	<MyButton @increase-by="(n) => count += n" />
</template>
<script>
	export default {
	  setup() {
	    const count = ref(0);
	    return {
	      count
	    };
	  }
	}
</script>
방법 2.
<template>
	<MyButton @increase-by="increaseCount" />
</template>
<script>
	export default {
	  setup() {
	    const count = ref(0);
			function increaseCount(n) {
			  count.value += n
			}
	    return {
	      count, increaseCount
	    };
	  }
	}
</script>
```

### $emit 이벤트는 자식에서 이벤트 생성, 부모에서 해당 이벤트 수신의 과정으로 작동합니다.

---

$emit 이벤트의 기본 구조를 보면 부모 컴포넌트에서는 함수 처리가 간단한데 반해, 자식 컴포넌트에서의 기능 구현이 어렵습니다.

⇒ Script 에서는 Template 에서 직접 선언한 함수에 접근이 불가능 하기 때문에

### defineEmits()을 사용해 복잡한 함수를 Script에 선언하고 Template에 할당해 줍니다.

```jsx
<template>
  <button @click="buttonClick">누르면 1증가</button>
</template>

<script setup>
import { defineEmits } from 'vue' /안해줘도 됩니다.
const emit = defineEmits(['increaseBy'])
// 버튼 클릭 이벤트 핸들러
const buttonClick = () => {
  emit('increaseBy', 1)
}
</script>
```

**$emit은 $emit(이벤트, 기타 같이 넘겨주는 값들) 구조로 이루어져 있습니다.**

이를 코드로 보면

```jsx
[ $emit을 보내는 자식 컴포넌트 ]
<template>
  <button @click="여러인자보내기">보내기</button>
</template>

<script setup>
import { defineEmits } from 'vue' / 없어도 됩니다.
const emit = defineEmits(['update'])
const 여러인자보내기 = () => {
  emit('update', '값1', '값2', 3)
}
</script>
[ 이벤트와 인자들을 받는 부모 컴포넌트 ]
<template>
  <자식컴포넌트 @update="여러인자인자받기" />
</template>

<script setup>
const handleUpdate = (arg1, arg2, arg3) => {
  console.log('Argument 1:', arg1)
  console.log('Argument 2:', arg2)
  console.log('Argument 3:', arg3)
}
</script>
```
