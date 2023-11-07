## 📌 1. computed

### 1-1. computed란?

> 
> 
> 
> **템플릿의 데이터 표현을 더 직관적이고 간결하게 도와주는 속성.**
> 

템플릿 안에 다음과 같은 코드가 있다고 가정해보자.

```
<p>강의가 존재 합니까?:</p>
<span>{{ teacher.lectures.length > 0 ? 'Yes' : 'No' }}</span>
```

위와 같이 템플릿 표현식 내 코드가 길어질 경우, 가독성이 떨어지고 유지보수가 어려워질 수 있다.

이러한 코드를 여러곳에서 반복적으로 사용해야 한다면 매우 비효율적일 것이다.

이럴때 사용하는 것이 **계산된 속성(computed property)** 이다.

위의 코드에서 computed를 사용해보자.

```
<p>강의가 존재 합니까?:</p>
<span>{{ hasLecture }}</span>

```

```
const hasLecture = computed(() => {
  return teacher.lectures.length > 0 ? 'Yes' : 'No'
})
```

template 표현식 내 코드가 더욱 깔끔하고 명확하게 되었다.

또한, computed로 인해 다른 곳에서도 자유롭게 사용할 수 있게 되었다.

이처럼 computed를 사용하는 이유는 가독성과 재사용성을 위해서이다.

### 1-2. computed 속성의 장점

> 
> 
> 
> **1) computed 속성의 대상으로 정한 data 속성이 변했을 때 이를 감지하고 자동으로 다시 연산해준다. 2) 코드의 가독성과 재사용성**
> 

computed는 가독성과 재사용성을 향상 시킬뿐만 아니라, 데이터의 변화를 감지하고 자동으로 다시 연산해주는 장점을 가지고 있다.

위의 예시 코드에서, `teacher.lectures.length` 데이터가 변경된다면, 자동으로 다시 연산하여 화면을 갱신해 줄 것이다.

### 1-3. computed 주의사항

> 
> 
> 
> **1) computed 속성은 인자를 받지 않는다. 2) HTTP 통신과 같이 컴퓨팅 리소스가 많이 필요한 로직을 정의하지 않는다.**
> 

computed속성은 인자를 받지 않는다.

즉, 다음과 같은 코드는 동작하지 않는다.

```
const hasLecture = computed((teacher.lectures.length ? ) => {
  return  teacher.lectures.length > 0 ? 'Yes' : 'No'
})
```

또한, http통신 코드를 제외한다.

computed 속성은 기본적으로 템플릿 코드의 가독성을 높이기 위한 기능이다.

axios같은 코드는 methods나 watch에 넣는 것이 적합하다.

## 📌 2. computed, methods, watch

### 2-1. computed VS methods

computed속성 대신 메소드와 같은 함수를 정의할 수도 있다.

```
const hasLecture = computed(() => {
  return teacher.lectures.length > 0 ? 'Yes' : 'No'
})

function existLecture() {
  return teacher.lectures.length > 0 ? 'Yes' : 'No'
}
```

최종 결과는 동일하다.

그럼 computed와 methods의 차이는 무엇일까?

바로,  **computed 속성은 종속 대상을 따라 저장(캐싱)된다는 것이다.**

computed 속성은 해당 속성이 종속된 대상이 변경될 때만 함수를 실행한다. 즉 데이터가 변경되지 않는 한, computed 속성인 데이터를 여러 번 요청해도 다시 계산하지 않고 이미 계산되어 있던 결과를 즉시 반환한다. 렌더링시 비용이 적게 드는 셈이다.

이에 비해 메소드를 호출하면 렌더링을 다시 할 때마다 항상 함수를 실행한다.

> 
> 
> 
> **1) computed는 캐싱된다. 2) computed는 캐싱으로 인해 렌더링시 비용이 적게든다. 3) method는 파라미터가 올 수 있다.**
> 

### 2-2. computed VS watch

Vue는 Vue 인스턴스의 데이터 변경을 관찰하고 이에 반응하는 보다 일반적인 **watch** 속성을 제공한다.

computed와 watch 모두 데이터의 변화를 감지한다.

그럼 computed와 watch의 차이점은 무엇일까?

바로,  **computed는 선언형 프로그래밍이고, watch는 명령형 프로그래밍이라는 차이가 있다.**

computed 속성은 계산해야 하는 목표 데이터를 정의하는 방식으로 선언형 프로그래밍이고, watch는 감시할 데이터를 지정하고, 그 데이터가 바뀌면 선언한 함수를 실행하라는 방식으로 명령형 프로그래밍이다.

일반적으로 선언형 프로그래밍이 명령형 프로그래밍보다 코드 반복이 적은 등 우수하다고 평가하는 경향이 있다.

그러므로, 복잡한 로직이나 무거운 처리를 해야하는 경우 watch를 사용하고, 연산작업은 computed를 사용하는 것이 좋다.

다음 코드에서 computed(선언형 프로그래밍)와 watch(명령형 프로그래밍)의 차이를 볼 수 있다.

```
const fullName = computed(() => {
      return firstName.value + ' ' + lastName.value
  }
```

```
watch(firstName, (cur, prev) => {
  fullName.value = cur + ' ' + lastName.value;
})

watch(lastName, (cur, prev) => {
  fullName.value = firstName.value +' ' + cur;
})
```

---
참고 : https://velog.io/@falling_star3/Vue.js-computed-%EC%86%8D%EC%84%B1



