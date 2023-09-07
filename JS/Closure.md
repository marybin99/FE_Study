# Closure
두 개의 함수로 만들어진 환경으로 이루어진 특별한 객체의 한 종류   
> *&nbsp;환경: 클로저가 생성될 때 그 범위에 있던 여러 지역 변수들이 포함된 `context`   

클로저를 통해 JS에는 없는 비공개 (private) 속성/메소드, 공개 속성/메소드를 구현할 수 있는 방안 마련가능

## 클로저 생성
> 1. 내부 함수가 익명 함수로 되어 외부 함수의 반환값으로 사용
> 2. 내부 함수는 외부 함수의 실행 환경(execution environmment)에서 실행
> 3. 내부 함수에서 사용되는 변수는 외부 함수의 변수 스코프에 있음

```js
function outer() {
    var name = `closure`;
    function inner() {
        console.log(name);
    }
    inner();
}
outer();
// console> closure
```

`outer`함수를 실행시키는 `context`에는 `name`이라는 변수가 존재하지 않음. 

```js
var name = `Warning`;
function outer() {
    var name = `closure`;
    return function inner() {
        console.log(name);
    };
}

var callFunc = outer();
callFunc();
// console> closure
```

<b>위 코드에서 `callFunc`를 클로저라 함</b>   
`callFunc` 호출에 의해 `name`값이 콘솔에 찍히는 데, `Warning`이 아니라 `closure`가 찍힘   
→ `outer`함수의 `context`에 속해있는 변수 참조. `outer`함수의 지역변수로 있는 `name`변수를 `free variable(자유변수)`라고 함

<br>

> 클로저 : 외부 함수 호출이 종료되도 외부 함수의 지역 변수 및 변수 스코프 객체의 체인 관계를 유지할 수 있는 구조   
          » ✨ 외부 함수에 의해 반환되는 내부 함수를 가리키는 말
