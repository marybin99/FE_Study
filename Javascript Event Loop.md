# Javascript Event Loop

## Javascript Engine ?
기본적으로 하나의 쓰레드에서 동작 (싱글 쓰레드)   
하나의 쓰레드 가짐 → 하나의 stack 가짐 → `동시에 단 하나의 작업만을 할 수 있음`    
V8과 같은 엔진이 이용됨
<br>   
크게 세 영역   
> #### Call Stack / Task Queue(Event queue) / Heap   
+&nbsp;`Event loop` : Task queue에 들어가는 task들을 관리

![Alt text](https://github.com/marybin99/CS/assets/110241993/e5f2e9ca-39a9-4371-b768-c9d5fc3c0851)

## Call Stack
단 하나의 호출 스택 사용   
→ js함수가 실행되는 방식을 `Run to Completion`이라고 함   
→ 하나의 함수가 실행되면 이 함수의 실행이 끝날 때까지 다른 어떤 task도 수행될 수 없음

> 요청이 들어올 때마다 해당 요청을 <u>순차적으로</u> 호출 스택에 담아 처리   
  메소드가 실행될 때, Call Stack에 새로운 프레임이 생기고 <b>push</b>되고 메소드의 실행이 끝나면 해당 프레임은 <b>pop</b>되는 원리

```js
function foo(b) {
    var a = 10;
    return a + b;
}

function bar(x) {
    var y = 2;
    return foo(x + y);
}

console.log(bar(1));
```
<img src="https://github.com/marybin99/CS/assets/110241993/c03297d8-704f-4f05-8b53-2d6a712e06cc" width="300"/>

## Heap
동적으로 생성된 객체는 힙에 할당   
대부분 구조화되지 않은 더미같은 메모리 영역

## Task Queue
JS 런타임 환경에서 처리해야 하는 Task들을 임시 저장하는 대기 큐   
Event Queue라고도 함   
Call Stack이 비어졌을 때 먼저 대기열에 들어온 순서대로 수행
```js
setTimeout(function() {
    console.log("first");
}, ());

console.log("second");
```
-&nbsp;결과 -
```shell
console >>
second
first
```
JS에서 비동기로 호출되는 함수들은 Call Stack에 쌓이지 않고 Task Queue에 enqueue   
이벤트에 의해 실행되는 함수들이 비동기로 실행   
Web API 영역에 따로 정의되어 있는 함수들은 비동기로 실행
```js
function test1() {
    console.log("test1");
    test2();
}

function test2() {
    let timer = setTimeout(function() {
        console.log("test2");
    }, ());
    test3();
}

function test3() {
    console.log("test3");
}

test1();
```
<img src="https://github.com/marybin99/CS/assets/110241993/a00bb23d-8753-4420-bc4d-212802a88666" width="350"/>
<img src="https://github.com/marybin99/CS/assets/110241993/2eb2708e-6bfe-4f9e-92c1-ac2e8103b610" width="350"/>

```shell
console >>
test1
test3
test2
```
---
```js
while(queue.waitForMessage()) {
    queue.processNextMessage();
}
```
<b>현재 실행중인 태스크가 없는지와 있는지를 반복적으로 확인</b>   
queue에 처리해야할 이벤트가 존재하면 `while-loop` 안으로 들어가서 해당 이벤트를 처리하거나 작업 수행 후 다시 돌아와 새로운 이벤트 존재 유무를 확인   
Event Queue에서 대기하고 있는 Event들은 한 번에 하나씩 Call Stack으로 호출되어 처리
