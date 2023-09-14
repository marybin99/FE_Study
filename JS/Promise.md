# Promise

하나의 작업을 콜백으로 결과를 받은 뒤 순차적으로 다음 작업을 진행하려 할 때 콜백 중첩, 즉 <b>콜백 지옥</b>을 만나게 됨   

```js
async(1, function() {
    async(2, function() {
        async(3, function() {
            async(4, function() {
                async(5, function() {
                    console.log("작업완료?????");
                });
            });
        });
    });
});
```

콜백 지옥을 해결하기 위해 `Promise`패턴 사용   
- 비동기 작업들을 순차적으로 진행하거나 병렬로 진행
- 컨트럴이 보다 수월해짐
- 코드의 가독성이 좋아짐
- 내부적으로 예외처리에 대한 구조가 탄탄해서 오류 발생 시 오류 처리 등에 대해 가시적으로 관리 가능

```js
// Promise 선언
var _promise = function(param) {
    return new Promise(function(resolve, reject) {

        // 비동기 표현위해 setTimeout 함수 사용
        window.setTimeout(function() {

            // 파라미터 = true,
            if(param) {

                // 해결됨
                resolve("해결 완료!");
            }

            // 파라미터 = false,
            else {

                // 실패
                reject("실패!!");
            }
        }, 3000);
    });
};

// Promise 실행
_promise(true)
.then(function(text) {
    // 성공 시
    console.log(text);
}, function(error) {
    // 실패 시
    console.error(error);
});
```
> 해결 완료

## Promise 선언부
Promise는 <b>"지금은 없는데 이상없으면 이따가 주고 없으면 알려줄게~!"</b>라는 약속   

### Promise의 상태 (state)
- pending
    - 아직 약속을 수행 중인 상태 (fulfilled / reject되기 전)
- fulfilled
    - 약속이 지켜진 상태
- rejected
    - 약속이 어떤 이유에서 못 지켜진 상태
- settled
    - 약속이 지켜졌든 안지켜졌든 일단 결론이 난 상태

```js
// Promise 선언
var _promise = function(param) {
    return new Promise(function(resolve, reject) {

        // 비동기 표현위해 setTimeout 함수 사용
        window.setTimeout(function() {

            // 파라미터 = true,
            if(param) {

                // 해결됨
                resolve("해결 완료!");
            }

            // 파라미터 = false,
            else {

                // 실패
                reject("실패!!");
            }
        }, 3000);
    });
};
```

> new Promise로 Promise가 생성되는 직후부터 resolve나 reject가 호출되기 전까지의 순간을 pending 상태   
이후 비동기 작업이 마친 뒤 결과물을 약속대로 잘 줄 수 있다면 첫 번째 파라미터로 주입되는 resolve 함수를 호출하고, 실패라면 두 번째 파라미터로 주입되는 reject 함수를 호출한다는 것이 promise의 주요 개념

## Promise 실행부

```js
// Promise 실행
_promise(true)
.then(function(text) {
    // 성공 시
    console.log(text);
}, function(error) {
    // 실패 시
    console.error(error);
});
```

_promise()를 호출하면 Promise 객체가 리턴됨.   
하나의 then API를 호출해서 비동기 작업이 완료되면 결과에 따라 성공 / 실패 메시지를 콘솔로그에 찍어주게 됨.   
<br>
then API는 첫번째 파라미터에 성공 시 호출할 함수, 두번째 파라미터에 실패 시 호출할 함수를 선언하면 상태에 따라 수행   

## Promise API
### Promise.catch API
체이닝 형태로 연결된 상태에서 비동기 작업이 중간에 에러가 나면 처리해주는 구문   
`.then(null, function(){})`을 메서드 형태로 바꾼 것

