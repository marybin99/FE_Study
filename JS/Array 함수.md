# Array 함수

## 배열에 값 추가

### .push()
배열의 맨 끝에 값을 추가

### .unshift()
배열의 맨 앞에 값을 추가

## 배열에 값 제거

### .pop()
배열의 맨 끝에 값을 제거

### .shift()
배열의 맨 앞에 값을 제거

### 예제
```js
var arr1 = ['a', 'b', 'c'];
arr1.push('d');
console.log('push: ' + arr1.join('/') + '\n');

var arr2 = ['a', 'b', 'c'];
arr2.pop();
console.log('pop: ' + arr2.join('/') + '\n');

var arr3 = ['a', 'b', 'c'];
arr3.unshift('d');
console.log('unshift: ' + arr3.join('/') + '\n');

var arr4 = ['a', 'b', 'c'];
arr4.shift();
console.log('shift: ' + arr4.join('/') + '\n');
```

```text
push: a / b / c / d
pop: a / b
unshift: d / a / b / c
shift: b / c
```