# v-deep
✔️  
## 컴포넌트의 scoped 선언
`vuetify`에서 제공되는 컴포넌트의 스타일 지정이 뜻대로 되지 않을 때가 있음. 이럴 때 `scoped` 선언 없이 스타일을 덮어 씌우면 해결되지만 전역적으로 스타일에 영향을 줘서 좋지 않은 방법임

```html
// 🤨👎
<style>
    .className { /** */ }
</style>
```
그렇다면 어떻게 `scoped`를 해치지 않고 스타일을 지정할까?  

`vue`에서 제공하는 **딥 셀렉터**인 `v-deep` 활용

## v-deep으로 컴포넌트 스타일 변경
3가지 방식으로 사용 가능

```html
// #1
<style scoped>
    .a >>> .b { /** */ }
</style>

// #2
<style scoped>
    .a::v-deep .b { /** */ }
</style>

// #3
<style scoped>
    .a /deep/ .b { /** */ }
</style>
```

※ `#1` : `sass`와 같은 일부 CSS 전처리 환경에서는 `>>>`가 올바르게 동작하지 않을 수 있음  

## v-deep으로 v-html 스타일 다루기
`v-html`로 선언된 HTML 마크업은 `scoped` 스타일 적용이 안됨 → `v-deep` 사용해 적용 가능  

```html
<template>
    <div>
        <div v-html="htmlData" />
    </div>
</template>

<script lang="ts">
//...

export default class Index extends Vue {
    private htmlData = '<p class="html_data">Hello World!</p>'
}
</script>

<style lang="scss" scoped>
div::v-deep .html_data {
    color: green;
}
</style>
```