```js
_promise(true)
    .then(JSON.parse) // - ①
    .catch(function() {
        window.alert('체이닝 중간에 에러가!?');
    })
    .then(function(text) {
        console.log(text);
    });
```
_promise에서 만든 객체는 성공 / 실패 시 JSON 객체가 아닌 String을 리턴해서 ①에서 Error가 남. 다음 then으로 이동하지 못하고 catch에서 받게 됨.   
→ catch는 이와같이 <b>promise가 연결되어 있을 때 발생하는 오류를 처리해주는 역할</b>

#### HTML5Rocks에서 보인 좋은 예제
```js
asyncThing1()
    .then(function() { return asyncThing2();})
    .then(function() { return asyncThing3();})
    .catch(function(err) { return asyncRecovery1();})

    .then(function() { return asyncThing4();}, function(err) { return asyncRecovery2();})
    .catch(function(err) { console.log("Don't worry about it");})

    .then(function() { console.log("All done!");});
```
<img src="https://github.com/marybin99/CS/assets/110241993/f3f8214f-e2ac-425b-875e-01c6699c2164" width="400"/>   

### Promise.all API
여러 개의 비동기 작업이 존재하고 이들이 모두 완료되었을 때 작업을 실행할 때 사용   

```js
var promise1 = new Promise(function (resolve, reject) {

    // 비동기 표현 위해 setTimeout 함수 사용
    window.setTimeout(function() {

        // 해결됨
        console.log("첫번째 Promise 완료");
        resolve("11111");
    }, Math.random() * 20000 + 1000);
});

var promise2 = new Promise(function (resolve, reject) {

    // 비동기 표현 위해 setTimeout 함수 사용
    window.setTimeout(function() {

        //해결됨
        console.log("두번째 Promise 완료");
        resolve("22222");
    }, Math.random() * 10000 + 1000);
});

Promise.all([promise1, promise2]).then(function(values) {
    console.log("모두 완료됨", values);
});
```

<img src="https://github.com/marybin99/CS/assets/110241993/dc976813-22a9-4dc1-85a9-3e9096842b5e" width="300"/>  

## return 형태 vs new Promise 할당

### return 형태

```js
var _promise = new Promise(function(resolve, reject) {
    
    // 여기에서는 무엇인가 수행

    // 50% 확률로 resolve
    if(+new Date()%2 == 0) {
        resolve("Stuff worked!");
    }
    else {
        reject(Error("It broke"));
    }
});
```
이처럼 선언할 경우 Promise 객체에 파라미터로 넘겨준 익명함수는 즉각 실행되어 `_promise.then(alert)` 등의 형태로 사용 가능. 이미 수행되어서 계속해서 같은 결과가 나올 것.

### new Promise 할당
```js
new Promise(function(resolve, reject) {

    // 50프로 확률로 resolve
    if(+new Date()%2 == 0) {
        resolve("Stuff worked!");
    }
    else {
        reject(Error("It broke"));
    }
}).then(alert).catch(alert);
```

Promise 객체를 new로 바로 생성할 경우, 위처럼 사용 가능

### return 형태로 변환 예제

Promise.all 예제를 return 형태로 바꿀 경우
```js
var promise1 = function () {

    return new Promise(function (resolve, reject) {

        // 비동기 표현 위해 setTimeout 함수 사용
        window.setTimeout(function() {

            // 해결됨
            console.log("첫번째 Promise 완료");
            resolve("11111");
        }, Math.random() * 20000 + 1000);
    });
};

var promise2 = function() {

    return new Promise(function (resolve, reject) {

        // 비동기 표현 위해 setTimeout 함수 사용
        window.setTimeout(function() {

            //해결됨
            console.log("두번째 Promise 완료");
            resolve("22222");
        }, Math.random() * 10000 + 1000);
    });
};


// Promise.all([promise1, promise2]).then(function(values) {
//     console.log("모두 완료됨", values);
// });
// → Promise 객체가 아니라 오류 발생, 아래처럼 실행
Promise.all([promise1(), promise2()]).then(function(values){
    console.log("모두 완료됨", values);
});
```
