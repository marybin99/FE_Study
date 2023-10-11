# v-bind:is

컴포넌트를 동적으로 변경시킬 때 사용

```js
var vm = new Vue({
    el: '#example',
    data: {
        currentView: 'home'
    },
    components: {
        home: { /* ... */},
        posts: { /* ... */},
        archive: { /* ... */},
    }
})
```
`components`에 사용할 컴포넌트 목록 추가  

```html
<component v-bind:is="currentView">
    <!-- vm.currentView가 변경되면 컴포넌트가 변경됨 -->
</component>
```

초기 값이 `home`으로 되어 있어서 home 컴포넌트가 노출됨.
