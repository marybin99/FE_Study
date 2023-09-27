# Vue Spinner

## 스피너
데이터를 불러오는 동안 유저에게 로딩바를 표시하는 기능

### 1. 스피너 컴포넌트
- 스피너 UI를 표시하는 컴포넌트 생성
- props로 `boolean`인 loading을 받아와 로딩바 표시

```html
<template>
    <div class="lds-facebook" v-if="loading">
      <div></div>
      <div></div>
      <div></div>
    </div>
</template>

<script>
export default {
    props: {
        loading: {
            type: Boolean,
            required: true,
        },
    },
}
</script>

<style scoped>
...
</style>
```

### 2. 이벤트 버스
- 로딩 상태를 전역적으로 관리하기 위해 이벤트 버스 사용
- `버스` → 빈 이벤트 객체를 만들어서 컴포넌트 간의 데이터를 전달하는 것

```js
// util/bus.js
import Vue from 'vue';

export default new Vue();
```

#### export default 와 export
```js
// export default
export default new Vue();
//  import 할 때 불러오는 곳에서 이름 정할 수 있음
import buid from './bus.js';
```
```js
// export
export const bus = new Vue();

import { bus } from './bus.js';
```

### 3. 이벤트 버스를 활용한 스피너 구현
- 최상위의 컴포넌트에서 스피너 등록
- 스피너의 상태를 최상위 컴포넌트에서 관리
    - 이벤트 버스를 감지하여 로딩 상태 관리
- 이벤트 버스 등록은 `created` 때 하기
- 이벤트 버스는 `beforeDestroy`에서 반드시 하기
    - 이벤트 객체가 계속 쌓이는 것을 방지

#### 최상위 컴포넌트
```html
<template>
    <div id="app">
        ...
        <!-- 스피너 -->
        <spinner :loading="loadingStatus"></spinner>
    </div>
</template>

<script>
...
import Spinner from './components/Spinner.vue';
import bus from './utils/bus.js';

export default {
    components: {
        ...
        Spinner,
    },
    data() {
        // 로딩 상태
        return {
            loadingStatus: false,
        };
    },
    methods: {
        // 로딩 상태를 바꾸는 메서드
        startSpinner() {
            this.loadingStatus = true;
        },
        endSpinner() {
            this.loadingStatus = false;
        }
    },
    created() {
        // start:spinner 이벤트를 받아서 startSpinner 메서드 실행 (로딩중)
        bus.$on('start:spinner', this.startSpinner);
        // end:spinner 이벤트를 받아서 endSpinner 메서드 실행 (로딩끝)
        bus.$on('end:spinner', this.endSpinner);
    },
    beforeDestroy() {
        // 이벤트 버스 off 처리
        // 이벤트 객체가 계속 쌓이는 것을 방지
        bus.$off('start:spinner', this.startSpinner);
        bus.$off('end:spinner', this.endSpinner);
    }
}
</script>

<style>
...
</style>
```

#### 하위 컴포넌트
- 데이터를 불러오는 하위 컴포넌트

```html
<template>
    <div>
        <list-item></list-item>
    </div>
</template>

<script>
import ListItem from '../components/ListItem.vue';
import bus from '../utils/bus.js';

export default {
    components: {
        ListItem,
    },
    created() {
        // 스피너 이벤트 전송 (보내는 곳)
        bus.$emit('start:spinner');
        this.$store.dispatch('FETCH_NEWS')  // 프로미스 체이닝
            .then(() => {
                // 데이터를 불러온 후 스피너 없애기
                bus.$emit('end:spinner');
            })
            .catch((error) => {
                console.log(error);
            });
    },
}
</script>
```
