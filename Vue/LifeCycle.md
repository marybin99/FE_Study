# Vue.js LifeCycle
Vue에서 인스턴스나 컴포넌트를 생성할 때 생기는 과정
> Creation, Mounting, Updating, Destruction

![lifecycle](https://github.com/marybin99/CS/assets/110241993/dcd4d908-bf6d-4548-a485-3d976fb48f61)

## Creation

**컴포넌트 초기화 단계**   

이 단계에서 실행되는 훅들이 라이프사이클 중 가장 먼저 실행   

아직 컴포넌트가 DOM에 추가되기 전, 서버 렌더링에서도 지원되는 훅   
→ 클라이언트와 서버 렌더링 모두에서 처리해야 할 일이 있으면, 이 단계에 적용  

`this.$el` 사용 불가

### beforeCreate

    가장 먼저 실행되는 훅   
    
    아직 데이터나 이벤트가 세팅되지 않은 시점이라 접근 불가능

### created

    데이터, 이벤트가 활성화되어 접근 가능   
    
    아직 템플릿과 virtual DOM은 마운트 및 렌더링되지 않은 상태

    * 데이터 초기화 시 사용
    * 부모 → 자식 순으로 훅 실행

## Mounting

**DOM 삽입 단계**

초기 렌더링 직전 컴포넌트에 직접 접근 가능   
컴포넌트 초기에 세팅되어야할 데이터들은 created에서 사용하는 게 나은 방법

### beforeMount

    템플릿이나 렌더 함수들이 컴파일된 후에, 첫 렌더링이 일어나기 직전에 실행됨  

    많이 사용하지 않음

### mounted

    컴포넌트, 템플릿, 렌더링된 DOM에 접근 가능  

    모든 화면이 렌더링된 후에 실행

    * 뷰가 아닌 라이브러리를 통합할 때 사용
    * 자식 → 부모 순으로 훅 실행

#### ❗주의
부모는 자식의 mounted 훅이 끝날 때까지 기다림  
부모의 mounted가 자식의 mounted보다 먼저 실행되지 않음

## Updating

**Diff 및 Re-rendering 단계**

컴포넌트에서 사용되는 반응형 속성들이 변경되거나 다시 렌더링되면 실행됨  
디버깅을 위해 컴포넌트가 다시 렌더링되는 시점을 알고 싶을 때 사용 가능  

컴포넌트의 반응형 속성들이 언제 변경되는지 알아야 하는 경우에는 computed나 watch 사용

### beforeUpdate

    컴포넌트의 데이터가 변해 업데이트 사이클이 시작될 때 실행됨

    (돔이 재렌더링되고 패치되기 직전 상태)

    컴포넌트가 렌더링되기 전에 반응형 데이터의 신규 상태를 가져와야 하는 경우 사용

### updated

    컴포넌트의 데이터가 변해 다시 렌더링된 이후에 실행됨

    property가 변경된 후 DOM에 접근해야할 때 사용

    업데이트가 완료된 상태라, DOM 종속적인 연산이 가능

## Destruction

**해체 단계**

컴포넌트가 해체되고 DOM에서 제거될 때 실행됨

### beforeDestroy

    해체되기 직전에 호출됨

    이벤트 리스너를 제거하거나 reactive subscription을 제거하고자 할 때 유용

### destroyed

    해체된 이후에 호출됨

    Vue 인스턴스의 모든 디렉티브가 바인딩 해제되고 모든 이벤트 리스너가 제거됨

## +++

### computed

템플릿에 데이터 바인딩할 수 있음

```html
<div id="example">
    <p>원본 메시지: "{{ msg }}"</p>
    <p>역순 메시지: "{{ reversedMsg }}"</p>
</div>
```

```js
new Vue({
    el: '#example',
    data: {
        msg: '안녕하세요'
    },
    computed: {
        // 계산된 getter
        reversedMsg: function() {
            // `this`는 vm 인스턴스를 가리킴
            return this.msg.split('').reverse().join('')
        }
    }
})
```
→ msg 값이 바뀌면, reverMsg 값도 따라 바뀜

`Date.now()`와 같이 의존할 곳없는 computed 속성은 업데이트 안됨

```js
computed: {
    now: function() {
        return Date.now()  // 업데이트 불가능
    }
}
```
→ 호출할 때마다 변경된 시간을 이용하고 싶으면 methods 이용

### watch

데이터가 변경되었을 때 호출되는 콜백함수를 정의  

watch는 감시할 데이터를 지정하고, 그 데이터가 바뀌면 어떤 함수를 실행하라는 방식으로 진행

### computed vs watch

```js
// computed
new Vue({
    el: '#demo',
    data: {
        firstName: 'Fu',
        lastName: 'Bao'
    },
    computed: {
        fullName: function() {
            return this.firstName + ' ' + this.lastName
        }
    }
})
```
computed는 선언형
```js
// watch
new Vue({
    el: '#demo',
    data: {
        firstName: 'Fu',
        lastName: 'Bao',
        fullName: 'Fu Bao'
    },
    watch: {
        firstName: function(val) {
            this.fullName = val + ' ' + this.lastName
        },
        lastName: function(val) {
            this.fullName = this.firstName + ' ' + val
        }
    }
})
```
watch는 명령형 프로그래밍 방식

watch 사용하면 API 호출하고, 그 결과에 대한 응답을 받기 전 중간 상태 설정 가능하나 computed는 불가능

> 데이터 변경의 응답으로 비동기식 계산이 필요한 경우나 시간이 많이 소요되는 계산 시에는 watch 사용이 좋으나 대부분의 경우 computed 사용이 더 좋음
