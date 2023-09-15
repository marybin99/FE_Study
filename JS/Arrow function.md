# Arrow Function

기존의 function 표현방식보다 간결하게 함수 표현 가능.   

### 제한점
- this, arguments, super에 대한 자체 바인딩이 없음
- methods로 사용해서는 안됨
- new.target 키워드가 없음
- 일반적으로 스코프 지정 시 사용하는 call, apply, bind methods 사용 불가
- 생성자(Constructor)로 사용 불가
- yield를 화살표 함수 내부에서 사용 불가

## 도입 영향

### 짧은 함수

기존의 function을 생략 후 `=>`로 대체 표현

```js
var features = [
    'eyes',
    'nose',
    'mouth',
    'ears'
];

features.map(function(feature) {
    return feature.length;
});
// → [4, 4, 5, 4]

features.map((feature) => {
    return feature.length;
});
// → [4, 4, 5, 4]

features.map(({length}) => length);
// → [4, 4, 5, 4]
```

### 상위 스코프 this

일반 함수에서 this는 자기 자신을 this로 정의하지만 화살표 함수 this는 Person의 this와 동일한 값을 가짐.

```js
function Person() {
    this.age = 0;

    setInterval(() => {
        this.age++;  // this는 person 객체를 참조
    }, 1000);
}

var p = new Person();
```
