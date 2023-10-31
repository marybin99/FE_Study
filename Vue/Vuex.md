# Vuex
✔️
Vuejs에서 컴포넌트들의 `상태 관리`를 위한 효율적인 `라이브러리`   

### 왜 사용할까?
vue에서 컴포넌트 간 여러 정보들을 관리하려면 `부모-자식` 간 데이터들을 넘겨주고 받고 해야함.  
실제로는 컴포넌트 간 관계가 매우 복잡한데 그 여러 컴포넌트 파일에서 `data` 속성을 관리해주어야 하는 단점이 있음.   

<div align="center">
    <img src="https://github.com/marybin99/CS/assets/110241993/1949b719-9e48-419b-b6ba-76ef04859239" width="400"/>
</div>
사진과 같이 깊은 관계에서 다른 컴포넌트로 데이터를 보내려면 부모 컴포넌트를 찾아 이벤트를 바인딩시키고 다시 props로 내려주어야 하는 불편함이 있음.   

`vuex`사용 시 단순하게 작업 가능. `store`라는 중앙 저장소에서 데이터 관리가 가능해짐.   

<div align="center">
    <img src="https://github.com/marybin99/CS/assets/110241993/5c32583e-20ba-4bbf-b8f7-a2d2134078b7" width="700"/>
</div>

### Vuex 패턴
<b>State, View, Actions 단방향 패턴 흐름</b>으로 데이터 관리   

<div align="center">
    <img src="https://github.com/marybin99/CS/assets/110241993/0c60c655-ae2a-4f33-8d70-9f73e9b213e5" width="400"/>
</div>

- **State**: 컴포넌트 간 공유하는 데이터 속성 (컴포넌트에서의 `data` 역할)
- **View**: 데이터를 표시하는 화면
- **Actions**: 사용자 입력에 따라 데이터를 변경하는 메서드

## Vuex의 주요 속성
> store 인스턴스 → **state, getters, mutations, actions**

```js
// store.js
import Vuex from 'vuex';

export const store = new Vuex.store({
    state: {},
    getters: {},
    mutations: {}
    actions: {}
});
```
- (state) 여러 컴포넌트에서 사용할 상태 값을 정의하고,   
- (actions) 서버로부터 api통신을 통해 비동기적으로 받아오거나,  
- (mutations) 필요에 따라 상태 값들을 변경하고,  
- (getters) 필요한 컴포넌트에게 실시간으로 전달해줌   

### state
컴포넌트 간 공유하는 데이터들을 관리하는 역할

```js
state: {
    message: "hello! I'm forest_in 🌳";
}
```

정의된 상태 값이 필요한 컴포넌트에서

```html
<!-- component template -->
<template>
    <h2>{{ this.$store.state.message }}</h2>
</template>
```
와 같이 받아올 수 있음

### getters
연산된 `state`에 접근해 데이터를 조작하는 역할 (일종의 computed 역할)

```js
state : {
    cnt : '',
},

getters : {
    upCnt : function(state) {
        return state.cnt++;
    }
}
```

### mutations
`state`값을 변경하는 역할 (일종의 methods 역할)

```js
state : {
    menuList : []
},

mutations : {
    setMenuList: fuction(state, list) {
        return state.menuList.push(list);
    },
}
```

`commit`메서드로 호출 가능 (첫 인자에 해당 메서드 이름, 두번째 인자에 데이터)

```js
this.$states.commit('setMenuList', {
    id: 1,
    menu: '국밥'
});
```

### actions
비동기 처리를 담당. 여러 컴포넌트에서 mutations로 비동기 이벤트가 일어나면 수행 시간 차이로 `state`가 변경될 경우 추적이 어렵기 때문에 `actions`에 비동기 로직들을 선언   
 ex) api 요청, ES6 Async함수(Promise), setTimeout() 등 로직

<br>

`commit`메서드로 mutation 속성 내 메서드에 접근 가능

```js
state : {
    menuList : []
},

mutations : {
    // state 데이터에 삽입
    setMenuList: function(state, list) {
        return state.list = list;
    }
},

actions : {
    // api 요청
    requestMenuList: function(state) {
        axios.get('/category/list').then(function(response) {
            state.commit('setMenuList', response.data.items);
        });
    }
}
```

컴포넌트에서 호출 시엔 `dispatch`메서드로 접근

```js
// vue-component
created() {
    this.$store.dispatch('requestMenuList');
}
```
