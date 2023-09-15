# lodash

JS에서 필요한 유용한 함수들 제공하는 인기있는 라이브러리   
<br>
`array, collection, date 등` 데이터의 필수적인 구조를 쉽게 다루기 위해 사용   
JS에서 배열 안의 객체들의 값을 <b>handling (배열, 객체 및 문자열 반복 / 복합적인 함수 생성)</b>할 때 유용   
→ JS 코드를 줄여주고, 빠른 작업이 가능하게 해줌. `frontend`환경에서 많이 사용

  
### 왜 lodash일까?
`ㅡ.(변수)` 로 작성할 경우 lodash wrapper로 변수를 감싸게 되면서 해당 변수에 대한 chaining을 시작.   
`_`라는 기호를 이용해서 사용하기 때문에 lodash라고 이름 붙여짐

### 왜 사용할까?
- 브라우저에서 지원하지 않는 성능이 보장되어 있는 다양한 메서드를 가지고 있음
- 퍼포먼스 측면에서 native보다 더 나은 성능을 가짐
- npm이나 기타 패키지 매니저를 통해 쉽게 사용 가능

## lodash method

### array 관련 method

#### findIndex()
> 형식: _.findindex(array, [predicate=.indentity], [thisArg])   
> 출력: index number   
> 배열 내에서 원하는 index를 쉽게 구할 수 있음

```js
var users = [
    { 'name': 'ollie',   'active': false },
    { 'name': 'ricky',   'active': false },
    { 'name': 'lindsey', 'active': true },
];

// 콜백함수를 통해 이름이 ollie인 객체의 index를 반환
_.findIndex(users, function(user) { return user.name == 'ollie'; });
// → 0

// 이름이 ricky이고 active 상태가 false인 처음 일치하는 객체의 index 
_.findIndex(users, { 'name': 'ricky', 'active': false });
// → 1

// active 상태가 false인 객체 중 첫번째 index를 반환
_.findIndex(users, ['active', false]);
// → 0

// active 상태가 true인 객체의 index를 반환
_.findIndex(users, 'active');
// → 2
```

#### flatten()
> 형식: _.flatten(arraym[isDeep])   
> 다차원 배열 내의 요소를 출력하는데 편리

```js
// 배열안의 배열 값을 순서대로 나열 (depth를 명시하지 않을 경우 1depth만)
_.flatten([1, [2, 3, [4]]]);
// → [1, 2, 3, [4]]

// 배열안의 배열 값을 깊이와 상관없이 순서대로 나열
_.flatten([1, [2, 3, [4]]], true);
// → [1, 2, 3, 4]
```

#### remove()
> 형식: .remove(array, [predicate=.identity], [thisArg])   
> 출력: 제거된 array   
> 배열 내의 조건에 맞는 요소들을 제거한 후 반환

```js
var array = [1, 2, 3, 4];
var evens = _.remove(array, function(n) {
    return n % 2 == 0; 
});

console.log(array);
// → [1, 3]

console.log(evens);
// → [2, 4]
```

### collection 관련 method

#### every()
> 형식: .every(collection, [predicate=.identity], [thisArg])   
> 출력: boolean 값   
> 배열 안 요소들의 값들은 비교하고 분석하는데 용이

```js
var myFriend = [
    { name: 'msh', active: false },
    { name: 'kym', active: false }
];

// 값 비교 가능
_.every(myFriend, { name: 'msh', active: false });
// → true

// key와 value가 있는지 확인 가능
_.every(myFriend, 'active', false);
// → true

// key에 해당하는 value가 모두 true면 true 반환
_.every(myFriend, 'active');
// → false
```

#### find()
> 형식: .find(collection, [predicate=.identity], [thisArg])   
> find()는 조건을 만족하는 컬렉션에서의 첫번째 요소를 찾는 메소드

```js
var myFriend = [
    { name: 'msh', job: 'developer', age: 28 },
    { name: 'kym', job: 'dancer', age: 27 },
    { name: 'khg', job: 'samsung man', age: 27 },
    { name: 'nyj', job: 'metamong', age: 24 },
    { name: 'psm', job: 'android man', age: 27 },
    { name: 'jkh', job: 'ssafy coach', age: 27 },
];

// 콜백함수가 처음으로 참이 되는 객체를 반환
_.find(myFriend, function(friend) {
  return friend.age < 28;
});
// → { name: 'kym', job: 'dancer', age: 27 }
```

#### filter()
> 형식: .filter(collection, [predicate=.identity], [thisArg])   
> filter()는 특정 조건을 만족하는 모든 요소를 추출하는 메소드

```js
var myFriend = [
    { name: 'msh', job: 'developer', age: 28 },
    { name: 'kym', job: 'dancer', age: 27 },
    { name: 'khg', job: 'samsung man', age: 27 },
    { name: 'nyj', job: 'metamong', age: 24 },
    { name: 'psm', job: 'android man', age: 27 },
    { name: 'jkh', job: 'ssafy coach', age: 27 },
];

// 입력한 object의 key와 value들을 모두 포함하는 객체들을 배열로 반환
_.filter(myFriend, { age: 24, job: 'metamong' });
// → [{ name: 'nyj', job: 'metamong', age: 24 }]


// 입력한 key값이 true인 객체들을 배열로 반환
_.filter(myFriend, friend => friend.age == 24);
// → [[{ name: 'nyj', job: 'metamong', age: 24 }]
```

#### map()
> 형식: .map(collection, [iteratee=.identity], [thisArg])   
> 출력: 계산 결과 배열함수를 실행하고 그 결과를 배열로 반환  
> key값을 입력할 경우 해당 key값들만 간추려서 반환

```js
function square(n) {
    return n * n;
}

_.map([1,2],square);
// → [1, 4]

var myFriend = [
    {'name': 'msh'},
    {'name': 'kym'},
];

.map(myFriend, 'name');
// → ['msh', 'kym']
```

#### forEach()
> 형식: .forEach(collection, [itertee=.identity], [thisArg])   
> 배열의 값마다 함수를 실행시킬 때 용이하게 사용

```js
_([1, 2]).forEach(function(n) {
    console.log(n);
}).value;
// 1
// 2
```

#### includes()
> 형식: _.includes(collection, target, [fromIndex=0])   
> 출력: boolean   
> 해당 collection에 target 값이 있는지 판별

```js
// 배열에 값이 있는지 찾음
_.includes([1, 2, 3], 1);
// → true

// index에 해당 값이 있는지 찾음
_.includes([1, 2, 3], 1, 2);
// → false

// 일치하는 값이 있는지 찾음
_.includes([ 'name': 'msh', 'age': 28 ], 'msh');
// → true

// 일치하는 값이 문자열 안에 있는지 찾음
_.includes('forestin', 'ore');
// → true
```

#### reduce()
> 형식: .reduce(collection, [iteratee=.identity], [accumulator], [thisArg])

```js
// 첫번재 인자에 대해 배열 내부의 값을 통해 콜백함수를 실행시킨 후 결과값 반환

_.reduce([1, 2], function(total, n) {
    return total + n;
});
// → 3
```
