# this

모든 함수는 실행될 때마다 함수 내부에 `this`라는 객체가 추가됨   
`arguments`라는 유사 배열 객체와 함께 함수 내부로 암묵적으로 전달되는 것   

## Ⅰ. 객체의 메서드를 호출

객체의 프로퍼티가 함수일 경우 메서드라 부름. `this`는 함수를 실행할 때, 함수를 소유하고 있는 객체를 참조.   
→ 해당 메서드를 호출한 객체로 바인딩. `A.B`일 때 `B` 함수 내부에서의 `this`는 `A`를 가리킴

```js
var myObject = {
    name: "foo",
    sayName: function() {
        console.log(this);
    }
};
myObject.sayName();

//console> Object {name: "foo", sayName: sayName()}
```

## Ⅱ. 함수를 호출

특정 객체의 메서드가 아니라, 함수를 호출하면 해당 함수 내부에 사용된 `this`는 <b>전역객체에 바인딩</b>됨

```js
var value = 100;
var myObj = {
    value: 1,
    func1: function() {
        console.log(`func1's this.value: ${this.vaule}`);

        var func2 = function() {
            console.log(`func2's this.value: ${this.value}`);
        };
        func2() ;
    }
};

myObj.func1();
// console> func1's this.value: 1
// console> func2's this.value: 100
```

`func1`의 this는 <b>[Ⅰ](#ⅰ-객체의-메서드를-호출)</b>와 같음. `myObj`가 `this`로 바인딩되고 `myObj`의 value인 1이 console에 찍히게 됨.   
`func2`는 <b>[Ⅱ](#ⅱ-함수를-호출)</b>임. `A.B`구조에서 A가 없기 때문에 함수 내부에서 `this`가 전역 객체를 참조하게 되고 value는 100이 됨.

## Ⅲ. 생성자 함수를 통해 객체 생성

`new`키워드로 생성자 함수를 호출할 때는 또 다르게 바인딩됨. `new`키워드로 호출된 함수 내부에서의 `this`는 객체 자신이 됨. 생성자 함수를 호출할 때의 `this`바인딩은 생성자 함수가 동작하는 방식으로 이해 가능함.   
<br>
`new`연산자로 함수를 생성자로 호출 → 빈 객체 생성 & this 바인딩   
(이 객체는 함수를 통해 생성된 객체, 부모인 프로토타입 객체와 연결되어 있음 / return 문이 명시되지 않은 경우, `this`로 바인딩된 새로 생성한 객체가 리턴)

```js
var Person = function(name) {
    console.log(this);
    this.name = name;
};

var foo = new Person("foo"); // Person
console.log(foo.name); // foo
```

## Ⅳ. apply, call, bind를 통한 호출 

`this`를 JS 코드로 주입 혹은 설정할 수 있는 방법   
`func2` 호출 시, `func1`에서의 this를 주입하기 위해 위 세 메소드 사용. 차이를 두기 위해 `func2`에 파라미터 받도록 수정

- `bind` 메소드 사용

```js
var value = 100;
var myObj = {
    value: 1,
    func1: function() {
        console.log(`func1's this.value: ${this.value}`);

        var func2 = function(val1, val2) {
            console.log(`func2's this.value ${this.value} and ${val1} and ${val2}`);
        }.bind(this, `param1`, `param2`);
        func2();
    }
};

myObj.func1();
// console> func1's this.value: 1
// conosle> func2's this.value: 1 and param1 and param2
```

- `call` 메소드 사용

```js
var value = 100;
var myObj = {
    value: 1,
    func1: function() {
        console.log(`func1's this.value: ${this.value}`);

        var func2 = function(val1, val2) {
            console.log(`func2's this.value ${this.value} and ${val1} and ${val2}`);
        };
        func2.call(this, `param1`, `param2`);
    }
};

myObj.func1();
// console> func1's this.value: 1
// conosle> func2's this.value: 1 and param1 and param2
```

- `apply` 메소드 사용

```js
var value = 100;
var myObj = {
    value: 1,
    func1: function() {
        console.log(`func1's this.value: ${this.value}`);

        var func2 = function(val1, val2) {
            console.log(`func2's this.value ${this.value} and ${val1} and ${val2}`);
        };
        func2.apply(this, [`param1`,`param2`]);
    }
};

myObj.func1();
// console> func1's this.value: 1
// conosle> func2's this.value: 1 and param1 and param2
```

- `bind` vs `apply`, `call`   
    > `bind`는 함수 선언 시, `this`와 파라미터 지정   
      `call`과 `apply`는 함수 호출 시 `this`와 파라미터를 지정해줌

- `apply` vs `bind`, `call`   
    > `apply`는 첫번째 인자로 `this`를 넘겨주고 두번째 인자로 넘겨줘야 하는 파라미터를 배열 형태로 전달   
      `bind`와 `call`는 각각의 파라미터를 하나씩 넘겨주는 형태
