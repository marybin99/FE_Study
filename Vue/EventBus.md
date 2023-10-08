# EventBus
컴포넌트 간 이벤트 주고받기

경우에 따라 서로 관련없는 독립적인 컴포넌트끼리 이벤트를 통신해야할 상황 발생. vuex라는 상태 관리 라이브러리가 있지만, 규모가 작을 경우 EventBus 활용해 컴포넌트 간 통신 가능.  
**컴포넌트 간 메스드 호출** 시 활용. 

## 기본 구성
```js
// main.js
import Vue from 'vue';
import App from './App.vue';

// EventBus 생성
Vue.prototype.$EventBus = new Vue();

new Vue({
    ...
    render: h => h(App)
}).$mount('#app');
```

## 활용
A 컴포넌트 버튼 클릭 시, B 컴포넌트 메서드 실행

> A 컴포넌트
```html
<template>
    ...
    <button @click = "onAClick">버튼</button>
    ...
</template>
...
```

```js
method: {
    onAClick() {
        this.$EventBus.$emit('fetchData');
    }
}
```

> B 컴포넌트
```html
<template>
    ...
</template>
...
```

```js
created() {
    this.$EventBus.$on('fetchData', () => {
        this.fetchData();
    })
},
```

A 컴포넌트에서 버튼 클릭하여 EventBus로 emit 실행  
→ B컴포넌트에선 리스너로 실행  

- 파라미터를 전송해야한다면 ?
```js
// call
this.$EventBus.$emit('fetchData', param);

// listen
this.$EventBus.$on('fetchData',(param) => {
    this.fetchData(param);
})                           
```

