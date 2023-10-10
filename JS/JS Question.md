# 자주 나오는 JS 질문

## 1. 이벤트 위임하기

#### Q. todolist 어플리케이션 제작에서 해당 리스트의 아이템에 대해 사용자가 클릭할 때 이벤트가 일어나도록 구현해보세요.

```html
<ul id="todo-app">
    <li class="item">장보기</li>
    <li class="item">공부하기</li>
    <li class="item">밥먹기</li>
    <li class="item">잠자기</li>
</ul>
```

#### 대부분의 구현 코드

```js
document.addEventListener('DOMContentLoaded', function() {
    let app = document.getElementById('todo-app');
    let items = app.getElementsByClassName('item');

    // 각 아이템에 이벤트 리스너 등록
    for(let item of items) {
        item.addEventListener('click', function() {
            alert('you clicked on item: ' + item.innerHTML);
        });
    }
});
```

제대로 동작하는 코드지만 리스트의 아이템 **각각**에 이벤트를 붙이고 있음.   
요소의 개수가 작으면 상관없지만 요소의 개수만큼 분리된 이벤트 리스너를 생성하고 DOM에 각각 등록하는 비효율적인 코드임.  

#### 효율적인 해결책

모든 아이템 리스트에 대해 한 개의 이벤트 리스너를 생성해 전체 영역에 등록.  
해당 아이템 선택 시 이벤트 리스너가 해당 아이템에 대해서 이벤트 발생  
» `이벤트 위임`이라고 함

```js
document.addEventListener('DOMContentLoaded', function() {
    let app = document.getElementById('todo-app');

    // 리스트 아이템의 전체 영역에 이벤트 리스너 등록
    app.addEventListener('click', function(e) {
        if(e.target && e.target.nodeName === 'LI') {
            let item = e.target;
            alert('you clicked on item: ' + item.innerHTML);
        }
    });
});
```

## 2. 루프에서 클로저 이용하기

> 클로저 : 이너함수가 스코프 밖에 있는 변수에 접근하는 것. 보통 정보은닉을 구현하거나 함수 팩토리 생성 시 사용.

#### Q. 정수 값을 갖는 리스트를 반복문으로 접근해 해당 요소마다 3초를 지연시키고 값을 출력해보세요.

#### 대부분의 구현 코드

```js
const arr = [4, 7, 14, 17];
for(var i = 0; i < arr.length; i++) {
    setTimeout(function() {
        console.log('The index of this number is : ' + i);
    }, 3000);
}
```

위와 같이 구현하면 각 인덱스에서 3초씩 지연된 후 모두 4가 찍힘.  

💥 `setTimeout`함수가 인덱스 i를 반복하는 스코프 밖의 스코프를 갖는 클로저를 생성하기 때문에 3초가 지난 후 클로저가 실행되고 i 값을 출력할 때 반복문의 종료 값인 4를 출력.  
→ `setTimeout`의 스코프와 `for반복문` 안의 스코프가 다르기 때문에 발생 

#### 2가지 해결법

ⅰ)
```js
const arr = [4, 7, 14, 17];
for(var i = 0; i < arr.length; i++) {
    // i 값을 setTime 함수 안에 전달하여 각 함수 호출마다 올바른 값에 접근하게 함
    setTimeout(function(i_local) {
        return function() {
            console.log('The index of this.number is : ' + i_local);
        }
    }(i), 3000);
}
```

ⅱ)
```js
const arr = [4, 7, 14, 17];
for(let i = 0; i < arr.length; i++) {
    // ES6의 let은 함수가 호출될 때마다 인덱스 i 값이 바인딩되는 새로운 바인딩 기법을 사용
    setTimeout(function() {
        console.log('The index of this.number is : '  + i);
    }, 3000);
}
```

## 3. 디바운싱 (Debouncing)

윈도우 크기를 재조정하거나 페이지 스크롤을 내리는 등 매우 짧은 시간에 다수 발생되는 이벤트들에 이벤트 리스너를 달 경우 성능에 심각한 악영향을 줌. 
 
어플리케이션 제작에 대해 논할 때, 스크롤링이나 화면 재조정 그리고 키 눌림 같은 이벤트에 대해서 **페이지 속도와 성능을 향상시키기 위한 `디바운싱(Debouncing)`이나 `쓰로틀링(Throttling)`**을 꼭 짚고 넘어가야 함.

#### 실제 사례
> 2011년에 트위터 웹 사이트에서 **'사용자가 트위터 피드 화면을 내리면 갑자기 굉장히 느려지고 무반응 상태가 되는 문제'** 발생  

John Resig는 스크롤 이벤트에 복잡도가 높은 함수를 직접 붙이는 게 얼마나 위험한 지 설명하는 [블로그 글](https://johnresig.com/blog/learning-from-twitter/)을 올림.

#### 디바운싱
실제로 함수가 다시 호출되기 전까지 시간 간격을 두어 성능 이슈를 해결하는 방법.  
몇 가지 함수 호출을 한 개의 그룹으로 묶고 특정 시간이 지난 후에만 호출될 수 있도록 구조화하는 방식으로 구현. 

#### js로 스코프, 클로저, this, timing 이벤트를 구현한 예제

```js
// 이벤트를 감쌀 디바운싱 함수
function debounce(fn, delay) {
    // 타이머 선언
    let timer = null;

    // 타이머 변수에 접근 가능한 클로저 함수
    return function() {
        // 클로저 함수 안에서 this와 arguments 변수로 디바운싱 함수의 스코프와 변수에 접근
        let context = this;
        let args = arguments;

        // 만약 이벤트가 호출되면 타이머를 초기화하고 다시 시작
        clearTimeout(timer);
        timer = setTimeout(function() {
            fn.apply(context, args);
        }, delay);
    }
}
```
특정 시간 후에만 실행됨

실제 사용 예제)
```js
// 사용자가 스크롤할 때 호출되는 이벤트 함수
function foo() {
    console.log('You are scrolling!');
}

// 이벤트 함수를 디바운싱 함수로 감싸서 2초마다 발생하도록 함
let elem = document.getElementById('container');
elem.addEventListener('scroll', debounce(foo, 2000));
```

❗쓰로틀링 (Throttling)  
함수 호출을 긴 시간 간격으로 발생하게끔 퍼뜨리는 기술  
만약 이벤트가 100ms에 10번 발생한다면, 쓰로틀링으로 각 실행이 100ms 대신 2초로 조정가능
