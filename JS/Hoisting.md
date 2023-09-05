# Hoisting

변수의 정의가 그 범위에 따라 선언과 할당으로 분리되는 것을 의미   
변수가 함수 내에서 정의되었을 경우, 선언이 함수의 최상위로, 함수 바깥에서 정의되었을 경우, 전역 컨텍스트의 최상위로 변경   
`var` 키워드로 선언된 모든 변수 선언은 <b>호이스트</b>됨   

## 선언 (Declaration) / 할당 (Assignment)
끌어올려지는 것

```js
function getX() {
    console.log(X); // undefined
    var x = 100;
    console.log(x); // 100
}
getX();
```

다른 언어의 경우, 변수 x를 출력하려고 했을 때 오류 발생   
BUT, JS는 `undefined`로 넘어감.   
→ `var x = 100;`에서 `var x`를 호이스트하기 때문

- 작동 순서에 맞게 코드 재구성

```js
function getX() {
    var x;
    console.log(x);
    x = 100;
    console.log(x);
}
getX();
```

선언문은 JS 엔진 구동시 제일 최우선으로 해석하여 호이스팅되고,   
<u>할당구문은 런타임 과정에서 이루어져서 호이스팅되지 않음</u>   
<br>
함수 선언문 형태로 정의한 함수의 유효범위는 전체 코드의 맨 처음부터 시작   
함수 선언이 실행부분보다 뒤에 있어도 JS엔진이 함수 선언을 끌어올리는 것을 의미   
함수 호이스팅은 함수를 끌어올리지만 변수의 값은 끌어올리지 않음

```js
foo();
function foo() {
    console.log('hello');
};
// console> hello
```

foo함수에 대한 선언을 호이스팅해 global 객체에 등록 → `hello`가 제대로 출력

```js
foo();
var foo = function() {
    console.log('hello');
};
// console> Uncaught TypeError: foo is not a function
```

함수 리터럴을 할당하는 구조라 호이스팅되지 않음   
→ 런타임 환경에서 `Type Error` 발생

> `선언`은 호이스팅 O, `할당`은 호이스팅 X

## 우선순위
동일한 이름의 변수와 함수를 선언하는 경우 우선 순위 존재
> 변수 선언 << 함수 선언 << 변수 할당

- 함수 선언 << 변수 할당

```js
var msg = 'Hello';

function msg() {
    console.log('msg() Call!');
}

console.log(typeof msg); // string
```
```js
function msg() {
    console.log('msg() Call!');
}

var msg = 'Hello';

console.log(typeof msg); // string
```
변수 할당과 함수 선언의 순서를 변경해도 결과는 동일

<br>

- 변수 선언 << 함수 선언

```js
var msg;

function msg() {
    console.log('msg() Call!');
}

console.log(typeof msg); // function
```
```js
function msg() {
    console.log('msg() Call!');
}

var msg;

console.log(typeof msg); // function
```
순서 변경해도 결과는 동일
